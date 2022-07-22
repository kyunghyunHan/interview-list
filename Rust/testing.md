# testing

- 예를 들어 어떤 숫자든 입력되면 2를 더하는 add_two라는 함수를 작성한다 칩시다. 이 함수의 시그니처는 정수를 파라미터로 받아들여서 정수를 결과로 반환합니다
- 러스트는 우리가 정확히 의도한 것을 이 함수가 수행하는가에 대해서는 검사할 수 없는데, 말하자면 파라미터 더하기 10 혹은 파라미터 빼기 50이 아니라 파라미터 더하기 2여야 합니다! 이러한 지점이 바로 테스트가 필요해지는 부분입니다.
- 예를 들면 우리가 3을 add_two 함수에 넘겼을 때, 반환 값은 5임을 단언하는(assert) 테스트를 작성할 수 있습니다. 우리는 어떤 종류의 코드 변경이라도 있을 때마다 기존의 정확히 동작하던 부분에 어떠한 변화도 없음을 확신할 수 있도록 이 테스트들을 실행할 수 있습니다.

## 테스트를 작성

- 테스트는 테스트 아닌 코드가 프로그램 내에서 기대했던 대로 기능을 하는지 검증하는 러스트 함수입니다. 테스트 함수의 본체는 통상적으로 다음의 세 가지 동작을 수행합니다:

```
1.필요한 데이터 혹은 상태를 설정하기
2.우리가 테스트하고 싶은 코드를 실행하기
3.그 결과가 우리 예상대로인지 단언하기(assert)
```

## 테스트 함수의 해부

- 러스트 내의 테스트란 test 속성(attribute)이 주석으로 달려진 (annotated) 함수입니다.
- 속성은 러스트 코드 조각에 대한 메타데이터입니다:
- 한 가지 예로 5장에서 우리가 구조체와 함께 사용했던 derive 속성이 있습니다. 함수를 테스트 함수로 변경하기 위해서는, fn 전 라인에 #[test]를 추가합니다. cargo test 커맨드를 사용하여 테스트를 실행시키면, 러스트는 test 속성이 달려있는 함수들을 실행하고 각 테스트 함수가 성공 혹은 실패했는지를 보고하는 테스트 실행용 바이너리를 빌드할 것입니다.
- adder라는 새로운 라이브러리 생성

```
$ cargo new adder --lib
     Created library `adder` project
$ cd adder
```

- src/lib.rs

```rs
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}
```

- fn 라인 전의 #[test] 이 속성이 테스트 함수임을 나타내므로 테스트 실행기는 이 함수를 테스트로 다루어야 한다는 것을 알게 됩니다
- 우리는 tests 모듈 내에 일반적인 시나리오를 셋업 하거나 일반적인 연산을 수행하는 것을 돕기 위한 테스트 아닌 함수를 넣을 수 있으므로, 어떤 함수가 테스트 함수인지 #[test]를 이용하여 나타낼 필요가 있습니다.
- cargo test커맨드는 프로젝트에 있는 모든 테스트를 실행합니다.

```
$ cargo test
   Compiling adder v0.1.0 (file:///projects/adder)
    Finished dev [unoptimized + debuginfo] target(s) in 0.22 secs
     Running target/debug/deps/adder-ce99bcc2479f4607

running 1 test
test tests::it_works ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests adder

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

- cargo는 테스트를 컴파일하고 실행했습니다. Compiling, Finished, 그리고 Running 라인 이후에는 running 1 test 라인이 있습니다.
- 그다음 라인에는 생성된 테스트 함수의 이름인 it_works가 나타나고, 테스트의 실행 결과 ok가 나타납니다. 그러고 나서 테스트 실행의 전체 요약이 나타납니다. test result: ok.는 모든 테스트가 통과했다는 뜻입니다. 1 passed; 0 failed는 통과하거나 실패한 테스트의 개수를 추가적으로 보여줍니다.
- 무시하라고 표시한 테스트가 없기 때문에, 요약문에 0 ignored라고 표시됩니다
- 0 measured 통계는 성능을 측정하는 벤치마크 테스트를 위한 것입니다. 벤치마크 테스트는 이 글이 쓰인 시점에서는 오직 나이틀리(nightly) 러스트에서만 사용 가능합니다
- Doc-tests adder로 시작하는 테스트 출력의 다음 부분은 문서 테스트의 결과를 보여주기 위한 것입니다. 아직 어떠한 문서 테스트도 없긴 하지만, 러스트는 우리의 API 문서 내에 나타난 어떠한 코드 예제라도 컴파일할 수 있습니다. 이 기능은 우리의 문서와 코드가 동기화를 유지하도록 돕습니다

```rs
#[cfg(test)]
mod tests {
    #[test]
    fn exploration() {
        assert_eq!(2 + 2, 4);
    }
}
```

- cargo test를 다시 실행시킵니다. 이제 출력 부분에서 it_works 대신 exploration을 볼 수 있을 것입니다:

```
running 1 test
test tests::exploration ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

- 각 테스트는 새로운 스레드 내에서 실행되며, 테스트 스레드가 죽은 것을 메인 스레드가 알게 되면, 테스트는 실패한 것으로 표시됩니다

