# 제어문

## if

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

- 0이 아닐시에 실행

```rs
fn main() {
    let num = 3;

    if num != 0 {
        println!("number was something other than zero");
    }
}

```

## else if 와 다수 조건

- if 와 else사이에 else if식을 추가 결합

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

- number is divisible by 2가 출력되지 않는데 Rust가 첫번쨰 조건이 참이 되는 블록만 찾아 실행 나머는 검사하지 않음

## let 구문에서 if 사용

- if 표현식의 결과값을 변수에 대입하기

```rs
fn main() {
    let condition = true;
    let num = if condition { 5 } else { 6 };
    println!("The value of number is: {}", num);
}

```

- if갈래와 else 갈래 모두 같은 타입가져야 한다

## 반복문

```rs
fn main() {
    loop {
        println!("agin!");
    }
}

```

- while함꼐하는 조건부 반복

```rs
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{}!", number);

        number = number - 1;
    }

    println!("LIFTOFF!!!");
}
```

- for과 함꼐하는 콜렌션 반복

```rs
fn main() {
    let a = [10, 20, 30, 40, 50];
    let mut index = 0;

    while index < 5 {
        println!("the value is: {}", a[index]);

        index = index + 1;
    }
}
```

- 이러한 방식은 에러를 발생하기 쉬움
- 정확한 길이의 색인을 찾지 못하면 프로그램은 패닉이 발생

## for in

```rs
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```

## for 반복문을 사용해 콜렉션 각 요소 순회

```rs
fn main() {
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}
```

- range 는 한숫자에서 다른숫자 전까지 모든 숫자를 차례대로 생성
- range를 역순하는 rev
