# Generictype

- 모든 프로그래밍 언어는 컨셉의 복제를 효율적으로 다루기 위한 도구를 가지고 있습니다.
- 제네릭은 구체화된 타입이나 다른 속성들에 대하여 추상화된 대리인 입니다.
- 코드를 작성하고 컴파일 할떄 제네릭이 어떻게 완성되는지 알 필요없이
  제네릭의 동작 혹은 어떻게 다른 제네릭과 어떻게 연관되는지와 같은 제네릭에 대한 속성을 표현 할수 있습니다.
- 여러 개의 구체화된 값들에 대해 실행될 코드를 작성하기 위해서 함수가 어떤 값을 담을지 알 수 없는 파라미터를 갖는 것과 동일한 방식으로, i32나 String과 같은 구체화된 타입 대신 몇몇 제네릭 타입의 파라미터를 갖는 함수를 작성할 수 있습니다
- 트레잇은 동작을 제네릭 한 방식으로 정의하는 방법을 말합니다.트레잇은 제네릭 타입과 결합되어 제네릭 타입에 대해 아무 타입이나 허용하지 않고, 특정 동작을 하는 타입으로 제한할 수 있습니다.
- 라이프타임은 제네릭의 일종으로서 우리가 컴파일러에게 참조자들이 서로에게 어떤 연관이 있는지에 대한 정보를 줄 수 있도록 해줍니다. 라이프타임은 수많은 상황에서 값을 빌릴 수 있도록 허용해 주고도 여전히 참조자들이 유효할지를 컴파일러가 검증하도록 해주는 러스트의 지능입니다.

## 함수를 추출하여 중복없애기

```rs
fn main(){
    let numbers= vec![34,50,25,100,65];
    let mut largest = numbers[0];

    for number in numbers{
        if number >largest{
            largest= number;
        }
    }
}
```

- 숫자 리스트 중에서 가장 큰 수를 찾는 코드
- 이 코드는 정수의 리스트를 얻는데 여기서는 변수 numbers에 저장되었습니다.
- 첫 번째 아이템을 largest라는 이름의 변수에 우선 집어넣습니다
- 그러고 나서 리스트 내의 모든 숫자들에 대해 반복 접근을 하는데, 만일 현재 숫자가 largest 내에 저장된 숫자보다 더 크다면, 이 숫자로 largest 내의 값을 변경합니다. 만일 현재 숫자가 여태까지 본 가장 큰 값보다 작다면, largest는 바뀌지 않습니다. 리스트 내의 모든 아이템을 다 처리했을 때, largest는 가장 큰 값을 가지고 있을 것인데, 위 코드의 경우에는 100이 될 것입니다.

```rs
fn main() {
    let numbers = vec![34, 50, 25, 100, 65];

    let mut largest = numbers[0];

    for number in numbers {
        if number > largest {
            largest = number;
        }
    }

    println!("The largest number is {}", largest);

    let numbers = vec![102, 34, 6000, 89, 54, 2, 43, 8];

    let mut largest = numbers[0];

    for number in numbers {
        if number > largest {
            largest = number;
        }
    }

    println!("The largest number is {}", largest);
}
```

- 두 개의 숫자 리스트에서 가장 큰 숫자를 찾는 코드
- 이 코드는 잘 동작하지만, 코드를 중복 적용하는 일은 지루하고 오류가 발생하기도 쉬우며, 또한 로직을 바꾸고 싶다면 이 로직을 갱신할 곳이 여러 군데가 된다는 의미이기도 합니다.
- 이러한 중복을 제거하기 위해서 우리는 추상화를 쓸 수 있는데, 이 경우에는 어떠한 정수 리스트가 함수의 파라미터로 주어졌을 때 동작하는 함수의 형태가 될 것입니다.

```rs
fn largest(list: &[i32]) -> i32 {
    let mut largest = list[0];

    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let numbers = vec![34, 50, 25, 100, 65];

    let result = largest(&numbers);
    println!("The largest number is {}", result);

    let numbers = vec![102, 34, 6000, 89, 54, 2, 43, 8];

    let result = largest(&numbers);
    println!("The largest number is {}", result);
}

```

- 이 함수는 list라는 파라미터를 갖고 있는데, 이것이 함수로 넘겨질 구체적인 임의 i32 값들의 슬라이스를 나타냅니다. 함수 정의 내의 코드는 임의의 &[i32]의 list 표현에 대해 동작합니다. largest 함수를 호출할 때, 이 코드는 실제로 우리가 넘겨준 구체적인 값에 대해 실행됩니다.

```
중복된 코드가 있음을 알아챘습니다.
중복된 코드를 함수의 본체로 추출하고, 함수의 시그니처 내에 해당 코드의 입력값 및 반환 값을 명시했습니다.
두 군데의 코드가 중복되었던 구체적인 지점에 함수 호출을 대신 집어넣었습니다.
```