```rs
#[cfg(test)]
mod tests {
    #[test]
    fn exploration() {
        assert_eq!(2 + 2, 4);
    }

    #[test]
    fn another() {
        panic!("Make this test fail");
    }
}
```

```
running 2 tests
test tests::exploration ... ok
test tests::another ... FAILED

failures:

---- tests::another stdout ----
    thread 'tests::another' panicked at 'Make this test fail', src/lib.rs:10:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.

failures:
    tests::another

test result: FAILED. 1 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out

error: test failed

```

- test tests::another 라인은 ok 대신 FAILED을 보여줍니다. 개별 결과 부분과 요약 부분 사이에 새로운 두 개의 섹션이 나타납니다: 첫번째 섹션은 테스트 실패에 대한 구체적인 이유를 표시합니다
- 다음 섹션은 실패한 모든 테스트의 이름만 목록화한 것인데, 이는 테스트들이 많이 있고 구체적인 테스트 실패 출력이 많을 때 유용합니다. 실패하는 테스트의 이름은 이를 더 쉽게 디버깅하기 위해서 해당 테스트만을 실행시키는데 사용될 수 있습니다
- 요약 라인이 가장 마지막에 표시됩니다: 전체적으로, 우리의 테스트 결과는 FAILED입니다

## assert! 매크로를 이용하여 결과 확인하기

- 표준 라이브러리에서 제공하는 assert! 매크로는 여러분이 테스트가 어떤 조건이 true임을 보장하기를 원하는 경우 유용합니다.
- assert! 매크로에는 부울린 타입으로 계산되는 인자가 제공됩니다. 만일 값이 true라면 assert!는 아무일도 하지 않고 테스트는 통과됩니다. 만일 값이 false라면, assert!는 panic! 매크로를 호출하는데, 이것이 테스트를 실패하게 합니다

```rs
#[derive(Debug)]
pub struct Rectangle {
    length: u32,
    width: u32,
}

impl Rectangle {
    pub fn can_hold(&self, other: &Rectangle) -> bool {
        self.length > other.length && self.width > other.width
    }
}
```

- can_hold 메소드는 부울린 값을 반환하는데, 이는 assert! 매크로를 위한 완벽한 사용 사례라는 의미입니다

```rs
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn larger_can_hold_smaller() {
        let larger = Rectangle { length: 8, width: 7 };
        let smaller = Rectangle { length: 5, width: 1 };

        assert!(larger.can_hold(&smaller));
    }
}
```

- 사각형이 작은 사각형을 정말로 담을 수 있는지 검사하는 can_hold를 위한 테스트
- use super::\*;. tests 모듈은 보통의 가시성 규칙을 따르는 일반적인 모듈입니다. 우리가 내부 모듈 내에 있기 때문에, 외부 모듈에 있는 코드를 내부 모듈의 스코프로 가져올 필요가 있습니다
- 여기서는 글롭(\*)을 사용하기로 선택했고 따라서 우리가 외부 모듈에 정의한 어떠한 것이듯 이 tests모듈에서 사용 가능합니다

```
running 1 test
test tests::larger_can_hold_smaller ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

```

```rs
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn larger_can_hold_smaller() {
        // --snip--
    }

    #[test]
    fn smaller_cannot_hold_larger() {
        let larger = Rectangle { length: 8, width: 7 };
        let smaller = Rectangle { length: 5, width: 1 };

        assert!(!smaller.can_hold(&larger));
    }
}
```

- can_hold 함수의 올바른 결과값은 false이므로, assert! 매크로에게 넘기기 전에 이 결과를 반대로 만들 필요가 있습니다. 결과적으로, 우리의 테스트는 can_hold가 false를 반환할 경우에만 통과할 것입니다:

```
running 2 tests
test tests::smaller_cannot_hold_larger ... ok
test tests::larger_can_hold_smaller ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

-통과하는 테스트가 두 개가 되었습니다! 이제는 만약 우리의 코드에 버그가 있을 때는 테스트 결과가 어찌되는지 확인합니다

- can_hold 메소드의 구현 부분 중 큰(>) 부등호를 이용해 길이를 비교하는 부분을 작은(<) 부등호로 바꿉니다

```rs
// --snip--

impl Rectangle {
    pub fn can_hold(&self, other: &Rectangle) -> bool {
        self.length < other.length && self.width > other.width
    }
}
```

```
running 2 tests
test tests::smaller_cannot_hold_larger ... ok
test tests::larger_can_hold_smaller ... FAILED

failures:

---- tests::larger_can_hold_smaller stdout ----
    thread 'tests::larger_can_hold_smaller' panicked at 'assertion failed:
    larger.can_hold(&smaller)', src/lib.rs:22:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.

failures:
    tests::larger_can_hold_smaller

