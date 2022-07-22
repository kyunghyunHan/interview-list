# pub

```
warning: function is never used: `connect`, #[warn(dead_code)] on by default
src/client.rs:1:1
  |
1 | fn connect() {
  | ^

warning: function is never used: `connect`, #[warn(dead_code)] on by default
 --> src/network/mod.rs:1:1
  |
1 | fn connect() {
  | ^

warning: function is never used: `connect`, #[warn(dead_code)] on by default
 --> src/network/server.rs:1:1
  |
1 | fn connect() {
  | ^

```

- src/main.rs

```rs
extern crate communicator;

fn main() {
    communicator::client::connect();
}
```

- exturn create 를 사용하여 라이브러리 참조

```
error: module `client` is private
 --> src/main.rs:4:5
  |
4 |     communicator::client::connect();
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

```

- 이 에러는 client모듈이 private임을 암시
- rust의 모든 코드의 공개는 비공개 입니다
- client::connect와 같은 함수를 공개로 지정한 뒤에는 우리의 바이너리 크레이트 상에서 이 함수를 호출하는 것이 가능해질 뿐만 아니라, 그 함수가 사용된 적이 없다는 경고 또한 사라질 것입니다.

## 함수를 공개로 만들기

- src/lib.rs

```rs
pub mod client;

mod network;
```

```
error: function `connect` is private
 --> src/main.rs:4:5
  |
4 |     communicator::client::connect();
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

```

```rs

pub fn connect() {
}
```

## 비공개 규칙(privacy rules)

- 만일 어떤 아이템이 공개라면, 이는 부모 모듈의 어디에서건 접근 가능합니다.
- 만일 어떤 아이템이 비공개라면, 같은 파일 내에 있는 부모 모듈 및 이 부모의 자식 모듈에서만 접근 가능합니다.

```rs
mod outermost {
    pub fn middle_function() {}

    fn middle_secret_function() {}

    mod inside {
        pub fn inner_function() {}

        fn secret_function() {}
    }
}

fn try_me() {
    outermost::middle_function();
    outermost::middle_secret_function();
    outermost::inside::inner_function();
    outermost::inside::secret_function();
}
```

- error
- outermost::middle_secret_function 호출은 컴파일 에러를 일으킬 것입니다. middle_secret_function는 비공개이므로, 두번째 규칙이 적용됩니다. 루트 모듈은 middle_secret_function의 현재 모듈도 아니고 (outermost가 현재 모듈입니다), middle_secret_function의 현재 모듈의 자식 모듈도 아닙니다.
- inside 모듈은 비공개고 자식 모듈이 없으므로, 이것의 현재 모듈인 outermost에 의해서만 접근될 수 있습니다. 이는 즉 try_me 함수는 outermost::inside::inner_function나 outermost::inside::secret_function를 호출할 수 없음을 의미합니다.