## 함수 정의 내에서 제네릭데이터 타입을 이용하기

- largest 함수로 계속 진행하면, Listing 10-4는 슬라이스 내에서 가장 큰 값을 찾는 동일한 기능을 제공하는 두 함수를 보여주고 있습니다. 첫 번째 함수는 Listing 10-3에서 추출한 슬라이스에서 가장 큰 i32를 찾는 함수입니다. 두 번째 함수는 슬라이스에서 가장 큰 char를 찾습니다:

```rs
fn largest_i32(list: &[i32]) -> i32 {
    let mut largest = list[0];

    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn largest_char(list: &[char]) -> char {
    let mut largest = list[0];

    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let numbers = vec![34, 50, 25, 100, 65];

    let result = largest_i32(&numbers);
    println!("The largest number is {}", result);

    let chars = vec!['y', 'm', 'a', 'q'];

    let result = largest_char(&chars);
    println!("The largest char is {}", result);
}
```

- 우리가 정의하고자 하는 함수의 시그니처 내에 있는 타입들을 파라미터화 하기 위해서, 타입 파라미터를 위한 이름을 만들 필요가 있는데, 이는 값 파라미터들의 이름을 함수에 제공하는 방법과 유사합니다. 우리는 T라는 이름을 선택할 겁니다.
- 어떤 식별자(identifier)든지 타입 파라미터의 이름으로 사용될 수 있지만, 러스트의 타입 이름에 대한 관례가 낙타 표기법(CamelCase)이기 때문에 T를 사용하려고 합니다. 제네릭 타입 파라미터의 이름은 또한 관례상 짧은 경향이 있는데, 종종 그냥 한 글자로 되어 있습니다. "type"을 줄인 것으로서, T가 대부분의 러스트 프로그래머의 기본 선택입니다.
- 우리가 정의하고자 하는 제네릭 largest 함수의 함수 시그니처는 아래와 같이 생겼습니다:

```rs
fn largest<T>(list:&[T])->T{}
```

- 함수 largest는 어떤 타입 T을 이용한 제네릭입니다. 이것은 list라는 이름을 가진 하나의 파라미터를 가지고 있고, list의 타입은 T 타입 값들의 슬라이스입니다. largest 함수는 동일한 타입 T 값을 반환할 것입니다.

```rs
fn largest<T>(list: &[T]) -> T {
    let mut largest = list[0];

    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let numbers = vec![34, 50, 25, 100, 65];

    let result = largest(&numbers);
    println!("The largest number is {}", result);

    let chars = vec!['y', 'm', 'a', 'q'];

    let result = largest(&chars);
    println!("The largest char is {}", result);
}
```

- 제네릭 타입 파라미터를 이용하지만 아직 컴파일되지 않는 largest 함수의 정의
- 위 노트는 std::cmp::PartialOrd를 언급하는데, 이는 트레잇(trait) 입니다
- 이 에러가 말하고 있는 것은 T가 될 수 있는 모든 가능한 타입에 대해서 동작하지 않으리라는 것입니다: 함수 본체 내에서 T 타입의 값을 비교하고자 하기 때문에, 어떻게 순서대로 정렬하는지 알고 있는 타입만 사용할 수 있는 것입니다. 표준 라이브러리는 어떤 타입에 대해 비교 연산이 가능하도록 구현할 수 있는 트레잇인 std::cmp::PartialOrd을 정의해뒀습니다

## 구조체 정의 내에서 제네릭 데이터 타입 사용하기

```rs
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
}
```

- T 타입의 값 x와 y를 갖는 Point 구조체
- 문법은 함수 정의 내에서의 제네릭을 사용하는 것과 유사합니다. 먼저, 구조체 이름 바로 뒤에 꺾쇠괄호를 쓰고 그 안에 타입 파라미터의 이름을 선언해야 합니다. 그러면 구조체 정의부 내에서 구체적인 데이터 타입을 명시하는 곳에 제네릭 타입을 이용할 수 있습니다.
- Point의 정의 내에서 단 하나의 제네릭 타입을 사용했기 때문에, Point 구조체는 어떤 타입 T를 이용한 제네릭이고 x와 y가 이게 결국 무엇이 되든 간에 둘 다 동일한 타입을 가지고 있다고 말할 수 있습니다.
- 만일 다른 타입의 값을 갖는 Point의 인스턴스를 만들고자 한다면, 컴파일이 되지 않을 것입니다

```rs
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let wont_work = Point { x: 5, y: 4.0 };
}  //error
```

