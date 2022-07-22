# 난수 생성하기

- 다음으로 수행할 작업은 사용자가 예측해야할 난수를 생성하는것이다.
- rust는 이런 rand crate 를 제공한다

## 크레이트를 이용해 필요한 기능 추가

- 크레이트는 소스 파일의 집합
- 프로젝트 진행중인 프로젝트로 실행이 가능한 바이너리 크레이트 이다.
- rand크레이트 또한 라이브러리 크레이트 이며 다른 프로그램에서 재사용할 것을 염두에 두고 작성된 코드를 포함되고 있다.
- cargo.toml파일을 수정해서 rand크레이트를 의존 패키지로 등록해주어야 한다.
- Cargo.toml

```
[depedencies]
rand = "0.6.1"
```

- cargo.toml파일 내에서 헤더 다음에 작성하는 내용들은 다른 섹션의 헤더를 만나기 전까지는 해당 섹션에 속한다.
- [depedencies] 섹션에는 프로그램에 필요한 외부 크레이트의 종류와 버전을 카고에게 알려준다

```
cargo build
```

- 사용하는 시기에 따라 버전 번호가 다를수도 있으며 출력의 결과의 순서도 다를수 있다.
- 프로그램에 외부 의존 패키지를 추가하면 cargo는 패키지가 등록된 저장소에서 가장 최신의 복사본을 내려받는다
- 패키지 저장소의 업데이트가 끝나면 cargo는 [depedencies] 섹션을 완료하고 아직 다운로드되지 않은 크레이트를 다운로드 한다.
- cargo는 src/main.rs의 변경사항만을 빌드한다.

## cargo.lock파일을 이용해 재생산 가능 빌드 구현

- cargo는 다른 누군가가 이 코드를 빌드 하더라도 매번 같은 결과물을 재생산 하기위한 메커니즘이 있다.
- cargo는 무조건 명시된 버전의 의존 패키지만을 사용한다.
- rand패키지의 중요한 버그를 수정하였지만 그로 인해 코드가 컴파일 되지 않는 0.6.2버전이 출시된다면 어떻게 될가
- cargo.lock파일은 cargo build명령을 실행할떄 생성되며 디렉토리에 생성되었다.
- cargo는 프로젝트를 처음 빌드할떄 모든 의존 패키지를 검색 cargo.lock파일에 기록한다 그 다음부터 cargo.lock파일을 조회하고 이전 기록들을 사용한다.
- 명시적으로 업데이트 하지 않는 이상 계속해서 rand 0.6.1버전을 사용할 것이다.

## 새 버전의 크레이트로 업데이트

- cargo는 update라는 명령으로 크레이트를 업데이트 할수 있게 지원한다.
- 이 명령은 cargo.lock파일에 명시된 버전을 무시하고 cargo.toml파일에 지정된 가장 최신 버전을 다시 찾는다.

```
cargo update
```

## 난수 생성하기

```rs
use std::io;
use rand::Rng;

fn main(){
    println!("숫자를 맞혀 봅시다");
    let secrect_number=rand::thread_rng().gen_range(1,101);
    println!("사용자가 맞혀야 할 숫자:{}",secrect_number);
     println!("정답이라고 생각하는 숫자를 입력하세요");
     let mut guess = String::new();
     io::stdin().read_line(&mut guess).expect("입력한 값을 읽지 못했습니다.");
     println!("입력한 값:{}",guess);
}
```

- use rand::Rng구문을 추가했다.
- Rng트레이트는 난수 생성기에 구현된 메서드를 정의하며,이 메서드를 사용하려면 트레이트가 반드시 현재 scope내에 선언되어야 한다.
- rand::thread_rng함수는 우리가 사용할 난수 생성기를 리턴한다.이 함수가 리턴하는 생성기는 현재 코드를 실행중인 스레드 내에 존재하며 운영채제가 지정한 시드값을 이용한다.
- 그런 다음 생성기의 gen_range메서드를 호출한다.
- 이 메서드는 use rand::Rng구문을 이용해 코드의 실행 범위로 불러온 Rng트레이트에 정의 되어 있다.
- gen_range메서드는 인수로 지정된 두값 사이의 난수를 선택해 리턴한다.
- 코드 중간 쯤에 추가한 코드는 생성한 난수를 화면애 출력한다.

```
cargo run
```

## 난수와 사용자의 입력 비교하기

