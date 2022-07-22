## panic

- 프로젝트 내에서 결과 바이너리가 가능한 작아지기를 원한다면, 여러분의 Cargo.toml 내에서 적합한 [profile] 섹션에 panic = 'abort'를 추가함으로써 되감기를 그만두기로 바꿀 수 있습니다

```rs
fn main() {
    panic!("crash and burn");
}
```

```
$ cargo run
   Compiling panic v0.1.0 (file:///projects/panic)
    Finished dev [unoptimized + debuginfo] target(s) in 0.25 secs
     Running `target/debug/panic`
thread 'main' panicked at 'crash and burn', src/main.rs:2
note: Run with `RUST_BACKTRACE=1` for a backtrace.
error: Process didn't exit successfully: `target/debug/panic` (exit code: 101)
```

## panic! 백트레이스

```rs
fn main() {
    let v = vec![1, 2, 3];

    v[99];
}

```

```
$ cargo run
   Compiling panic v0.1.0 (file:///projects/panic)
    Finished dev [unoptimized + debuginfo] target(s) in 0.27 secs
     Running `target/debug/panic`
thread 'main' panicked at 'index out of bounds: the len is 3 but the index is
100', /stable-dist-rustc/build/src/libcollections/vec.rs:1362
note: Run with `RUST_BACKTRACE=1` for a backtrace.
error: Process didn't exit successfully: `target/debug/panic` (exit code: 101)
```

## Result

```rs
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

```rs
use std::fs::File;

fn main() {
    let f = File::open("hello.txt");
}
```

```rs
ler f :u32 = File::open("hello.txt"); //->error
```

- match표현식을 이용해 리턴된 Result타입의 리턴값 처리

```rs
use std::fs::File;
fn main(){
    let f = File::open("hello");
    let f = match f{
        Ok(file)=>file,
        Err(error)=>{
            panic!("파일 열기 실패")
        }
    }
}
```

- Option열거자와 마찬가ㄹ지로 Result열거자와 그 열것값도 프렐류드에 의해 자동으로 임포트된 상태이므로 Ok나 Error열것값 앞에 Result::를 명시할 필요가 없다

## match표현식으로 여러 종류의 에러처리

- 만약 File::open메서드가 다른이유로 실패한다면 panic매크로 호출

```rs
use std::fs::file;
use std::io::Errorkind;

fn main(){
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file)=>file,
        Err(ref error)=>match error.kind(){
     ErrorKind::NotFound=>match File::create("hello.txt"){
        Ok(fc)=>fc,
        Err(e)=>panic!("파일을 생성하지 못했습니다:{:}",e),

     },
     other_error=>panic!("파일을 열지 못햇습니다:{:?}",other_erorr).
        }.
    };
}
```

- File::open메서드가ㅜ 리턴하는 Err열것값에 저장된 값의 타입은 표준라이브러리에 정의된 구조체는 io::Error타입이다.
- 이 구조체는 io::ErrorKind타입을 리턴하는kind메서드를 제공한다
- 표준라이브러리가 제공하는 io::ErrorKind열거자는 io작업의 결좌로 발생할수 있는 다양한 종류의 오류를 표현한다
- 여기서 우리가 처리해야할 열것값을 존재하지 않는 파일을 열고자하는 상황을 가리키는 ErrorKind::NotFound값이다 따라서 변수 f에대한 match표현식 안에 error.kind()값에 대해 실행할 다른 match표현식을 중첩해야한다
- 안쪽의 match표현식에서는 error.kind()메서드가 리턴한 값이 ErrorKind::NotFound열것값인지 확인한다 만일 열것값이 일치하면 File::create메서드를 이용하여 파일을 생성한다.

## 에러발생시 panic을발생시키는 더 빠른 방법 unwrap과 expect

- match표현식을 사용하는 방법은 의도대로 잘 동작하지만 코드가 길어지며 의도를 항상 정확하게 표현하지 못한다.

- 그래서 Result<T,E>타입은 다양한 작업을 처리하기 위한 여러가지 도움 메서드들을 제공한다.unwrap메서드는 이런 도움 메서드중 하나로 작성햇던 match표현식과 정확히 같은 동작을 하는 단축 메서드이다.
- Rusult타입의 값이 Ok열것값이면 unwrap메서드는 oK열것값에 저장된 값을 리턴한다.

```rs
use std::fs::File;
fn main(){
    let f =File::open("hello.txt").unwrap();

}

```

- hello.text파일이 없는 상태에서 이 코드를 실행하면 unwrap메서드에 의해 panic! 메크로가 호출된 여러 메세지를 볼수 있다.
- 또 다른 메서드은 expect메서드는 unwrap메서드오 유사하지만 panic!메크로에 에러메세지를 전달해 준다
- unwrap메서드 매신 expect메서드를 사용하면 개발자의 의도를 명확히 표현하는 동시에 발생한 원인을 더쉽게 추적할수 있다.

```rs
use std::fs::File;

