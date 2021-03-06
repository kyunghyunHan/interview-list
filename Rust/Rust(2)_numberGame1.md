# 숫자 맞추기 게임 구현

- 초급자용 프로그램 문제
- 1과 100사이의 난수를 생성한다.
- 그런 다음 플레이어가 어떤 값을 예상했는지를 묻는다.
- 플레이어가 예측한 값을 입력하면 프로그램은 생성한 수와비교해 작은지 큰지 알려준다.
- 예측한 값이 일치하면 게임은 메세지를 출력하고 종료한다.

## 새프로그램 Setup

```
$ cargo new guessing_game
$ cd guessing_game
```

```
[package]
name = "guessuing_game"
version = "0.1.0"
authors=["작성자이름<이메일>"]
[dependencies]
```

- src/maim.rs

```rs
fn main(){
    println!("hello,world")
}
```

```
$ cargo run
```

## 플레이어가 예측한 값 처리하기

- 숫자 맞추기 게임을 구현하는 첫번쨰는 플레이어에게 입력할 값을 묻고 이 입력을 처리한 후 그 값이 원하는 형태인지를 확인하는 것이다.

```rs
use std::io

fn main(){
    println!("숫자를 맞혀봅시다");
    println!("정답이라고 생각하는 숫자를 입력하세요");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess).expect("입력한 값을 읽지 못했습니다.");
    println!("입력한 값:{}",guess);
}
```

- 먼저 플레이어가 출력하려면 io 라이브러리를 가져와야한다.(input,output)

```rs
use std::io
```

- Rust는 모든 프로그램의 prelude에서 기본적으로 단 몇개의 타입만을 가져온다.
- 만일 사용하고자 하는 타입이 prelude에 포함되지 않으면 use구문을 통해 명시적으로 프로그램의 범위로 가져와야한다.
- std::io라이브러리는 사용자가 입력한 값을 읽는 기능을 비롯해 여러가지 기능을 제공한다.

```rs
fn main(){}
```

- 새로운 함수를 선언하며 빈 ()는 함수의 매개변수가 없는 것을 의미한다.
- {는 함수 본문의 시작을 의미한다.

```rs
println!("숫자를 맞혀봅시다");
```

- 문자열을 화면에 출력하는 코드

## 변수에 값 지정하기

```rs
let mut guess = String::new();

```

- let은 변수를 생성하는 구문이다.

```
let foo = bar;
```

- 이코드는 새로운 변수 foo 생성후 bar라는 변수의 값을 바인딩한다.
- rust에서 변수는 기본적으로 값을 변경할수 없다.
- 다음코드는 값을 변경할수 있는 변수를 생성하기 위해 mut키워드를 사용했다.

```rs
let foo =5; //불변
let mut bar = 5; //가변
```

- = 의 반대편에는 guess변수에 바인딩 할 값을 지정하는데 여기서는 새로운 String타입의 인스턴스를 생성하는 String::new함수의 실행결과를 바인딩 했다.
- String은 표준 라이브러리가 제공하는 문자열 타입으로 길이 조절이 가능하며 UTF-8형식으로 인코딩된 텍스트를 표현한다.
- ::new줄에서 사용된 ::문법은 new 함수가 String타입의 연관함수라는 점을 의미한다.
- 연관함수는 특정한 인스턴스가 아니라 타입 자체에 구현된 함수다
- new함수는 새로운 빈 문자열을 생성한다.
- let mut guess= String::new구문은 변경이 가능한 변수를 선언하고 여기에 빈값을 가진 String타입의 새로운 인스터스를 연결하는구문이다.

```rs
io::stdin().read_line(&mut guess).expect("입력한 값을 읽지 못했습니다.");
```

- 프로그램 시작부분에 use std::io두줄을 작성하지 않는다면 이 코드는 std::io::stdin과 같이 작성해도 된다.
- stdin함수는 터미널로부터 표준 입력에 대한 핸들을 표현하는 std::io::Stdin타입의 인스턴스를 리턴한다.
- 표준 입력핸들의 read_line메스드를 호출해서 사용자가 입력한 값을 읽어온다.이떄 &mut guess라는 인수를 read_line에 전달한다.
- read_line메서드의 역활은 사용자가 입력한 값을 표준 입력으로부터 읽어 문자열에 저장한다.그래서 문자열 인스턴스를 인수로 전달해준다.
- read_line메서드는 사용자가 입력한 값으로 문자열의 내용을 변경하기 떄문에 인수로 전달하는 문자열은 변경이 가능한 문자열이어야한다.

- 인수에 사용한 &는 참조 타입이라는 점을 지시한다.즉 프로그램의 다른곳에서도 해당데이터를 여러번 메모리에 복사할 필요없이 접근할수 있다는 것을 의미한다.
- 변수와 마찬가지로 참조 역시 기본적으로 변경할수가 없다.변경이 가능한 참조를 전달하기 위해 &guess가아닌 & mut guess로 표기해야한다.

## Result 타입을 이용해 잠재적인 오류 처리하기

- 메서드의 두번쨰 부분을 다시 살펴보자

```rs
.expect("입력한 값을 읽지 못했습니다.")

```

- 이 메서드가 호출하는 코드가 길떄는 .foo()형태로 다음줄로 넘기고 메서드 앞에 공백을 추가해주면 조금 더 보기 좋게 작성할수 있다.

```rs
io::stdin().read_line(&mut guess).expect("입력한 값을 읽지 못했습니다.")
```

- 하지만 이렇게 길게 작성한 코드는 읽기가 어려우므로 두개의 메서드 호출은 두줄로 나누는 것이 최선이다.
- read_line메서드는 사용자가 입력한 값을 우리가 전달한 문자열에 대입하지만 io::Result값을 리턴하기도 한다.
- Result타입은 열거자로 줄여서 enums라 표기하기도 한다.
- enums는 미리 정의된 범위의 값을 갖는 타입이며 이 값들을 열거자의 열것값 이라 부른다.
- Result열거자의 경우 열것값은 Ok와 Err이다
- Ok는 작업이 성공적으로 완료되었다는 것을 의미하며 ,Ok열것값에는 성공적으로 생성된 값을 보관한다.
- Err열것값은 작업이 실패햇음을 의미하며 실패한 원인에 대해 정보를 보관한다.
- Rseult타입의 목적은 에러 처리를 위하 정보를 인코딩 하기 위한 것이다.
- io::Result타입의 인스턴스는 조금 전 우리가 호출 했던 expect메서드를 정의한다.
- 만일 io::Result타입의 인스턴스가 Err값이라면 expect메서드는 프로그램의 실행을 종료하고 expect메서드에 인수로 전달한 메세지를 표시한다.
- io::Result인스턴스의 값이 Ok라면 expect메서드는 Ok값이 보관하고 있는 값만을 읽어와 리터한다.
- expect메서드를 호출하지 않는다면 컴파일은되지만 경고 메세지가 나타난다.

## println! 자리 지정자를 이용해 값 출력

```rs
println!("입력한 값",guess);
```

- 이코드는 사용자의 입력을 저장한 문자열을 출력한다.
- 여기서 사용한 중괄호는 자리 지정자라 부른다.
- 중괄호를 이용하면 둘 이상의 값도 출혈할수 있다.순서대로 대입댄다

```rs
let x = 5;
let y=  10;
println!("x={},y = {}",x,y);
```

## 지금까지 작성한 코드 테스트하기

```rs
cargo run

//숫자를 맞혀봅시다
//정답이라고 생각하는 숫자를 입력하세요
6
//입력한 값:6
```

다음 이어서....
