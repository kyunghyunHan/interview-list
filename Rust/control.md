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