```rs
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main(){
    println!("숫자를 맞혀 봅시다");
    let secrect_number=rand::thread_rng().gen_range(1,101);
    println!("사용자가 맞혀야 할 숫자:{}",secrect_number);
     println!("정답이라고 생각하는 숫자를 입력하세요");
     let mut guess = String::new();
     io::stdin().read_line(&mut guess).expect("입력한 값을 읽지 못했습니다.");
     println!("입력한 값:{}",guess);
     match guess.cmp(&secreat_number){
        Ordering::Less =>println!("입력한 숫자가 작습니다."),
        c =>println!("입력한 숫자가 큽니다."),
        Ordering::Equal =>println!("정답"),

     }
}
```

- use std::cmp::Ordering 타입을 로드한다.Result타입과 마친가지로 Order역시 열거자이며 Less,Greater,Equal증 하나를 선택한다.
- cmp메서드는 두개의 값을 비교하여 비교가 가능한 어떤 값에도 적용할수 있다.
- 또한 비교할 값을 저장한 참조를 인수로 사용하며 guess와 secreat_number를 비교한다.
- 결과로 Ordering열거자의 열것값중 하나로 리턴한다.
- match표현식은 여러개의 가지로 구성된다.가지는 패턴,match표현식의 시작 부분에 주어진 값이 이패턴과 일치할떄 지정된 값을 읽어 패턴과 차례대로 비교한다.
- match표현식은 입력된 값은 50인데 생성한 난수는 38 이라 가정하면,cmp메서드는 50과 38을 비교한 후 50이 38 보다 큰숫자이므로 secreat_number를 리턴한다.
- match 표현식은 Ordering::Greater값을 가져와 각가지의 패턴을 확인한다.일치하지 않으면 무시하고 다음 가지로 넘어간다.
- 만약 일치하게 되면 println!를 실행 후 실행을 종료한다.

```
cargo build
//에러 발생
```

- 에러의 메세지는 타입 불일치 이며 Rust는 타입 추론을 지원한다.
- let guess = String::new()코드를 작성하면 rust 는 String타입이어야 한다는 점을 스스로 이해하고 개발자에게 타입을 지정할 것을 강요하지 않는다.
- 한편 secrect_number변수는 숫자 타입이다.
- 기본적으로 Rust에서는 숫자 값에 대하여 i32타입을 가지는데 별도 타입을 지정해주 않는 이상 Rust는 secrect_number를 i32타입이라 이해한다.
- 숫자타입과 문자열 타입을 비교할수 없기 떄문에 에러가 발생한다.

```rs
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main(){
    println!("숫자를 맞혀 봅시다");
    let secrect_number=rand::thread_rng().gen_range(1,101);
    println!("사용자가 맞혀야 할 숫자:{}",secrect_number);
     println!("정답이라고 생각하는 숫자를 입력하세요");
     let mut guess = String::new();
     io::stdin().read_line(&mut guess).expect("입력한 값을 읽지 못했습니다.");
     let guess:u32 = guess.trim().parse().expect("입력한 값이 올바른 숫자가 아닙니다.")
     println!("입력한 값:{}",guess);
     match guess.cmp(&secreat_number){
        Ordering::Less =>println!("입력한 숫자가 작습니다."),
        c =>println!("입력한 숫자가 큽니다."),
        Ordering::Equal =>println!("정답"),

     }
}
```

- 새로 작성한 코드는 guess라는 새로운 변수를 선언한다.
- rust는 guess변수가 보관하던 값을 새로운 변수의 값으로 가려버린다.
- 새로 선어한 guess변수에는 guess.trim().parse()표현식의 결과를 바인딩한다.
- string타입의 trim메서드는 문자열 양끝의 공백을 제거한다.
- u32타입은 오로지 숫자에 해당하는 문자만 처리할수 있지만 read_line메서드가 사용자의 입력으 읽으려면 사용자가 반드시 엔터키를 입력해야한다
- 사용자가 5를 입력하고 엔터키를 누르면 guess에는 5\n이 저장된다.\n은 줄바꿈을 의미한다.그래서trim메서드를 호출해서 \n을 제거하고 5로 만들어주어야한다.
- 문자열의 parse메서드는 문자열을 구문부석해서 숫자로 변환한다.
- 따라서 let guess:u32처럼 정확히 숫자타입을 알려줘야 한다.
- guess변수 이름 다음 : 은 변수의 타입을 지정한다.
- parse메서드는 에러를 리턴하기도 한다.사용자가 A<%라는 문자열을 입력햇다면 숫자로 변환할 방법이 없다.parse메서드가 문자열을 숫자로 변환할수 없어 Err Result열것값을 리턴하면 expect메서드는 프로그램을 종료하고 에러 메세지를 리턴한다.

```
cargo run
```
