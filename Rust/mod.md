# mod

- 바이너리 크레이트 --bin
- 라이브러리 크레이트 --lib

```

$ cargo new communicator --lib
$ cd communicator

```

- cargo가 src/main.rs.대신 src/lib.rs생성
- src/lib.rs 내부

```rs


fn main() {
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
    }
}
}

```

- cargo run 커맨드로 실행할것이 없기때문에 카고가 실행x
- cargo build사용

## 모듈정의

- rust내 모듈 정의는 mod로 시작

```rs



fn main() {
mod network {
    fn connect() {
    }
}
}


```

- 여러개의 모듈 나란히 정의

```rs


fn main() {
mod network {
    fn connect() {
    }
}

mod client {
    fn connect() {
    }
}
}

```

```
communicator
 ├── network
 └── client

```

- network::connect함수와 clinet::connect함수
- 이둘은 완전히 다른 기능을 가지고 있거나 서로 다른 모듈에 정의 되어 있기 때문에 서로 부딪힐 일이 없습니다.

```rs
fn main() {
mod network {
    fn connect() {
    }

    mod client {
        fn connect() {
        }
    }
}
}

```

```
communicator
 └── network
     └── client

```

## 모듈을 다른 파일로 옮기기

```rs

#![allow(unused)]
fn main() {
mod client {
    fn connect() {
    }
}

mod network {
    fn connect() {
    }

    mod server {
        fn connect() {
        }
    }
}
}

```

```
communicator
 ├── client
 └── network
     └── server

```

- 만약 이 모듈들이 여러개의 함수들을 가지고 있고 이함수들이 길어지고 있다면 코드를 찾기 힘들어 질겁니다
- 그렇게떄문에 clinet,network,server모듈을 lib.rs로부터 뗴어 각자를 위한 파일들에 위치시키기 좋은 이유

```rs
mod client;

mod network {
    fn connect() {
    }

    mod server {
        fn connect() {
        }
    }
}

```

- 여전히 clinet모듈을 사용하고 있지만 코드블록을 세미클론으로 대체함으로써 rust에게 client모둘이 스코프 내에 정의된 코드를 다른 위치에서 찾으라 명령

```rs
mod client {
    // contents of client.rs
}

```

- 이제 모듈의 이름과 같은 이름을 가진 외부파일을 생성
- client.rs파일을 src/디렉토리에 생성
- src/client.rs

```rs

fn connect() {
}


```

- 기본적으로 src/lib.rs만 찾기때문에 만약에 더많은 파일을 프로젝트에 추가하고 싶다면 src/lib.rs 내에 mod client;생성하여 rust에게 알려줄필요잇습니다
- 라이브러리 크레이트를 만드는 중이므로 cargo run대신 cargo build

```
$ cargo build
   Compiling communicator v0.1.0 (file:///projects/communicator)

warning: function is never used: `connect`, #[warn(dead_code)] on by default
 --> src/client.rs:1:1
  |
1 | fn connect() {
  | ^

warning: function is never used: `connect`, #[warn(dead_code)] on by default
 --> src/lib.rs:4:5
  |
4 |     fn connect() {
  |     ^

warning: function is never used: `connect`, #[warn(dead_code)] on by default
 --> src/lib.rs:8:9
  |
8 |         fn connect() {
  |         ^

```

- 이 경고들은 사용한적이 없는 함수가 있음을 경고
- pub을 이용하여 가시성 제어

```rs
mod client;

mod network;

```

- network.rs생성

```rs


fn connect() {
}

mod server {
    fn connect() {
    }
}


```

- 서브 모듈이기 때문에 모듈을 파일로 추출해서 모듈이름으로 사용하는 전략은 사용하기 힘듭니다

```rs
fn connect() {
}

mod server;

```

- src/server.rs

```rs


fn connect() {
}


```

```
$ cargo build
   Compiling communicator v0.1.0 (file:///projects/communicator)
error: cannot declare a new module at this location
 --> src/network.rs:4:5
  |
4 | mod server;
  |     ^^^^^^
  |
note: maybe move this module `network` to its own directory via `network/mod.rs`
 --> src/network.rs:4:5
  |
4 | mod server;
  |     ^^^^^^
note: ... or maybe `use` the module `server` instead of possibly redeclaring it
 --> src/network.rs:4:5
  |
4 | mod server;
  |     ^^^^^^

```

- 이위치에 새로운 모듈을 선언할수 없다고 경고하며 mod server라인을 지적;

- 다른방법인 network 이름의 새로운디렉토리 생성
- src/network.rs 파일을 이 새로운 network 디렉토리 안으로 옮기고, 파일 이름을 src/network/mod.rs로 고치세요.
- 서브모듈 파일 src/server.rs를 network 디렉토리 안으로 옮기세요.

```
$ mkdir src/network
$ mv src/network.rs src/network/mod.rs
$ mv src/server.rs src/network

```

```
communicator
 ├── client
 └── network
     └── server

```

```
├── src
│   ├── client.rs
│   ├── lib.rs
│   └── network
│       ├── mod.rs
│       └── server.rs

```

```
communicator
 ├── client
 └── network
     └── client

```

- 위에서는 client,network,network::clinet라는 세개의 모듈이 존재
- 따라서 network모듈의 network::client서브모듈을 위한 파일을 추출하기 위해서는 src/network대신 network모듈을 위한 디렉토리 생성

## 모듈 파일 시스템이 규칙

- 만일 foo라는 이름의 모듈이 서브모듈을 가지고 있지 않다면, foo.rs라는 이름의 파일 내에 foo에 대한 선언을 집어넣어야 합니다.
- 만일 foo가 서브모듈을 가지고 있다면, foo/mod.rs라는 이름의 파일에 foo에 대한 선언을 집어넣어야 합니다.

```
├── foo
│   ├── bar.rs (contains the declarations in `foo::bar`)
│   └── mod.rs (contains the declarations in `foo`, including `mod bar`)

```