test result: FAILED. 1 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```

- 테스트가 버그를 찾았습니다! larger.length는 8이고 smaller.length는 5이므로, can_hold의 길이 부분에 대한 비교값은 이제 false를 반환합니다

## aseert_eq!와 assert_ne!를 이용한 동치(equality) 테스트

- 기능성을 테스트하는 일반적인 방법은 테스트 내의 코드의 결과값과 우리가 기대하는 값을 비교하여 둘이 서로 같은지를 확실히 하는 것입니다. 이를 assert! 매크로에 ==를 이용한 표현식을 넘기는 식으로 할 수도 있습니다.
- 그러나 이러한 테스트를 더 편리하게 수행해주는 표준 라이브러리가 제공하는 한 쌍의 매크로 - assert_eq!와 assert_ne! - 가 있습니다. 이 매크로들은 각각 동치(equality)와 부동(inequality)을 위해 두 인자를 비교합니다. 또한 이들은 만일 단언에 실패한다면 두 값을 출력해 주는데, 이는 왜 테스트가 실패했는지를 포기 더 쉬워집니다;

```rs
pub fn add_two(a: i32) -> i32 {
    a + 2
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_adds_two() {
        assert_eq!(4, add_two(2));
    }
}
```

```
running 1 test
test tests::it_adds_two ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

- assert_eq! 매크로에 제공한 첫번째 인자 4는 add_two(2) 호출의 결과와 동일합니다. 이 테스트에 대한 라인은 test tests::it_adds_two ... ok이고, ok 문자열은 테스트가 통과했음을 나타냅니다
- assert_eq!를 이용하는 테스트가 실패했을때는 어떻게 보이는지를 알아보기 위해 테스트에 버그를 집어넣어 봅시다. add_two 함수에 3을 대신 더하는 형태로 구현을 변경해 보세요:

```rs
pub fn add_two(a: i32) -> i32 {
    a + 3
}
```

```
running 1 test
test tests::it_adds_two ... FAILED

failures:

---- tests::it_adds_two stdout ----
        thread 'tests::it_adds_two' panicked at 'assertion failed: `(left == right)`
  left: `4`,
 right: `5`', src/lib.rs:11:8

failures:
    tests::it_adds_two

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```

- it_adds_two 테스트는 assertion failed: `(left == right)`라는 메세지와 left는 4였고 right는 5였다는 것으로 보여줌과 함께 실패메세지를 만들어 낼것입니다.
- assert_ne! 매크로는 우리가 제공한 두 개의 값이 서로 갖지 않으면 통과하고 동일하면 실패할 것입니다. 이 매크로는 어떤 값이 될 것인지는 정확히 확신하지 못하지만, 어떤 값이라면 절대로 될 수 없는지는 알고 있을 경우에 가장 유용합니다.
- 표면 아래에서, assert_eq!와 assert_ne! 매크로는 각각 ==과 != 연산자를 이용합니다. 단언에 실패하면, 이 매크로들은 디버그 포맷팅을 사용하여 인자들을 출력하는데, 이는 비교되는 값들이 PartialEq와 Debug 트레잇을 구현해야 한다는 의미입니다
- . 모든 기본 타입과 표준 라이브러리가 제공하는 대부분의 타입들은 이 트레잇들을 구현하고 있습니다. 여러분이 정의한 구조체나 열거형에 대해서, 해당 타입의 값이 서로 같은지 혹은 다른지를 단언하기 위해서는 PartialEq를 구현할 필요가 있습니다
- 단언에 실패할 경우에 값을 출력하기 위해서는 Debug를 구현해야 합니다. 5장에서 설명한 바와 같이 두 트레잇 모두 추론 가능한(derivable) 트레잇이기 때문에, 이 트레잇의 구현은 보통 #[derive(PartialEq, Debug)] 어노테이션을 여러분의 구조체나 열거형 정의부에 추가하는 정도로 간단합니다

## 커스텀 실패 메세지 추가하기

- assert!, assert_eq! 및 assert_ne! 매크로의 추가 인자로서 커스텀 메세지를 입력하여 실패 메세지와 함께 출력되도록 할 수 있습니다
- assert!가 요구하는 하나의 인자 후에 지정된 인자들이나 assert_eq!와 assert_ne!가 요구하는 두 개의 인자 후에 지정된 인자들은+ 연산자나 format! 매크로에 넘겨지므로, 여러분은 {} 변경자 (placeholder)를 갖는 포맷 스트링과 이 변경자에 입력될 값들을 넘길 수 있습니다

```rs
pub fn greeting(name: &str) -> String {
    format!("Hello {}!", name)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn greeting_contains_name() {
        let result = greeting("Carol");
        assert!(result.contains("Carol"));
    }
}
```

- 프로그램의 요구사항은 아직 합의되지 않았고, 인사말의 시작 지점에 있는 Hello 텍스트가 변경될 것이라는 점이 꽤나 확실한 상태라고 칩시다. 우리는 그런 변경사항이 생기더라도 이름에 대한 테스트를 갱신할 필요는 없다고 결정했고, 따라서 greeting 함수로부터 반환된 값과 정확히 일치하는 체크 대신, 출력 값이 입력 파라미터의 텍스트를 포함하고 있는지만 단언할 것입니다.

```rs
pub fn greeting(name: &str) -> String {
    String::from("Hello!")
}
```

