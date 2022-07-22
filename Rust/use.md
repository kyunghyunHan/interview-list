# use

```rs
pub mod a {
    pub mod series {
        pub mod of {
            pub fn nested_modules() {}
        }
    }
}

fn main() {
    a::series::of::nested_modules();
}
```

## use를 이용한 간결한 가져오기

- rust의 use 키워드는 긴 함수 호출을 줄여줍니다

```rs
pub mod a {
    pub mod series {
        pub mod of {
            pub fn nested_modules() {}
        }
    }
}

use a::series::of;

fn main() {
    of::nested_modules();
}
```

- use 키워드는 우리가 명시한 것만 스코프 내로 가져옵니다: 즉 모듈의 자식들을 스코프 내로 가져오지는 않습니다. 이는 nested_modules 함수를 호출하고자 할 떄 여전히 of::nested_modules를 사용해야 하는 이유입니다.

```rs
pub mod a {
    pub mod series {
        pub mod of {
            pub fn nested_modules() {}
        }
    }
}

use a::series::of::nested_modules;

fn main() {
    nested_modules();
}
```

- 열거형 또한 모듈과 비슷한 일종의 이름공간을 형성하고 있기 때문에, 열거형의 variant 또한 use를 이용하여 가져올 수 있습니다. 어떠한 use 구문이건 하나의 이름공간으로부터 여러 개의 아이템을 가져오려 한다면, 여러분은 아래와 같이 중괄호와 쉼표를 구문의 마지막 위치에 사용하여 이 아이템들을 나열할 수 있습니다

```rs
enum TrafficLight {
    Red,
    Yellow,
    Green,
}

use TrafficLight::{Red, Yellow};

fn main() {
    let red = Red;
    let yellow = Yellow;
    let green = TrafficLight::Green;
}
```

## \*를 이용한 모두(glob) 가져오기

```rs
enum TrafficLight {
    Red,
    Yellow,
    Green,
}

use TrafficLight::*;

fn main() {
    let red = Red;
    let yellow = Yellow;
    let green = Green;
}
```

- \*는 글롭(glob) 이라고 부르며, 이는 이름공간 내에 공개된 모든 아이템을 가져올 것입니다. 여러분은 글롭을 아껴가며 써야 합니다: 글롭은 편리하지만, 여러분이 예상한 것보다 더 많은 아이템을 끌어와서 이름 간의 충돌(naming conflict)의 원인이 될수도 있습니다.

## super

```rs
pub mod client;

pub mod network;

#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
    }
}

```

```
communicator
 ├── client
 ├── network
 |   └── client
 └── tests
```

```rs
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        client::connect();
    }
}

```

```
$ cargo test
   Compiling communicator v0.1.0 (file:///projects/communicator)
error[E0433]: failed to resolve. Use of undeclared type or module `client`
 --> src/lib.rs:9:9
  |
9 |         client::connect();
  |         ^^^^^^^^^^^^^^^ Use of undeclared type or module `client`
```

- communicator::를 붙일 필요가 없는데, 왜냐하면 이 코드가 분명히 communicator 라이브러리 크레이트 안에 있기 때문입니다. 원인은 경로가 항상 현재 모듈을 기준으로 상대적인데, 여기는 test이기 때문입니다
- 부모 모듈로 올라가기위해

```
::client::connect();

```

or

```
super::client::connect();
```

- 1번쨰는 클론두개사용하여 전체경로나열
- 2번쨰는 현재 경로에 한단계 상위 경로

```rs
#[cfg(test)]
mod tests {
    use super::client;

    #[test]
    fn it_works() {
        client::connect();
    }
}
```

```
$ cargo test
   Compiling communicator v0.1.0 (file:///projects/communicator)
     Running target/debug/communicator-92007ddb5330fa5a

running 1 test
test tests::it_works ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
```
