## 구조체

```rs
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool
}
```

- 구조체를 사용하려면 각 필드의 값을 명세한 인스턴스를 생성
- 안스턴스는 구조체의 이름을 명시함으로써 사용할수 있고 필드를 식별할수 있는 키와 value를 이어지는 중괄호 안에 추가하여 생성

```rs
let user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};
```

## User의 인스턴스 생성

- 구조체에서 특정한 값을 읽어오려면 .표기법 사용

```rs
 let mut user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};

user1.email = String::from("anotheremail@example.com");
```

## User인스턴스의 email필드 변경

- 인스턴스는 반디스 변경가능 mutable해야합니다
- rust에서는 특정 필드만 변경할수 있도록 허용하지 않습니다

```rs
fn bulid_user(email:String,username:String)->User{
    User{
        email: email,
        username: username,
        active: true,
        sign_in_count: 1,

    }
}
```

## 변수명이 필드명과 같을 떄 간단하게 필드 초기화

- 매개변수인 email과 usename이 User구조체의 필드명과 같기 떄문에 build_user에서 emali과 username를 명시하는 부분을 다시 작성할 필요가 없습니다

```rs
fn build_user(email: String, username: String) -> User {
    User {
        email,
        username,
        active: true,
        sign_in_count: 1,
    }
}
```

## 구조체 갱신법을 이용하여 기존 구조체 인스턴스로 새 구조체 인스턴스 생성하기

- user2에 email과 username은 새로 할당하고 user1의 값들은 그대로 사용

```rs
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    active: user1.active,
    sign_in_count: user1.sign_in_count,
};

```

## user1의 일부 값들을 재사용하려 구조체 User의 인스턴스 user2를 새로생성

- 구조체 갱신법은 입력으로 주어진 인스턴스와 변화하지 않는 필드들을 명시적으로 할당하지 않기 위해 ..구문을 사용

```rs
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    ..user1
};

```

## 이름이 없고 필드마다 타입은 다르게 정의 가능한 튜플 구조체

- 구조체 명을 통해 의미를 부여할수는 있으나 필드의 타입만 정의할수 있고 명명은 할수 없는 튜플구조체

```rs
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

- 다른 튜플 구조체 이기떄문에 black 와 origin은 다른 타입

## 구조체 데이터의 소유권

- 구조체가 소유권이 없는 데이터의 참조를 저장할수는 있지만 라이프 타임의 사용을 전제로합니다
- 라이프타임은 구조체가 존재하는 동안 참조하는 데이터를 계속 존재할수 있도록 합니다
- 라이프타임을 사용하지 않고 참조를 저장하고자 하면 아래와 같은일이 발생

```rs
struct User {
    username: &str,
    email: &str,
    sign_in_count: u64,
    active: bool,
}

fn main() {
    let user1 = User {
        email: "someone@example.com",
        username: "someusername123",
        active: true,
        sign_in_count: 1,
    };
}

```

```
error[E0106]: missing lifetime specifier
 -->
  |
2 |     username: &str,
  |               ^ expected lifetime parameter

error[E0106]: missing lifetime specifier
 -->
  |
3 |     email: &str,
  |            ^ expected lifetime parameter
```