```
running 1 test
test tests::greeting_contains_name ... FAILED

failures:

---- tests::greeting_contains_name stdout ----
    thread 'tests::greeting_contains_name' panicked at 'assertion failed:
    result.contains("Carol")', src/lib.rs:12:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.

failures:
    tests::greeting_contains_name
```

- 이 결과는 그저 단언이 실패했으며 몇 번째 줄의 단언이 실패했는지만을 나타냅니다. 이 경우에서 더 유용한 실패 메세지는 greeting 함수로부터 얻은 값을 출력하는 것일 테지요.

```rs
#[test]
fn greeting_contains_name() {
    let result = greeting("Carol");
    assert!(
        result.contains("Carol"),
        "Greeting did not contain name, value was `{}`", result
    );
}

```

- 이제 테스트를 다시 실행시키면, 더 많은 정보를 가진 에러 메세지를 얻을 것입니다:

```
---- tests::greeting_contains_name stdout ----
    thread 'tests::greeting_contains_name' panicked at 'Greeting did not contain
    name, value was `Hello!`', src/lib.rs:12:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```

## should_panic을 이용한 패닉에 대한 체크

- 코드가 우리가 기대한 정확한 값을 반환하는 것을 체크하는 것에 더하여, 우리의 코드가 우리가 기대한 대로 에러가 나는 경우를 처리할 수 있는지 체크하는 것 또한 중요합니다
- should_panic 이 속성은 함수 내의 코드가 패닉을 일으키면 테스트가 통과하도록 만들어줍니다; 함수 내의 코드가 패닉을 일으키지 않는다면 테스트는 실패할 것입니다.

```rs
pub struct Guess {
    value: u32,
}

impl Guess {
    pub fn new(value: u32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {}.", value);
        }

        Guess {
            value
        }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic]
    fn greater_than_100() {
        Guess::new(200);
    }
}
```

```
running 1 test
test tests::greater_than_100 ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

- new 함수가 100 이상의 값일 때 패닉을 발생시키는 조건을 제거함으로써 코드에 버그를 넣어봅시다:

```rs
impl Guess {
    pub fn new(value: u32) -> Guess {
        if value < 1  {
            panic!("Guess value must be between 1 and 100, got {}.", value);
        }

        Guess {
            value
        }
    }
}
```

```
running 1 test
test tests::greater_than_100 ... FAILED

failures:

failures:
    tests::greater_than_100

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```

- 이 경우에는 그다지 쓸모 있는 메세지를 얻지 못하지만, 한번 테스트 함수를 살펴보게 되면, 함수가 #[should_panic]으로 어노테이션 되었다는 것을 볼 수 있습니다. 우리가 얻은 실패는 함수 내의 코드가 패닉을 일으키지 않았다는 의미가 됩니다.
- should_panic 테스트는 애매할 수 있는데, 그 이유는 이 속성이 단지 코드에서 어떤 패닉이 유발되었음만을 알려줄 뿐이기 때문입니다. should_panic 테스트는 일어날 것으로 예상한 것 외의 다른 이유로 인한 패닉이 일어날 지라도 통과할 것입니다. should_panic 테스트를 더 엄밀하게 만들기 위해서, should_panic 속성에 expected 파라미터를 추가할 수 있습니다

```rs
// --snip

impl Guess {
    pub fn new(value: u32) -> Guess {
        if value < 1 {
            panic!("Guess value must be greater than or equal to 1, got {}.",
                   value);
        } else if value > 100 {
            panic!("Guess value must be less than or equal to 100, got {}.",
                   value);
        }

        Guess {
            value
        }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic(expected = "Guess value must be less than or equal to 100")]
    fn greater_than_100() {
        Guess::new(200);
    }
}
```

- 이 테스트는 통과할 것인데, 그 이유는 should_panic 속성에 추가한 expected 파라미터 값이 Guess::new 함수가 패닉을 일으킬 때의 메세지의 서브 스트링이기 때문입니다
- 우리가 예상하는 전체 패닉 메세지로 특정할 수도 있는데, 그러한 경우에는 Guess value must be less than or equal to 100, got 200.이 되겠지요. 여러분이 should_panic에 대한 기대하는 파라미터를 특정하는 것은 패닉 메세지가 얼마나 유일한지 혹은 유동적인지, 그리고 여러분의 테스트가 얼마나 정확하기를 원하는지에 따라서 달라집니다. 위의 경우, 패닉 메세지의 서브 스트링은 실행된 함수의 코드가 else if value > 100 경우에 해당함을 확신하기에 충분합니다.

```rs
if value < 1 {
    panic!("Guess value must be less than or equal to 100, got {}.", value);
} else if value > 100 {
    panic!("Guess value must be greater than or equal to 1, got {}.", value);
}
```

```
running 1 test
test tests::greater_than_100 ... FAILED

failures:

---- tests::greater_than_100 stdout ----
        thread 'tests::greater_than_100' panicked at 'Guess value must be greater than or equal to 1, got 200.', src/lib.rs:11:12
note: Run with `RUST_BACKTRACE=1` for a backtrace.
note: Panic did not include expected string 'Guess value must be less than or
equal to 100'

