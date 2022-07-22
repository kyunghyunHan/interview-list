## 슬라이스

- 소유권을 갖지 않는 데이터 타입
- 컬렉션 전체가 아닌 컬렉션의 연속될 일련의 요소들을 참조

```rs
fn first_word(s: &String) -> usize {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return i;
        }
    }

    s.len()
}
```

```rs

fn first_word(s: &String) -> usize {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return i;
        }
    }

    s.len()
}
```

```rs
let bytes = s.as_bytes();
```

- 입력된 string을 요소별로 보면서 그값이 공백인지 확인할 필요가 있기 떄문에 string은 as_bytes메소드를 이용하여 바이트 배열로 변환

```rs
for (i, &item) in bytes.iter().enumerate() {
```

- 지금은 iter가 컬렉션의 각 요소를 변환하는 함수
- euumerate은 iter의 결과값을 직접 반환하는 대신 이를 감싸서 튜플의 일부로 만들어 반환
- 반환된 튜플의 첫번쨰 요소는 인덱스,두번쨰는 요소에 대한 참조값
- enumerate 메소드가 튜플을 반환하기 때문에 , 이 튜플을 해제하기 위해 패턴을 이용
- for 루프 내에서 i 는 튜플 내의 인덱스 에 대응 &item은 튜플내의 한바이트에 대응하는 패턴을 기술
- .iter().enumerate()의 요소에 대한 참조자를 갖는것이므로 & 사용

```rs
    if item == b' ' {
        return i;
    }
}
s.len()
```

```rs
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s); // word는 5를 갖게 될 것입니다.

    s.clear(); // 이 코드는 String을 비워서 ""로 만들게 됩니다.

    // word는 여기서 여전히 5를 갖고 있지만, 5라는 값을 의미있게 쓸 수 있는 스트링은 이제 없습니다.
    // word는 이제 완전 유효하지 않습니다!
}
```

- 이 프로그램은 오류없이 컴파일 되고 s.clear()을 사용한뒤 word를 호출해도 역시 컴파일 될 것입니다

## 스트링슬라이스

- 스트링 슬라이스는 String의 일부분에 대한 참조자

```rs
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```

- world 는 s 의 6번쨰 바이트를 가리키고 있는 포인터와 기링값 5를 가지고 있는 슬라이스

## string의 일부를 참조하는 스트링 슬라이스

```rs
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];
```

- 두둘은 동일한 표현

```rs
let s = String::from("hello");

let len = s.len();

let slice = &s[3..len];
let slice = &s[3..];
```

- 동일

```rs
let s = String::from("hello");

let len = s.len();

let slice = &s[0..len];
let slice = &s[..];
```

- 동일

```rs
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

- 공백 문자가 첫번쨰로 나타난 지점을 찾아서 단어의 끝 인덱스를 얻어냅니다
- 스트링의 시작과 공백문자의 인덱스를 각각 시작과 끝 인덱스를 사용하여 스트링 슬라이스로 반환

```rs
fn second_word(s: &String) -> &str {
```

```rs
//error
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);

    s.clear(); // Error!

    println!("the first word is: {}", word);
}
```

- 무언가에 대한 불변 참조자를 만들었을 경우 가변참조자를 만들수 없다는 점
- claer함수가 string을 잘라낼 필요가 있기 떄문에
  이함수는 가변참조자를 갖기 위함 시도를 핡것이고 이는 실패

## 스트링 리터럴 슬라이스

- s 의 타입은 &str 이것은 바이너리의 특점 지점을 가리키는 슬라이스
- &str은 불변참조자

```rs
let s = "Hello, world!";
```

## 파라미터로서의 스트링 슬라이스

```rs
fn first_word(s: &String) -> &str {
```

```rs
fn first_word(s: &str) -> &str {
```

- &string 과 &str모두 같은 함수를 사용할수 있게함
- 함수가 String의 참조자 대신 스트링 슬라이스를 갖도록 정의하는 것은 우리의 API를 어떠한 기능적인 손실 없이도 더 일반적이고 유용하게 해줍니다:

```rs
fn main() {
    let my_string = String::from("hello world");

    // first_word가 `String`의 슬라이스로 동작합니다.
    let word = first_word(&my_string[..]);

    let my_string_literal = "hello world";

    // first_word가 스트링 리터럴의 슬라이스로 동작합니다.
    let word = first_word(&my_string_literal[..]);

    // 스트링 리터럴은 *또한* 스트링 슬라이스이기 때문에,
    // 아래 코드도 슬라이스 문법 없이 동작합니다!
    let word = first_word(my_string_literal);
}
```

```rs

let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];
```

- 이 슬라이스는 &[i32] 타입을 가짐

## 정리

- 소유권 ,빌림 , 슬라이스의 개념은 러스트 프로그램이 메모리 안정성을 컴파일 타임에 보장하는것