fn main(){
    let f = File::open("hello.txt").expect("파일을 열수 없습니다");
}
```

## 에러전파하기

- match표현식을 이용해 호출자에게 에러를 리턴하는 함수

```rs
use std::io;
use std::io::read;
use std::fs::File;

fn read_username_from_file()->Resulr<String,io::Error>{
    let f= File::open("hello.txt");

    let mut f = match f {
        Ok(file)=>file,
        Err(e)=>return Err(e),
    };
    let mut s = String::new();

    match f.read_to_string(&mut s){
        Ok(_)=>Ok(s),
        Err(e)=>Err(e),
    }
}

```

- 이 함수는 더 짧게 작성할수 있지만 에러 처리 방법을 보기 위해 많은 부분 직접 해결하고 있다.
- 이함수의 리턴타입은 Resulit<String,io:Error>이다.즉 함수는 Result<T,E>타입을 리턴하며 제네릭 매개변수 T는 String으로 제네릭 매개변수 E는 io::Error으로 각각 지정되어 있다.
- 이함수가 아무 문제 없이 실행되면 이 함수를 호출한 코드는 파일에서 읽은 사용자 이름을 저장한 String값이포함된 Ok열것값을 리턴받는다.

- 함수의 본문은 File::open메서드를 호출하는 것으로 시작한다.에러를 발생했을떄는 단순히 panic! 매크로를 호출하는 것이 아니라 이 함수의 실행을 조기에 중단하고,File::open메서드가 리턴한 에러값을 이 함수의 에러값으로 호출자에게 리턴한다.File::open메서드호출이 성공하면 파일 핸들을 변수 f에 할당한 후 나머지 코드를 실행한다.

- read_to_String메서드 호출은 File::open메서드호출이 성공했더라도 파일 내용일 읽는 과정에서 실패할 가능성이 있기 떄문에 마찬가지로 Result타입을 리턴한다.

## ?연산자를 이용해 에러를 더 쉽게 전파하기

- 연사자를 이용해 호출자에게 에러를 리턴하는 함수

```rs
use std::io;
use std::uo::Read;
use std::fs::File;

fn read_username_from()->Result<String,io::Error>{
    let mut f = File::open("hello.txt")?;
    let mut s = String::new();
    f.read_to_string(&mut)?;
    Ok(s)
}
```

- ?연산자는 Result타입을 처리하기 위해 사용햇던 match표현식과거의 같은 방식으로 동작한다.
- Result값이 ok면 Ok열것값에 저장된 값이 리턴되며 프로그램이 계속해서 실행된다.만일 이값이 Err이면 Err열것값이 return키워드를 사용한 것처럼 전체 함수의 리턴값이 호춯자로 에러가 전파된다.
- match표현식과 ?연산자의 동작에는 한가지 차이점이 있다.
- ?연산자의 경우 에러값은 from함수를 이용해 전달된다. 이 함수는 표준 라이브러리에 정의된 From트레이드에 선언되어 있으며 에러를 어떤 한 타입에서 다른 타입으로 변환한다.
- ?연산자가 form함수를 호출하면 전달된 에러 타입은 현재 함수의 리턴타입에 정의된 에러 타입으로 변환된다.
- File::Open메서드 호출 다음의? 연산자는 Ok열것값 안에 저장된 값을 변수 f 에 리턴한다
- ?연산자를 이용하면 거추장스러운 코드를 모두 제거하고 함수를 더 간결하게 유지할수 있다.

```rs
use std::io;
use std::io::Read;
use std::fs::File;

fn read_username_from_file()->Result<String,io::Error>{
    let mut s = String::new();
    File::open("hello.txt")?.read_to_String(&mut s )?;
    Ok(s)
}
```

- 변수f를 선언하는 대신 File::open("hello.txt")의 결과에 곧바로 read_to_String(&mut s )?;를 붙엿다
- File::open메서드와 read_to_string메서드 호출 둘다 성공하면 에러를 리턴하는 것이 아니라 사용자 이름을 리턴한다.

```rs
use std::io;
use std::fs;

fn read_username_from_file()->Result<String,io:Error>{
    fs::read_to_string("hello.txt");
}
```

## ? 연산자를 Result타입을 리턴하는 함수에서만 사용

```rs
use std::fs::File;
fn main(){
    let f = File::open("hello.txt");//->error
}
```

- 이 에러는 Result타입을 리턴하지 않는 함수에?연산자를 사용할수 없다는 점을 설명
- Result타입을 리턴하지 않는 함수 안에서 Result타입을 리턴하는 다른 함수를 호출 할떄는 호출자에게 에러를 전파할수 있는 ? 연산자를 사용하기 보다는 match표현식이나
  Result타입의 메서드중 하나를 사용해서 Result를 처리해야한다,

```rs
use std::error::Error;
use std::fs::File;