failures:
    tests::greater_than_100

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```

- 실패 메세지는 이 테스트가 우리 예상에 맞게 실제로 패닉에 빠지기는 했으나, 패닉 메세지가 예상하는 스트링을 포함하지 않고 있다고 말하고 있습니다 (did not include expected string 'Guess value must be less than or equal to 100'.) 우리가 얻어낸 패닉 메세지를 볼 수 이는데, 이 경우에는 Guess value must be greater than or equal to 1, got 200. 이었습니다.

## 2. 테스트의 실행방식 제어하기

- cargo run이 여러분의 코드를 컴파일하고 난 뒤 그 결과인 바이너리를 실행하는 것과 마찬가지로, cargo test는 여러분의 코드를 테스트 모드에서 컴파일하고 결과로 발생한 테스트 바이너리를 실행합니다
- 커맨드 라인 옵션을 지정하여 cargo test의 기본 동작을 변경할 수 있습니다
- 어떤 커맨드 라인 옵션은 cargo test에 입력되고 어떤 옵션은 결과 테스트 바이너리에 입력됩니다. 이 두 가지 타입의 인자를 구분하기 위해서, cargo test에 주어질 인자를 먼저 나열하고, 그다음 구분자(separator)로 --를 넣고, 그 뒤 테스트 바이너리에 입력될 인자를 나열합니다. cargo test --help를 실행하는 것은 cargo test에서 사용할 수 있는 옵션을 표시하고, cargo test -- --help를 실행하는 것은 구분자 -- 이후에 나올 수 있는 옵션을 표시합니다.

## 테스트를 병렬 혹은 연속으로 실행하기

- 여러 개의 테스트를 실행할 때는, 기본적으로 스레드를 이용하여 병렬적으로 수행됩니다. 이는 테스트가 더 빠르게 실행되어 끝낼 수 있다는 의미이므로, 우리의 코드가 잘 동작하는지 혹은 그렇지 않은지에 대한 피드백을 더 빨리 얻을 수 있습니다.
- 작성한 테스트 각각이 test-output.txt라는 파일을 디스크에 만들고 이 파일에 어떤 데이터를 쓰는 코드를 실행한다고 가정해봅시다. 그런 다음 각 테스트는 그 파일로부터 데이터를 읽고, 이 파일이 특정한 값을 담고 있는지 단언하는데, 이 값들은 테스트마다 다릅니다. 모든 테스트들이 동시에 실행되기 때문에, 어떤 테스트가 파일을 쓰고 읽는 동안 다른 테스트가 파일을 덮어쓸지도 모릅니다.
- 두 번째 테스트는 실패할 것인데, 이는 코드가 정확히 않아서가 아니라 테스트들이 병렬적으로 실행하는 동안 서로에게 간섭을 일으켰기 때문입니다. 한 가지 해결책은 각 테스트가 서로 다른 파일을 쓰도록 확실히 하는 것일 겁니다; 또 다른 해결책은 테스트를 한 번에 하나씩만 실행하는 것입니다.

```
$ cargo test -- --test-threads=1
```

- 여기서는 테스트 스레드의 개수에 1을 지정했는데, 이는 프로그램이 어떠한 병렬 처리도 사용하지 않음을 얘기해줍니다

## 함수 결과 보여주기

- 기본적으로 어떤 테스트가 통과하면, 러스트의 테스트 라이브러리는 표준 출력(standard output)으로 출력되는 어떤 것이든 캡처합니다. 예를 들면, 우리가 테스트 내에서 println!을 호출하고 이 테스트가 통과하면, println! 출력을 터미널에서 볼 수 없습니다
- 우리는 오직 그 테스트가 통과되었다고 표시된 라인만 볼 뿐입니다. 만일 테스트가 실패하면, 실패 메세지 아래에 표준 출력으로 출력되었던 어떤 것이든 보게 될 것입니다.

```rs
fn prints_and_returns_10(a: i32) -> i32 {
    println!("I got the value {}", a);
    10
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn this_test_will_pass() {
        let value = prints_and_returns_10(4);
        assert_eq!(10, value);
    }

    #[test]
    fn this_test_will_fail() {
        let value = prints_and_returns_10(8);
        assert_eq!(5, value);
    }
}
```

```
running 2 tests
test tests::this_test_will_pass ... ok
test tests::this_test_will_fail ... FAILED

failures:

---- tests::this_test_will_fail stdout ----
        I got the value 8
thread 'tests::this_test_will_fail' panicked at 'assertion failed: `(left == right)`
  left: `5`,
 right: `10`', src/lib.rs:19:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.

failures:
    tests::this_test_will_fail

test result: FAILED. 1 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out

```

- I got the value 4라는 메세지를 어디에서도 볼 수 없는데, 이는 성공하는 테스트가 실행시키는 출력이라는 점을 주목하세요. 이 출력 메세지는 캡처되었습니다. 실패한 테스트로부터 얻어진 출력 메세지인 I got the value 8은 테스트 정리 출력 부분에 나타나는데, 이는 테스트 실패 원인 또한 함께 보여줍니다.
- 만일 성공하는 테스트에 대한 출력 값 또한 볼 수 있기를 원한다면, --nocapture 플래그를 이용하여 출력 캡처 동작을 비활성화시킬 수 있습니다:

```
$ cargo test -- --nocapture

