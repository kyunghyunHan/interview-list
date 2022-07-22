# References or Borrowing

```rs
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

- & ->참조자 어떤값을 소유권을 넘기지 않고 참조 할수 있도록

```rs
let s1 = String::from("hello");

let len = calculate_length(&s1);
```

- &s1 문법은 우리가 s1의 값을 참조하지만 소유하지는 않는 참조자를 생성하도록 해줍니다
- 소유권을 갖고 있지는 않기 때문에, 이 참조자가 가리키는 값은 참조자가 스코프 밖으로 벗어났을 때도 메모리가 반납되지 않을 것입니다.

```rs
fn calculate_length(s: &String) -> usize { // s는 String의 참조자입니다
    s.len()
} // 여기서 s는 스코프 밖으로 벗어났습니다. 하지만 가리키고 있는 값에 대한 소유권이 없기
  // 때문에, 아무런 일도 발생하지 않습니다.
```

```rs
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world");
}

```

```
error: cannot borrow immutable borrowed content `*some_string` as mutable
 --> error.rs:8:5
  |
8 |     some_string.push_str(", world");
  |     ^^^^^^^^
```

- 참조하는 어떤것을 변경하는것은 허용되지 않음

## 가변참조자

```rs
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}

```

- 제한:특정한 스코프 내에서 특정한 데이터 조각에 대한 가변참조자를 딱 하나만 생성
- rust가 컴파일 타임에 data race를 방지

```
두 개 이상의 포인터가 동시에 같은 데이터에 접근한다.
그 중 적어도 하나의 포인터가 데이터를 쓴다.
데이터에 접근하는데 동기화를 하는 어떠한 메커니즘도 없다.
```

```rs
let mut s = String::from("hello");

{
    let r1 = &mut s;

} // 여기서 r1은 스코프 밖으로 벗어났으므로, 우리는 아무 문제 없이 새로운 참조자를 만들 수 있습니다.

let r2 = &mut s;
```

```rs
let mut s = String::from("hello");

let r1 = &s; // 문제 없음
let r2 = &s; // 문제 없음
let r3 = &mut s; // 큰 문제
```

- 불변 참조자를 가지고 있을 동안에도 역시 가변 참조자를 만들 수 없습니다.

## 댕글링 참조자(Dangling References)

```rs
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s
}
```

```rs
fn dangle() -> &String { // dangle은 String의 참조자를 반환합니다

    let s = String::from("hello"); // s는 새로운 String입니다

    &s // 우리는 String s의 참조자를 반환합니다.
} // 여기서 s는 스코프를 벗어나고 버려집니다. 이것의 메모리는 사라집니다.
  // 위험하군요!

```

- s 가 함수 안에서 만들어졋기 떄문에 함수의 코드가 끝이 나면 s 는 할당해제
- 무효화된 string을 참조하면 error발생

```rs

fn no_dangle() -> String {
    let s = String::from("hello");

    s
}
```

## 참조자의 규칙

```
어떠한 경우이든 간에, 여러분은 아래 둘 다는 아니고 둘 중 하나만 가질 수 있습니다:
하나의 가변 참조자
임의 개수의 불변 참조자들
참조자는 항상 유효해야만 한다.
```