fn main()->Result<(),Box<dyn Error>>{
    let f =File::open("hello.txt"?;
    Ok(())
}
```

- Box<dyn Error> 타입은 트레이드 객체 라고 부르는 타입이다
- Box<dyn Error> 타입은 모든 종류의 에러를 의미한다.

## 예제,프로토타입 코드 테스트

- Result타입을 리턴하는 로직의 결과가 Ok값을 리턴할 것이 확실하더라도 그로직이 컴파일러 이해할수 없는 것이라면 unwrap메서드를 함께 호출해주는것이 좋디.

```rs
use std::net::IpAddr;
let home:IpAddr= "127.0.0.1".parse().unwrap();
```

- 이 예제에서는 하드코드 문자열을 파싱해서 IpAddr타입의 인스턴스를 생성하고 있다.
  당연히 127.0.0.1은 유효한 IP주소여서 파싱이 실패할 리는 없지만 그래도 unwrap메서드를 호출할수 있따.
  - 하지만 유효한 문자열을 이용한 parse메서드 호출의 리턴 타입은 여전히 Result타입이다 하지만 컴파일러 관점에서는 이 문자열이 항상 유효한 IP주소라는 것을 알수가 없으므로 Result값이 Err인 경우를 처리하도록 강요한다.

## 에러 처리를 위한 조언

- 코드가 결국은 잘못된 상태가 될 상황이라면 패닉을 발생시키는 것도 나쁜 선택은 아니다.
- 잘못된 상태라는 것은 어떤 가설이나 보장,계약 혹은 불변이어야 할 것들이 꺠진 상황을 말한다.
- 즉 유효하지 않은 값 ,모순된 값 ,혹은 실수로 놓친 값이 코드로 전달되는 상황을 말한다.

```
잘못된 상태는 원래 기대햇던 동작이 어쩌다 실패하는상황을 말하는 것이 아니다.
어느 지점 이후의 코드는 프로그램이 절대 잘못된 상태에 놓이지 않아야만 정상적으로 동작한다.
이 정보를 사용중인 타입으로 표현할 방법이 없다
```

## 유효성 검사를 위한 커스텀 타입

```rs
loop{


    let guess:i32 = match guess.trim().parse(){
        Ok(num)=>num,
        Err(_)=>continue,
    };
    if guess<1 || guess >100{
        println!("1에서 100사이의 값을 입력해 주세요");
        continue;
    }
    match guess.cmp(&serect_number);
}
```

- 이 방법은 이상적인 해결책이 아니나 프로그램이 1부터 100사이의 값을 처리해야하는 것이 매우 중요한 조건이고 이조건에 만족해야하는 함수가 많아지면 if문을 통해서 매번검사하는 것은 매우 비효율적이다
- 더 나은 방법은 새로운 타입을 생성하고 이타입의 인스턴스를 생성하는 함수에 유효성 검사 코드를 작성하는것이다.

```rs
pub struct Guess{
    value:i32.
}

impl Guess{
    pub fn new(value:i32)->Guess{
        if value<1 ||value>100{
            panic!("유추한 값은 반드시 1부터 100사이의 값이어야합니다 입력한 값:{}",value);
        }
        Guess{


        value
        }
    }
    pub fn value(&self)->i32{
        self.value
    }
}
```

- i32타입의 value필드를 가지고 있는 구조체 생성
- 그다음 구조체타입을 생성하는 new연관함수 생성
- new함수의본문을 구현한 코드에서는 입력된 값이 1에서 100사이인지를 검사한다.입력값이 범위를 벗어나면 이 값으로 guess타입 인스턴스를 생성하는 것은 Guess::new함수의 계약을 위반하는 것이므로 panic메크로를 호출해서 호출 코드를 작성한 프로그래머가 버그를 수정할수있도록한다.
- Guess::new함수가 패닉을 발생시키는 조건은 반드시 공개된 api문서에 설명해둬야한다
- 입력값이 유혀성 검사를 통과하면 value매개변수의 값을 이용해 새로운 Guess인스턴스를 생성해서 리턴한다.
- 그 다음 코드는 매개변수가 없으며 자신의 값을 대여해 u32타입으 값을 리턴하는 value메서드를 구현한 코드다
- 이 공개 메서드가 필요한 이유는 Guess구조체의 value필드가 비공개 이기때문
- value필드를 비공개로 선언하는 것이 중요한 이유는Guess구조체를 사용하는 코드가 value필드의 값을 직접 조작하는 경우를 방지할수 있어서
-