```

```
running 2 tests
I got the value 4
I got the value 8
test tests::this_test_will_pass ... ok
thread 'tests::this_test_will_fail' panicked at 'assertion failed: `(left == right)`
  left: `5`,
 right: `10`', src/lib.rs:19:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.
test tests::this_test_will_fail ... FAILED

failures:

failures:
    tests::this_test_will_fail

test result: FAILED. 1 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```

## 이름으로 테스트의 일부분만 실행하기

- 가끔, 모든 테스트 셋을 실행하는 것은 시간이 오래 걸릴 수 있습니다. 만일 여러분이 특정 영역의 코드에 대해서 작업하고 있다면, 그 코드와 연관된 테스트만 실행시키고 싶어 할 수도 있습니다.

```rs

pub fn add_two(a: i32) -> i32 {
    a + 2
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn add_two_and_two() {
        assert_eq!(4, add_two(2));
    }

    #[test]
    fn add_three_and_two() {
        assert_eq!(5, add_two(3));
    }

    #[test]
    fn one_hundred() {
        assert_eq!(102, add_two(100));
    }
}
```

```
running 3 tests
test tests::add_two_and_two ... ok
test tests::add_three_and_two ... ok
test tests::one_hundred ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

- 단 하나의 테스트만 실행시키기 위해 cargo test에 그 테스트 함수의 이름을 넘길 수 있습니다:

```
$ cargo test one_hundred
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running target/debug/deps/adder-06a75b4a1f2515e9

running 1 test
test tests::one_hundred ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out
```

- 테스트 이름들 중에서 두 개가 add를 포함하므로, cargo test add라고 실행하여 이 두 개의 테스트를 실행시킬 수 있습니다:

```
$ cargo test add
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running target/debug/deps/adder-06a75b4a1f2515e9

running 2 tests
test tests::add_two_and_two ... ok
test tests::add_three_and_two ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 1 filtered out
```

- 이는 add가 이름에 포함된 모든 테스트를 실행시켰고 one_hundred라는 이름의 테스트를 걸러냈습니다. 또한 테스트가 있는 모듈이 테스트의 이름의 일부가 되어 있으므로, 모듈의 이름으로 필터링하여 그 모듈 내의 모든 테스트를 실행시킬 수 있습니다

## 특별한 요청이 없는 한 몇몇 테스트들 무시하기

- 이따금씩 몇몇 특정 테스트들은 실행하는데 너무나 시간이 많이 소모될 수 있어서, 여러분은 cargo test의 실행 시 이 테스트들을 배제하고 싶어 할지도 모릅니다.
- 모든 테스트들을 인자로서 열거하는 것 대신, 다음과 같이 시간이 많이 걸리는 테스트들에 ignore 속성을 어노테이션하여 이들을 배제시킬 수 있습니다:

```rs

#[test]
fn it_works() {
    assert_eq!(2 + 2, 4);
}

#[test]
#[ignore]
fn expensive_test() {
    // code that takes an hour to run
}
```

- 배제시키고 싶은 테스트에 대하여 #[test] 다음 줄에 #[ignore]를 추가하였습니다. 이제 우리의 테스트들을 실행시키면, it_works가 실행되는 것은 보이지만, expensive-test는 실행되지 않는 것을 볼 수 있습니다:

```
$ cargo test
   Compiling adder v0.1.0 (file:///projects/adder)
    Finished dev [unoptimized + debuginfo] target(s) in 0.24 secs
     Running target/debug/deps/adder-ce99bcc2479f4607

running 2 tests
test expensive_test ... ignored
test it_works ... ok

test result: ok. 1 passed; 0 failed; 1 ignored; 0 measured; 0 filtered out
```

- expensive_test는 ignored 리스트에 포함되었습니다. 만일 무시된 테스트들만 실행시키고 싶다면, cargo test -- --ignored라고 실행함으로써 이를 요청할 수 있습니다.

```
$ cargo test -- --ignored
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running target/debug/deps/adder-ce99bcc2479f4607

running 1 test
test expensive_test ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 1 filtered out

```

## 3,테스트 조직화

- 테스팅은 복잡한 분야이고, 여러 사람들이 서로 다른 용어와 조직화 방식을 이용합니다. 러스트 커뮤니티에서는 테스트에 대해서 두 개의 주요한 카테고리로 나눠 생각합니다: 단위 테스트(unit test) 그리고 *통합 테스트(integration test)*입니다. 단위 테스트는 작고 하나에 더 집중하며, 한 번에 하나의 모듈만 분리하여 테스트하고, 비공개 인터페이스 (private interface)를 테스트합니다
- 통합 테스트는 완전히 여러분의 라이브러리 외부에 있으며, 공개 인터페이스 (public interface)를 이용하고 테스트마다 여러 개의 모듈을 잠재적으로 실험함으로써, 다른 외부의 코드가 하는 방식과 동일한 형태로 여러분의 코드를 이용합니다.

