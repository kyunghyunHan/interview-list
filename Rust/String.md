## 스트링

- rust는 핵심 언어 기능 내에서 딱 한가지 스트링 타입만 제공하는데 str,참조자 &str
- String타입은 핵심 언어 기능내에서 구현된 것이 아니고,러스트 라이브러리를 통해 제공,가변적이며 소유권을 가지고 있고 UTF-8로 인고딩 된 스트링 타입입니다

## 스트링 생성

```rs
let mut s = String::new();
```

- to_string 메소드를 사용하여 스트링 리터럴로부터 String 생성하기

```rs
let data = "initial contents";
let s = data.to_string();
// the method also works on a literal directly:
let s = "initial contents".to_string();
```

- String::from 함수를 사용하여 스트링 리터럴로부터 String 생성하기

```rs
let s = String::from("initial contents");
```

- 스트링에 다양한 언어로 인삿말 저장하기

```rs
let hello = String::from("السلام عليكم");
let hello = String::from("Dobrý den");
let hello = String::from("Hello");
let hello = String::from("שָׁלוֹם");
let hello = String::from("नमस्ते");
let hello = String::from("こんにちは");
let hello = String::from("안녕하세요");
let hello = String::from("你好");
let hello = String::from("Olá");
let hello = String::from("Здравствуйте");
let hello = String::from("Hola");
```

## 스트링 갱신하기

- push_str과 push를 이용하여 스트링 추가하기

```rs
let mut s = String::from("foo");
s.push_str("bar");
```

```rs
let mut s1 = String::from("foo");
let s2 = "bar";
s1.push_str(&s2);
println!("s2 is {}", s2);
```

```rs
let mut s = String::from("lo");
s.push('l');
```

## +연산자나 format!매크로를 이용한 조합

- 종종 우리는 가지고 있는 두 개의 스트링을 조합하고 싶어합니다. 한 가지 방법은 아래 Listing 8-18와 같이 + 연산자를 사용하는 것입니다:
- +연산자를 사용하여 두 String 값을 하나의 새로운 String 값으로 조합하기

```rs
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // s1은 여기서 이동되어 더이상 쓸 수 없음을 유의하세요

```

- 첫번째로, s2는 &를 가지고 있는데, 이는 add 함수의 s 파라미터 때문에 첫번째 스트링에 두번째 스트링의 참조자를 더하고 있음을 뜻합니다: 우리는 String에 &str만 더할 수 있고, 두 String을 더하지는 못합니다.
- &s2를 add 호출에 사용할 수 있는 이유는 &String 인자가 &str로 강제될 수 있기 때문입니다 -
- 두번째로, 시그니처에서 add가 self의 소유권을 가져가는 것을 볼 수 있는데, 이는 self가 &를 안 가지고 있기 때문입니다.

```rs
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = s1 + "-" + &s2 + "-" + &s3;
```

```rs
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{}-{}-{}", s1, s2, s3);
- println!과 똑같은 방식으로 작동하지만, 스크린에 결과를 출력하는 대신 결과를 담은 String을 반환해줍니다. format!을 이용한 버전이 훨씬 읽기 쉽고, 또한 어떠한 파라미터들의 소유권도 가져가지 않습니다.
```

## string내부의 인덱싱

```rs
let s1 = String::from("hello");
let h = s1[0];
```

```
error: the trait bound `std::string::String: std::ops::Index<_>` is not
satisfied [--explain E0277]
  |>
  |>     let h = s1[0];
  |>             ^^^^^
note: the type `std::string::String` cannot be indexed by `_`
```

```rs
let len = String::from("Hola").len();
```

```rs
let len = String::from("Здравствуйте").len();
```

```rs
let hello = "Здравствуйте";
let answer = &hello[0];
```

## 스트링 슬라이싱하기

```rs
let hello = "Здравствуйте";

let s = &hello[0..4];
```

- 글자들이 각각 2바이트를 차지한다고 언급했으므로, 이는 s가 “Зд”이 될 것이란 뜻

```
thread 'main' panicked at 'index 0 and/or 1 in `Здравствуйте` do not lie on
character boundary', ../src/libcore/str/mod.rs:1694
```

- s는 string의 첫 바이트를 탐고 있는 &str

## 스트링 내에서 반복적으로 실행되는 메소드

- 만일 개별적인 유니코드 스칼라 값에 대한 연산을 수행하길 원한다면, 가장 좋은 방법은 chars 메소드를 이용하는 것
- chars를 “नमस्ते”에 대해 호출하면 char타입의 6개의 값으로 나누어 반환하며, 각각의 요소에 접근하기 위해 이 결과값에 대해 반복

```rs
for c in "नमस्ते".chars() {
    println!("{}", c);
}
```

```
न
म
स
्
त
े
```

- bytes 메소드는 가공되지 않은 각각의 바이트를 반환하는데,문제 범위에 따라 적절할 수도 있습니다

```rs
for b in "नमस्ते".bytes() {
    println!("{}", b);
}


```

```
224
164
168
224
// ... etc
```