- x와 y 필드는 둘 모두 동일한 제네릭 데이터 타입 T를 가지고 있기 때문에 동일한 타입이어야 합니다
- x에 정수 5를 대입할 때, 컴파일러는 이 Point의 인스턴스에 대해 제네릭 타입 T가 정수일 것이고 알게 됩니다. 그다음 y에 대해 4.0을 지정했는데, 이 y는 x와 동일한 타입을 갖도록 정의되었으므로, 타입 불일치 에러를 얻게 됩니다.
- 만일 x와 y가 서로 다른 타입을 가지지만 해당 타입들이 여전히 제네릭인 Point 구조체를 정의하길 원한다면, 여러 개의 제네릭 타입 파라미터를 이용할 수 있습니다

```rs
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x: 1.0, y: 4.0 };
    let integer_and_float = Point { x: 5, y: 4.0 };
}
```

## 열거형 정의 내에서 제네릭 데이터 타입 사용하기

```rs
enum Option<T> {
    Some(T),
    None,
}
```

- , Option<T>는 T 타입에 제네릭인 열거형입니다. 이것은 두 개의 variant를 가지고 있습니다: 타입 T 값 하나를 들고 있는 Some, 그리고 어떠한 값도 들고 있지 않는 None variant 입니다.
- 표준 라이브러리는 구체적인 타입을 가진 이 열거형에 대한 값의 생성을 지원하기 위해서 딱 이 한 가지 정의만 가지고 있으면 됩니다. "옵션 값"의 아이디어는 하나의 명시적인 타입에 비해 더 추상화된 개념이고, 러스트는 이 추상화 개념을 수많은 중복 없이 표현할 수 있도록 해줍니다.
- 열거형은 또한 여러 개의 제네릭 타입을 이용할 수 있습니다.

```rs
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

- Result 열거형은 T와 E, 두 개의 타입을 이용한 제네릭입니다. Result는 두 개의 variant를 가지고 있습니다:
- 타입 T의 값을 들고 있는 Ok, 그리고 타입 E의 값을 들고 있는 Err입니다. 이 정의는 성공하거나 (그래서 어떤 T 값을 반환하거나) 혹은 실패하는 (그래서 E 타입으로 된 에러를 반환하는) 연산이 필요한 어디에서든 편리하게 Result 열거형을 이용하도록 해줍니다.

## 메소드 정의 내에서 제네릭 데이터 타입 사용하기

```rs
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

fn main() {
    let p = Point { x: 5, y: 10 };

    println!("p.x = {}", p.x());
}
```

- 구조체 정의 내에서의 제네릭 타입 파라미터는 여러분이 구조체의 메소드 시그니처 내에서 사용하고 싶어하는 제네릭 타입 파라미터와 항상 같지 않습니다.
- Point<T, U> 구조체 상에 mixup 이라는 메소드를 정의했습니다. 이 메소드는 또다른 Point를 파라미터로 갖는데, 이는 우리가 호출하는 mixup 상의 self의 Point와 다른 타입을 가지고 있을 수도 있습니다. 이 메소드는 새로운 Point를 생성하는데 self Point로부터 (T 타입인) x 값을 가져오고, 파라미터로 넘겨받은 Point로부터 (W 타입인) y 값을 가져온 것입니다:

```rs
struct Point<T, U> {
    x: T,
    y: U,
}

impl<T, U> Point<T, U> {
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c'};

    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}
```

- main에서, 우리는 (5 값을 갖는) x에 대해 i32를, (10.4 값을 갖는) y에 대해 f64를 사용하는 Point를 정의했습니다. p2는 ("Hello" 값을 갖는) x에 대해 스트링 슬라이스를, (c 값을 갖는) y에 대해 char를 사용하는 Point입니다. p1상에서 인자로 p2를 넘기는 mixup 호출은 p3을 반환하는데, 이는 x가 p1으로부터 오기 때문에 x는 i32 타입을 갖게 될 것입니다. 또한 y는 p2로부터 오기 때문에 p3은 y에 대해 char 타입을 가지게 될 것입니다. println!은 p3.x = 5, p3.y = c를 출력하겠지요.

제네릭 파라미터 T와 U는 impl 뒤에 선언되었는데, 이는 구조체 정의와 함께 사용되기 때문임을 주목하세요. 제네릭 파라미터 V와 W는 fn mixup 뒤에 선언되었는데, 이는 이들이 오직 해당 메소드에 대해서만 관련이 있기 때문입니다.

## 제네릭을 이용한 코드의 성능

```rs
let integer = Some(5);
let float = Some(5.0);
```

- rust는 이코드를 컴파일할떄 단일화를 실행한다
- 이 컴파일러는 Option<T>인스턴스에 사용된 값들을 읽어 두 종류의 Option<T>가 사용되고 있음을 인지한다.

```rs
enum Option_i32 {
    Some(i32),
    None,
}

enum Option_f64 {
    Some(f64),
    None,
}

fn main() {
    let integer = Option_i32::Some(5);
    let float = Option_f64::Some(5.0);
}
```

- rust는 제네릭 코드를 특정 타입을 사용하는 코드로 컴파일 하로 제네릭을 사용한다 해서 별도의 런타임 비용이 들지 않는다.