## 단위테스트

- 단위 테스트의 목적은 각 코드의 단위를 나머지 부분과 분리하여 테스트하는 것인데, 이는 코드가 어디 있고 어느 부분이 기대한 대로 동작하지 않는지를 빠르게 정확히 찾아낼 수 있도록 하기 위함입니다
- 단위 테스트는 src 디렉토리 내에 넣는데, 각 파일마다 테스트하는 코드를 담고 있습니다. 관례는 각 파일마다 테스트 함수를 담고 있는 tests라는 이름의 모듈을 만들고, 이 모듈에 cfg(test)라고 어노테이션 하는 것입니다.
- 테스트 모듈 상의 #[cfg(test)] 어노테이션은 러스트에게 우리가 cargo build를 실행시킬 때가 아니라 cargo test를 실행시킬 때에만 컴파일하고 실행시키라고 말해줍니다. 이는 우리가 오직 라이브러리만 빌드하고 싶을 때 컴파일 시간을 절약시켜주고, 테스트가 포함되어 있지 않으므로 컴파일 결과물의 크기를 줄여줍니다.

```rs

#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}
```

- 이 코드는 자동으로 생성되는 테스트 모듈입니다. cfg 속성은 환경 설정(configuration) 을 의미하며, 러스트에게 뒤따르는 아이템이 특정한 환경 값에 대해서만 포함되어야 함을 말해줍니다.
- 환경 값이 test인데, 테스트를 컴파일하고 실행하기 위해 러스트로부터 제공되는 것입니다. 이 속성을 이용함으로써, 카고는 우리가 능동적으로 cargo test를 이용해서 테스트를 실행시킬 경우에만 우리의 테스트 코드를 컴파일합니다.
- 테스팅 커뮤니티 내에서 비공개 함수가 직접적으로 테스트되어야 하는지 혹은 그렇지 않은지에 대한 논쟁이 있었고, 다른 언어들은 비공개 함수를 테스트하는 것이 어렵거나 불가능하게 만들어두었습니다. 여러분이 어떤 테스트 이데올로기를 고수하는지와는 상관없이, 러스트의 비공개 규칙은 여러분이 비공개 함수를 테스트하도록 허용해줍니다

```rs
pub fn add_two(a: i32) -> i32 {
    internal_adder(a, 2)
}

fn internal_adder(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn internal() {
        assert_eq!(4, internal_adder(2, 2));
    }
}
```

- internal_adder 함수는 pub으로 표시되어 있지 않지만, 테스트가 그저 러스트 코드일 뿐이고 tests 모듈도 그냥 또 다른 모듈이기 때문에, internal_adder를 불러들여 호출하는 것이 가능합니다.

## 통합테스트

- 러스트에서 통합 테스트들은 완전히 여러분의 라이브러리 외부에 있습니다. 이들은 여러분의 라이브러리를 다른 코드들과 동일한 방식으로 이용하는데, 이는 이 외부 테스트들이 오직 여러분의 라이브러리의 공개 API 부분에 속하는 함수들만 호출할 수 있다는 의미입니다
- 프로젝트 디렉토리의 최상위, 그러니까 src 옆에 tests 디렉토리를 만듭니다. 카고는 이 디렉토리 내의 통합 테스트 파일들을 찾을 줄 압니다. 그런 후에는 이 디렉토리에 원하는 만큼 많은 테스트 파일을 만들 수 있으며, 카고는 각각의 파일들을 개별적인 크레이트처럼 컴파일할 것입니다.
- src/lib.rs 코드를 그대로 유지한 채로, tests 디렉토리를 만들고, tests/integration_test.rs라는 이름의 새 파일을 만듭니다

```rs
extern crate adder;

#[test]
fn it_adds_two() {
    assert_eq!(4, adder::add_two(2));
}
```

- 코드의 상단에 extern crate adder를 추가했는데, 이는 단위 테스트에서는 필요 없었지요. 이는 tests 디렉토리 내의 각 테스트가 모두 개별적인 크레이트이라서, 우리의 라이브러리를 각각에 가져올 필요가 있기 때문입니다.

```
$ cargo test
   Compiling adder v0.1.0 (file:///projects/adder)
    Finished dev [unoptimized + debuginfo] target(s) in 0.31 secs
     Running target/debug/deps/adder-abcabcabc

running 1 test
test tests::internal ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

     Running target/debug/deps/integration_test-ce99bcc2479f4607

running 1 test
test it_adds_two ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests adder

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

- 통합 테스트 섹션은 Running target/debug/deps/integration-test-ce99bcc2479f4607 이라고 말하는 라인과 함께 시작합니다
- 그다음 이 통합 테스트 안의 각 테스트 함수를 위한 라인이 있고, Doc-tests adder 섹션이 시작되기 직전에 통합 테스트의 결과를 위한 정리 라인이 있습니다.
- 통합 테스트 파일에 테스트 함수를 더 추가하는 것은 그 파일의 섹션의 라인을 더 늘릴 것입니다. 각 통합 테스트 파일은 고유의 섹션을 가지고 있으므로, 만일 우리가 tests 디렉토리에 파일을 더 추가하면, 통합 테스트 섹션이 더 생길 것입니다.
- cargo test의 인자로서 테스트 함수의 이름을 명시하는 식으로 특정 통합 테스트 함수를 실행시키는 것도 여전히 가능합니다. 특정한 통합 테스트 파일 내의 모든 테스트를 실행시키기 위해서는, cargo test에 파일 이름을 뒤에 붙인 --test 인자를 사용하세요:

```
$ cargo test --test integration_test
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running target/debug/integration_test-952a27e0126bb565

