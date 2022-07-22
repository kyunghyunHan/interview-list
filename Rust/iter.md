# 반복자로 일련의 항목들 처리하기

- 반복자 패턴은 일련의 항목들에 대해 순서대로 어떤 작업을 수행할 수 있도록 해줍 니다. 반복자는 각 항목들을 순회하고 언제 시퀀스가 종료될지 결정하는 로직을 담당 합니다. 반복자를 사용하면, 저런 로직을 다시 구현할 필요가 없습니다.
- 러스트에서, 반복자는 게으른데, 항목들을 사용하기위해 반복자를 소비하는 메서드를 호출하기 전까지 반복자는 아무런 동작을 하지 않습니다

```rs
let v1 = vec![1, 2, 3];

let v1_iter = v1.iter();

```

- 반복자는 v1_iter 변수에 저장되고, 그 시점에 순회는 발생하지 않습니다. v1_iter 에 있는 반복자를 사용하는 for 루프가 호출되면, 루프 순회 마다 반복자의 각 요소가 사용되는데, 각각의 값을 출력 합니다.

```rs
let v1 = vec![1, 2, 3];

let v1_iter = v1.iter();

for val in v1_iter {
    println!("Got: {}", val);
}
```

- 표준 라이브러리에서 반복자를 제공하지 않는 언어에서는, 변수를 인덱스 0으로 시작해서, 그 변수로 벡터를 색인해서 값을 가져오는데 사용하며, 루프안에서 벡터에 있는 아이템의 총 갯수까지 그 변수를 증가시키는 방식으로 동일한 기능을 작성할 수 있습니다.
- 반복자는 그러한 모든 로직을 대신 처리 하며, 잠재적으로 엉망이 될 수 있는 반복적인 코드를 줄여 줍니다. 반복자는 벡터처럼 색인할 수 있는 자료구조 뿐만 아니라, 많은 다른 종류의 시퀀스에 대해 동일한 로직을 사용할 수 있도록 더 많은 유연성을 제공 합니다.

## Iterator트레잇과 next 메서드

```rs
trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;

    // methods with default implementations elided
}
```

- 이 정의는 몇 개의 새로운 문법을 사용하는 것에 유의하세요: type Item 과 Self::Item 은 이 트레잇과 연관 타입 을 정의 합니다
- Iterator 트레잇을 구현하는 것은 Item 타입을 정의하는 것 또한 요구하며, 이 Item 타입이 next 메서드의 리턴 타입으로 사용된다는 것을 나타낸다는 것 입니 다. 다른 말로, Item 타입은 반복자로 부터 반환되는 타입이 될 것 입니다.
- Iterator 트레잇은 단지 구현자가 하나의 메서드를 정의하도록 요구 합니다: next 메서드 입니다. 이 메서드는 반복자의 하나의 항목을 Some 에 넣어서 반환 하고, 반복자가 종료되면 None 을 반환 합니다.
- 반복자의 next 메서드를 직접 호출할 수 있습니다; 리스트 13-15 는 벡터로 부터 생성된 반복자에 대해 반복된 next 호출이 어떤 값들을 반환하는지 보여줍니다:

```rs
#[test]
fn iterator_demonstration() {
    let v1 = vec![1, 2, 3];

    let mut v1_iter = v1.iter();

    assert_eq!(v1_iter.next(), Some(&1));
    assert_eq!(v1_iter.next(), Some(&2));
    assert_eq!(v1_iter.next(), Some(&3));
    assert_eq!(v1_iter.next(), None);
}
```

- 반복자에 대해 next 메서드를 호출하면 시퀀스의 어디에 있는지 추적하기 위해 반복자가 사용하는 내부 상태를 변경합니다. 다른 말로, 이 코드는 반복자를 소비 합니다, 혹은 다 써 버립니다. next 에 대한 각 호출은 반복자로 부터 하나의 항목을 소비 합니다. for 루프를 사용할 때는 v1_iter 를 변경할 수 있도록 만들 필요가 없는데, 루프가 v1_iter 의 소유권을 갖고 내부적으로 변경 가능하도록 만들기 때문 입니다.
- iter 메서드는 불변 참조에 대한 반복자를 만듭니다. 만약 v1 의 소유권을 갖고 소유된 값들을 반환하도록 하고 싶다면, iter 대신 into_iter 를 호출해야 합니다. 비슷하게, 가변 참조에 대한 반복자를 원한다면, iter 대신 iter_mut 을 호출할 수 있습니다.

## 반복자를 소비하는 메서드들

