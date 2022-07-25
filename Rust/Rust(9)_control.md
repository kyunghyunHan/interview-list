# 흐름제어

- 흐름제어는 일부 코드를 특정조건을 만족할떄만 실행하거나 특정 조건을 만족하는 동안에만 일부 코드를 반복해서 실행하는 것으로 수행한다.
- Rust에서 프로그램의 실행 흐름을 제어할수 잇는 가장 일반적인 구조는 if표현식과 루프이다.

## if

- if 표현식은 조건에 따라 코드를 분기한다.
- 조건을 지정한다는 것은 이조건이 일치하면 이 코드블록을 실행하고,조건이 일치하지 않으면 이코드를 실행하지 말것 을 표현하는 것이다.

```rs
fn main() {
    let num = 3;

    if num < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}

```

- 모든 if표현식은 if키워드로 시작하며 그 뒤에 조건을 명시한다.
- if표현식의 조건과 관련된 코드의 블록은 가지라 부르기도 한다.
- 필요하다면 else표현식을 이용해 조건이 일치하지 않을 떄 프로그램이 실행할수 있는 코드 블록을 별도로 작성할수도 있다.
- else표현식을 작성하지 않았는데 조건이 일치하지 않는다면 프로그램은 그냥 If블록을 건너뛰고 그 다음 코드를 실행한다.

```rs
cargo run
//조건이 일치합니다
```

- 조건이 일치하지 않았을떄

```rs
let number = 7;
```

```
cargo run
//조건이 일치하지 않습니다.
```

- if문의 조건은 반스시 불리언 타입중 하나를 리턴해야한다.조건이 불리언을 리턴하지 않으면 에러가 발생한다.

```rs
fn main() {
    let num = 3;

    if number {
        println!("변수에 저장된 값은 3입니다.");
    }
}

```

- 에러 메세지를 보면 rust는 불리언 타입의 결과를 원했지만 정수타입의 결과가 리턴되었다.
- rust는 다른 언어와 달리 불리언이 아닌 값을 불리언으로 자동으로 변환하지 않는다.
- 그래서 if표현식의 조건을 불리언으로 제공해야한다.
- 0이 아닐떄만 실행

```rs
fn main() {
    let num = 3;

    if num != 0 {
        println!("number was something other than zero");
    }
}

```

## else if를 이용해 여러 조건을 처리하기

- if와 else사이에 else if 표현식을 추가하면 여러개의 조건을 처리할수 있다.

```rs
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

```
cargo run
number is divisible by 3
```

- 이 코드는 if표현식을 차례대로 평가해서 처음으로 일치하는 표현식의 본문을 실해한다.
- 6이라는 값은 2로도 나누어 떨어지지만 다른 메세지는 화면에 출련되지 않는 것을 알수 있다.
- Rust가 처음으로 조건이 일치하는 코드 블록만 실행하며 나머지 조건을 평가하지 않기 떄문이다.
- elseif 표현식을 많이 사용하면 코드가 지저분해 보이므로 둘이상의 else if표현식이 필요할 떄는 리팩토링해야한다.(match사용)

## let구문에서 if표현식

```rs
fn main() {
    let condition = true;
    let num = if condition { 5 } else { 6 };
    println!("The value of number is: {}", num);
}

```

- 코드블록의 결과는 마지막 표현식의 값을 평가하며 숫자 자체도 하나의 표현식임을 기억하자
- if표현식 전체의 결과는 어떤 코드 블록이 실행되는가에 따라 달라진다.
- if 구문의 가지가 리턴하는 결과는 모두 같은 타입이어야 한다.

- 일치하지 않는경우

```rs
fn main() {
    let condition = true;
    let num = if condition { 5 } else { "six" };
    println!("The value of number is: {}", num);
}
//error
```

- if 블록의 표현식은 정수로 평가되지만 .else블록의 표현식은 문자열로 평가된다.
- 변수 타입이 반드시 하나 이어야 하므로 이 예제는 동작하지 않는다.

## loop

```

```