running 1 test
test it_adds_two ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

- 테스트하는 기능별로 테스트 함수들을 묶을 수 있습니다. 앞서 언급했듯이, tests 디렉토리 내의 각 파일은 고유의 개별적인 크레이트인 것처럼 컴파일됩니다.
- 각 통합 테스트 파일을 고유한 크레이트인 것 처럼 다루는 것은 여러분의 크레이트를 이용하게 될 사용자들의 방식과 더 유사하게 분리된 스코프를 만들어 내기에 유용합니다.
- 하지만, 이는 src 내의 파일들이 동일한 동작을 공유하는 것을 tests 디렉토리 내의 파일들에서는 할 수 없음을 의미합니다
- 만일 여러분이 여러 개의 통합 테스트 파일들 내에서 유용하게 사용될 헬퍼 함수들 묶음을 가지고 있으며, 이들을 공통 모듈로 추출하기 위해 7장의 "모듈을 다른 파일로 옮기기"절에 있는 단계를 따르는 시도를 한다면, 이러한 tests 디렉토리 내의 파일에 대한 이색적인 동작 방식은 가장 주목할 만 점입니다

```rs
pub fn setup() {
    // 여러분의 라이브러리 테스트에 특화된 셋업 코드가 여기 올 것입니다
}
```

- 만약 테스트를 다시 실행시키면, 비록 이 코드가 어떠한 테스트 함수도 담고 있지 않고, setup 함수를 다른 어딘가에서 호출하고 있지 않을지라도, common.rs 파일을 위한 테스트 출력 내의 새로운 섹션을 보게 될 것입니다:

```
running 1 test
test tests::internal ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

     Running target/debug/deps/common-b8b07b6f1be2db70

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

     Running target/debug/deps/integration_test-d993c68b431d39df

running 1 test
test it_adds_two ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests adder

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

- running 0 tests이 표시되는 테스트 출력이 보이는 common을 만드는 건 우리가 원하던 것이 아닙니다. 우리는 그저 다른 통합 테스트 파일들에서 어떤 코드를 공유할 수 있기를 원했지요.
- common이 테스트 출력에 나타나는 것을 막기 위해서는, tests/common.rs을 만드는 대신, tests/common/mod.rs를 만듭니다
- 서브모듈을 가지고 있는 모듈의 파일들을 위해 module_name/mod.rs라는 이름 규칙을 이용했었고, 여기서 common에 대한 서브모듈을 가지고 있지는 않지만, 이러한 방식으로 파일명을 정하는 것이 러스트에게 common 모듈을 통합 테스트 파일로 취급하지 않게끔 전달해줍니다.
- setup 함수 코드를 tests/common/mod.rs로 옮기고 tests/common.rs 파일을 제거하면, 테스트 출력에서 해당 섹션이 더 이상 나타나지 않을 것입니다. tests 디렉토리의 서브 디렉토리 내의 파일들은 개별적인 크레이트처럼 컴파일되지도, 테스트 출력의 섹션을 갖지도 않습니다.
- tests/common/mod.rs를 만든 뒤에는, 어떤 통합 테스트 파일에서라도 이를 모듈처럼 쓸 수 있습니다

```rs
extern crate adder;

mod common;

#[test]
fn it_adds_two() {
    common::setup();
    assert_eq!(4, adder::add_two(2));
}
```

- 만약 우리의 프로젝트가 src/lib.rs 파일이 없고 src/main.rs 파일만 갖고 있는 바이너리 프로젝트라면, tests 디렉토리 내에 통합 테스트를 만들어서 src/main.rs에 정의된 함수를 가져오기 위하여 extern crate를 이용할 수 없습니다
- 오직 라이브러리 크레이트만 다른 크레이트에서 호출하고 사용할 수 있는 함수들을 노출시킵니다; 바이너리 크레이트는 그 스스로 실행될 것으로 여겨집니다.
- 이는 바이너리를 제공하는 러스트 프로젝트들이 src/lib.rs에 위치한 로직을 호출하는 간단한 형태의 src/main.rs를 가지고 있는 이유 중 하나입니다. 이러한 구조와 함께라면, extern crate를 이용하여 중요한 기능들을 커버하도록 하기 위해 통합 테스트가 라이브러리 크레이트를 테스트할 수 있습니다. 만일 중요 기능이 작동한다면, src/main.rs 내의 소량의 코드 또한 동작할 것이고, 이 소량의 코드는 테스트할 필요가 없습니다.