- Iterator 트레잇에는 표준 라이브러리에서 기본 구현을 제공하는 다수의 다른 메서드들이 있습니다; Iterator 트레잇에 대한 표준 라이브러리 API 문서를 살펴 보면, 이 메서드들을 찾을 수 있습니다. 이 메서드들 중 일부는 그들의 구현에서 next 메서드를 호출하는데, 이것이 Iterator 트레잇을 구현할 때 next 메서드를 구현해야만 하는 이유 입니다.
- next 를 호출하는 메서드들을 소비하는 어댑터들 이라고 하는데, 그들을 호출하면 반복자를 써버리기 때문 입니다. sum 메서드가 하나의 예인데, 반복자의 소유권을 가져오고 반복적으로 next 를 호출해서 순회함으로써 반복자를 소비 합니다. 순회해 나가면서 누적합계에 각 아이템을 더하고 순회가 완료되면 합계를 반환 합니다.

```rs
#[test]
fn iterator_sum() {
    let v1 = vec![1, 2, 3];

    let v1_iter = v1.iter();

    let total: i32 = v1_iter.sum();

    assert_eq!(total, 6);
}
```

- sum 은 호출한 반복자의 소유권을 갖기 때문에, sum 을 호출한 후 v1_iter 은 사용할 수 없습니다.

## 다른 반복자를 생성하는 메서드들

- Iterator 트레잇에 정의된 다른 메서드들 중에 반복자 어댑터들 로 알려진 메서드 들은 반복자를 다른 종류의 반복자로 변경하도록 허용 합니다. 복잡한 행위를 수행하 기 위해 읽기 쉬운 방법으로 반복자 어댑터에 대한 여러개의 호출을 연결할 수 있습 니다. 하지만 모든 반복자는 게으르기 때문에, 반복자 어댑터들로 부터 결과를 얻기 위해 소비하는 메서드들 중 하나를 호출 해야 합니다.
- 반복자 어댑터 메서드인 map 을 호출하는 예를 보여주는데, 새로운 반복자를 생성하기 위해 각 항목에 대해 호출할 클로저를 인자로 받습니다. 여기서 클로저는 벡터의 각 항목에서 1이 증가된 새로운 반복자를 만듭니다. 그러나, 이 코드는 경고를 발생 합니다:

```rs
let v1: Vec<i32> = vec![1, 2, 3];

v1.iter().map(|x| x + 1);
```

```
warning: unused `std::iter::Map` which must be used: iterator adaptors are lazy
and do nothing unless consumed
 --> src/main.rs:4:5
  |
4 |     v1.iter().map(|x| x + 1);
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = note: #[warn(unused_must_use)] on by default
```

- 이것을 고치고 반복자를 소비하기 위해, collect메서드를 사용할 것인데 env::args 와 함께 사용했습니다. 이 메서드는 반복자를 소비하고 결과값을 수집 데이터 타입으로 모읍니다.
- 벡터에 대한 map 호출로 부터 반환된 반복자를 순회하면서 결과를 모읍니다. 이 벡터는 각 항목이 원본 벡터로 부터 1씩 증가된 상태로 될 것 입니다.

```rs
let v1: Vec<i32> = vec![1, 2, 3];

let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();

assert_eq!(v2, vec![2, 3, 4]);
```

- map 은 클로저를 인자로 받기 때문에, 각 항목에 대해 수행하기를 원하는 어떤 연산도 기술할 수 있습니다.

## 환경을 캡쳐하는 클로저 사용하기

- 반복자의 filter 메서드는 반복자로 부터 각 항목을 받아 Boolean 을 반환하는 클로저를 인자로 받습니다. 만약 클로저가 true 를 반환하면, 그 값은 filter 에 의해 생성되는 반복자에 포함될 것 입니다. 클로저가 false 를 반환하면, 결과로 나오는 반복자에 포함되지 않을 것 입니다.

```rs
#[derive(PartialEq, Debug)]
struct Shoe {
    size: u32,
    style: String,
}

fn shoes_in_my_size(shoes: Vec<Shoe>, shoe_size: u32) -> Vec<Shoe> {
    shoes.into_iter()
        .filter(|s| s.size == shoe_size)
        .collect()
}

#[test]
fn filters_by_size() {
    let shoes = vec![
        Shoe { size: 10, style: String::from("sneaker") },
        Shoe { size: 13, style: String::from("sandal") },
        Shoe { size: 10, style: String::from("boot") },
    ];

    let in_my_size = shoes_in_my_size(shoes, 10);

    assert_eq!(
        in_my_size,
        vec![
            Shoe { size: 10, style: String::from("sneaker") },
            Shoe { size: 10, style: String::from("boot") },
        ]
    );
}
```

