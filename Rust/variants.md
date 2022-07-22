# 열거형

```rs
enum IpAddrKind {
    V4,
    V6,
}
```

## 열거형 값

```rs
let four = IpAddrKind::V4;
let six = IpAddrKind::V6;
```

- IpAddrKind::V4 와 IpAddrKind::V6 은 동일한 타입이기 떄문에 이방식이 유용

```rs
fn route(ip_type: IpAddrKind) { }
```

```rs
enum IpAddrKind {
    V4,
    V6,
}

struct IpAddr {
    kind: IpAddrKind,
    address: String,
}

let home = IpAddr {
    kind: IpAddrKind::V4,
    address: String::from("127.0.0.1"),
};

let loopback = IpAddr {
    kind: IpAddrKind::V6,
    address: String::from("::1"),
};
```

## struct를 사용해서 IP주소의 데이터와 IpaddrKind Variant저장

- 첫 번째 home 은 kind 의 값으로 IpAddrKind::V4 을 갖고 연관된 주소 데이터로 127.0.0.1 를 갖습니다
- 두 번째 loopback 은 IpAddrKind 의 다른 variant 인 V6 을 값으로 갖고, 연관된 주소로 ::1 를 갖습니다

- 각 열거형 Variant에 데이터를 직접 넣는 방식을 사용해서 열거형 구조체의 일부로 사용하는 방식보다 더 간결하게 동일한 개념을 표현할수 있습니다.
- IpAddr열거형의 새로운 정의에서는 두개의 V4와 V6 Variant는 연관된 String타입을 갖게됩니다

```rs
enum IpAddr{
    V4(String),
    V6(String),
}
let home = IpAddr::V4(String::from("127.0.0.1"));
let loopback = IpAddr::V6(String::form("::1"));
```

- 열거형에 각 variant 에 데이터를 붙임으로써 구조체를 사용할 필요가 없어졋습니다
- 구조체보다 열거형사용할떄의 다른장점이 잇습니다
- variant 는 다른 타입과 다른 양의 연관된 데이터를 가질 수 있습니다
- 버전 4 타입의 IP 주소는 항상 0 ~ 255 사이의 숫자 4개로 된 구성요소를 갖게 될 것입니다. V4 주소에 4개의 u8 값을 저장하길 원하지만, V6 주소는 하나의 String 값으로 표현되길 원한다면, 구조체로는 이렇게 할 수 없습니다. 열거형은 이런 경우를 쉽게 처리합니다:

```rs
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);

let loopback = IpAddr::V6(String::from("::1"));
```

```rs
struct Ipv4Addr {
    // details elided
}

struct Ipv6Addr {
    // details elided
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}

```

```rs
use std::net::{IpAddr, Ipv4Addr, Ipv6Addr};

let localhost_v4 = IpAddr::V4(Ipv4Addr::new(127, 0, 0, 1));
let localhost_v6 = IpAddr::V6(Ipv6Addr::new(0, 0, 0, 0, 0, 0, 0, 1));

assert_eq!("127.0.0.1".parse(), Ok(localhost_v4));
assert_eq!("::1".parse(), Ok(localhost_v6));

assert_eq!(localhost_v4.is_ipv6(), false);
assert_eq!(localhost_v4.is_ipv4(), true);
```

## 열거형의 다른 예제

```rs
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

- 이 열거형에는 다른 데이터 타입을 갖는 네 개의 variants 가 있습니다:

```
Quit 은 연관된 데이터가 전혀 없습니다.
Move 은 익명 구조체를 포함합니다.
Write 은 하나의 String 을 포함합니다.
ChangeColor 는 세 개의 i32 을 포함합니다.
```

- variants 로 열거형을 정의하는 것은 다른 종류의 구조체들을 정의하는 것과 비슷합니다

```rs
struct QuitMessage; // 유닛 구조체
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // 튜플 구조체
struct ChangeColorMessage(i32, i32, i32); // 튜플 구조체
```

- 구조체에 impl 을 사용해서 메소드를 정의한 것처럼, 열거형에도 정의할 수 있습니다. 여기 Message 열거형에 에 정의한 call 이라는 메소드가 있습니다:

```rs
impl Message {
    fn call(&self) {
        // 메소드 내용은 여기 정의할 수 있습니다.
    }
}