- shoes_in_my_size 함수는 파라미터로 신발들의 벡터에 대한 소유권과 신발 크기를 받습니다. 그것은 지정된 크기의 신발들만을 포함하는 벡터를 반환 합니다.
- shoes_in_my_size 의 구현부에서, 벡터의 소유권을 갖는 반복자를 생성하기 위해 into_iter 를 호출 합니다. 그 다음 그 반복자를 클로저가 true 를 반환한 요소들만 포함하는 새로운 반복자로 바꾸기 위해 filter 를 호출 합니다
- 클로저는 환경에서 shoe_size 매개 변수를 캡처하고, 지정된 크기의 신발만 유지하면서 각 신발의 크기와 값을 비교합니다. 마지막으로,collect를 호출하면 적용된 반복자에 의해 리턴된 값을 함수가 리턴한 벡터로 모으게됩니다.

## Iterator 트레잇으로 자신만의 반복자 만들기

- 이것을 보여주기 위해 1부터 5까지 셀 수있는 반복자를 만듭니다. 우선, 어떤 값들을 유지하는 구조체를 만들 것 입니다. 그 다음 Iterator 트레잇을 구현하고 그 구현에서 값들을 사용함으로써 이 구조체를 반복자로 만들 것 입니다.

```rs
struct Counter {
    count: u32,
}

impl Counter {
    fn new() -> Counter {
        Counter { count: 0 }
    }
}
```

- Counter 구조체는 count 라는 이름의 하나의 필드를 갖습니다. 이 필드는 u32 타입의 값을 갖는데 1부터 5까지 순회하는데 어디까지 진행했는지를 추적할 것 입니다. count 필드는 Counter 구현이 그 값을 관리하길 원하기 때문에 외부로 노출되지 않습니다. new 함수는 항상 새로운 인스턴스가 count 필드에 0을 담은 채로 시작하도록 강제합니다.

```rs
impl Iterator for Counter {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
        self.count += 1;

        if self.count < 6 {
            Some(self.count)
        } else {
            None
        }
    }
}
```

- 우리는 우리의 반복자가 현재 상태에 1을 더하길 원합니다, 그래서 count 를 0 으로 초기화 했고 처음엔 1을 반환할 것 입니다. count 의 값이 6 보다 작다면, next 는 Some 으로 포장된 현재 값을 리턴할 것이며, count 가 6 이거나 더 크다면, 우리의 반복자는 None 을 반환할 것 입니다.
- Iterator 트레잇을 구현 했다면, 반복자를 갖게 됩니다! 리스트 13-22 는 리스트 13-15 에서 벡터로 부터 생셩된 반복자에 했던 것 처럼, Counter 구조체에 직접 next 메서드를 호출 함으로써 반복자 기능을 사용할 수 있다는 것을 보여주는 테스트를 보여 줍니다.

```rs
#[test]
fn calling_next_directly() {
    let mut counter = Counter::new();

    assert_eq!(counter.next(), Some(1));
    assert_eq!(counter.next(), Some(2));
    assert_eq!(counter.next(), Some(3));
    assert_eq!(counter.next(), Some(4));
    assert_eq!(counter.next(), Some(5));
    assert_eq!(counter.next(), None);
}
```

- 우리는 next 메서드를 정의함으로써 Iterator 트레잇을 구현했습니다, 그래서 표준 라이브러리에 정의된 Iterator 트레잇 메서드들의 기본 구현을 사용할 수 있 는데, 그들은 모두 next 메서드의 기능을 사용하기 때문 입니다.

```rs
#[test]
fn using_other_iterator_trait_methods() {
    let sum: u32 = Counter::new().zip(Counter::new().skip(1))
                                 .map(|(a, b)| a * b)
                                 .filter(|x| x % 3 == 0)
                                 .sum();
    assert_eq!(18, sum);
}
```

- zip 은 단지 네 개의 쌍을 생성한다는데 유의 하세요; 이론적으로 다섯번째 쌍인 (5, None) 은 결코 생성되지 않는데, zip 은 입력 반복자 중 하나라도 None 을 반환하면 None 을 반환하기 때문 입니다.

우리가 next 메서드가 어떻게 동작하는지에 대해 기술했기 때문에 이 모든 메서드 호출이 가능하며, 표준 라이브러리는 next 를 호출하는 다른 메서드들의 기본 구현 을 제공 합니다.