let m = Message::Write(String::from("hello"));
m.call();
```

- 열거형의 값을 가져오기 위해 메소드 안에서 self 를 사용할 것입니다. 이 예제에서 생성한 변수 m 은 Message::Write(String::from("hello")) 값을 갖게 되고, 이 값은 m.call()이 실행될 때, call 메소드 안에서 self가 될 것입니

## Option 열거형과 Null값보다 좋은점들

- Option 타입은 많이 사용되는데, 값이 있거나 없을 수도 있는 아주 흔한 상황을 나타내기 때문입니다. 이 개념을 타입 시스템의 관점으로 표현하자면, 컴파일러가 발생할 수 있는 모든 경우를 처리했는지 체크할 수 있습니다. 이렇게 함으로써 버그를 방지할 수 있고, 이것은 다른 프로그래밍 언어에서 매우 흔합니다.
- 프로그래밍 언어 디자인은 가끔 어떤 특성들이 포함되었는지의 관점에서 생각되기도 하지만, 포함되지 않은 특성들도 역시 중요합니다. 러스트는 다른 언어들에서 흔하게 볼 수 있는 null 특성이 없습니다.
- null 값으로 발생하는 문제는, null 값을 null 이 아닌 값처럼 사용하려고 할 때 여러 종류의 오류가 발생할 수 있다는 것입니다. null이나 null이 아닌 속성은 어디에나 있을 수 있고, 너무나도 쉽게 이런 종류의 오류를 만들어 냅니다.
- 이와 같이 러스트에는 null 이 없지만, 값의 존재 혹은 부재의 개념을 표현할 수 있는 열거형이 있습니다. 이 열거형은 Option<T>

```rs
enum Option<T> {
    Some(T),
    None,
}

```

- Option:: 를 앞에 붙이지 않고, Some 과 None 을 바로 사용할 수 있습니다. Option<T> 는 여전히 일반적인 열거형이고, Some(T) 과 None 도 여전히 Option<T> 의 variants 입니다.

```rs
let some_number = Some(5);
let some_string = Some("a string");

let absent_number: Option<i32> = None;
```

- Some 이 아닌 None 을 사용한다면, Option<T> 이 어떤 타입을 가질지 러스트에게 알려줄 필요가 있습니다. 컴파일러는 None 만 보고는 Some variant 가 어떤 타입인지 추론할 수 없습니다.
- Option<T> 와 T (T 는 어떤 타입이던 될 수 있음)는 다른 타입이며, 컴파일러는 Option<T> 값을 명확하게 유효한 값처럼 사용하지 못하도록 합니다. 예를 들면, 아래 코드는 Option<i8> 에 i8 을 더하려고 하기 때문에 컴파일되지 않습니다:

```rs
let x: i8 = 5;
let y: Option<i8> = Some(5);

let sum = x + y; //error
```

- 주목하세요! 실제로, 이 에러 메시지는 러스트가 Option<i8> 와 i8 를 어떻게 더해야 하는지 모른다는 것을 의미하는데, 둘은 다른 타입이기 때문입니다
- T 에 대한 연산을 수행하기 전에 Option<T> 를 T 로 변환해야 합니다. 일반적으로, 이런 방식은 null 과 관련된 가장 흔한 이슈 중 하나를 발견하는데 도움을 줍니다: 실제로 null 일 때, null 이 아니라고 가정하는 경우입니다.

- null 이 아닌 값을 갖는다는 가정을 놓치는 경우에 대해 걱정할 필요가 없게 되면, 코드에 더 확신을 갖게 됩니다. null 일 수 있는 값을 사용하기 위해서, 명시적으로 값의 타입을 Option<T> 로 만들어 줘야 합니다. 그다음엔 값을 사용할 때 명시적으로 null 인 경우를 처리해야 합니다. 값의 타입이 Option<T> 가 아닌 모든 곳은 값이 null 아 아니라고 안전하게 가정할 수 있습니다. 이것은 null을 너무 많이 사용하는 문제를 제한하고 러스트 코드의 안정성을 높이기 위한 러스트의 의도된 디자인 결정사항입니다.
