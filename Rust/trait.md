## 트레이드 :공유 가능한 행위를 정의하는 방법

- trait는 러스트 컴파일러에게 특정타입이 어떤 기능을 실행할수 있으며 어떤 타입과 이 기능을 공유할수 있는지를 알려주는 방법이다.

- 트레이트는 공유 가능한 행위를 추상화 된 방식으로 정의하는 방법이다.

## 트레이드 선언

- 타입의 행위는 해당 타입에 대해 호출할 수 있는 메서드 구성된다.
- 이뗴 다른 타입에 같은 메서드를 호출할 있다면 그 행위는 타입간에 공유가 가능하다.
- 트레이 정의는 어떤 목적을 이루는데 필요한 일련의 행위를 정의하고 여러 타입에 적응할 메서드 시그니처를 그룹화 하는 방법
- 예를들어 각기 다른 종류와 크기의 텍스트를 저장하는 여러 개의 구조체가 있다
- NewsArticle구조체는 stroy필드에 특정 지역의 뉴스콘텐츠를 저장하며 tweet구조체는 최대 280글자의 텍스트와 해당 트윗이 새 트윗인지 다른 트윗의 댓글인지 가리키는 메타데이터를 포함한다
- 이제 NewArticle이나 tweet구조체의 인스턴스에 저장된 데이터를 요약해서 보여주는미디어 수집 라이브러리를 개발한다고 가정
- 각 인스턴스로부터 summarize메서드를 호출해야한다

```rs
pub trait summary{
    fn summarize(&self) ->String;
}
```

- 트레이드를 정의할 떄는 trait키워드 다음에 트레이드의 이름을 지정
- 메서드 시그니처 다음에는 실제 구현코드대신 세미클론을 덧붙인다
- 이 트레이트를 구현하는 각 타입은 반드시 이 메서드 본문에 자신의 행위를 구현해야 한다

## 타입에 트레이트 구현

```rs
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summarizable for NewsArticle {
    fn summary(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summarizable for Tweet {
    fn summary(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

- 타입에 트레이트를 구현하는 것은 보통의 메서드를 구현하는 방법과 유사하다
- 한가지 차이점은 impl키워드 다음에 구현할 트레이트의 이름과 for키워드 그리고 트레이트를 구현할 타입의 이름을 덧붙인다는점이다.
- impl블록에서는 트레이트에 정의된 메서드 시그니처를 추가한다.
- 그리고 각 시그니처를 세미클론으로 마무리 하는 대신 중괄호를 이용해 트레이트의 메소드가 이타입에 대해 수행해야할 특정 동작을 매서드 본문으로 구분하게 하면댄다
- 트레이트를 구현한 뒤에는 보통의 메소드와 마찬가지로 NewsArticle과 Tweet타입의 인스턴스에 대해 해당 메서드를 호출할수 있다.

```rs
let tweet = Tweet {
    username: String::from("horse_ebooks"),
    content: String::from("of course, as you probably already know, people"),
    reply: false,
    retweet: false,
};

println!("1 new tweet: {}", tweet.summary());
```

- 트레이트 구현에 있어 한가지 제약은 트레이트나 트레이트를 구현할 타입이 현재 크레이트 로컬 타입이어야 한다는 점이다.
- Display같은 표준 라이브러리 트레이트를 aggregator크레이트의 일부인 tweet타입에 구현할수 있는 이유는 tweet타입이 aggregator크레이트의 로컬타입이기 때문이다.

```rs
extern crate aggregator;

use aggregator::Summarizable;

struct WeatherForecast {
    high_temp: f64,
    low_temp: f64,
    chance_of_precipitation: f64,
}

impl Summarizable for WeatherForecast {
    fn summary(&self) -> String {
        format!("The high will be {}, and the low will be {}. The chance of
        precipitation is {}%.", self.high_temp, self.low_temp,
        self.chance_of_precipitation)
    }
}
```

## 기본구현

```rs

pub trait Summarizable {
    fn summary(&self) -> String {
        String::from("(Read more...)")
    }
}
```

- NewAruicle구조체의 인스턴스에 새로운 구현을 제공하는 대신 summarize메서드의 기본 구현을 사용하려면 impl Summary for NewArticle블록을 비워두면 된다

```rs
impl Summarizable for NewsArticle {}
```

```rs
let article = NewsArticle {
    headline: String::from("Penguins win the Stanley Cup Championship!"),
    location: String::from("Pittsburgh, PA, USA"),
    author: String::from("Iceburgh"),
    content: String::from("The Pittsburgh Penguins once again are the best
    hockey team in the NHL."),
};

println!("New article available! {}", article.summary());
```

- summarize메서드의 기본 구현을 제공한다고 해도 Tweet구조체에 구현한 summary구현은 전혀 영향을 받지 않는다
- 기본 구현은 같은 트레이트의 다른 메서드를 호춯할수 있다.
-

```rs
pub trait Summarizable {
    fn author_summary(&self) -> String;

    fn summary(&self) -> String {
        format!("(Read more from {}...)", self.author_summary())
    }
}
```

- 이버전의 Summary트레이틑 사용하려면 원하는 타입에 author_summary메서드만 저으이해주면 된다.

```rs
impl Summarizable for Tweet {
    fn author_summary(&self) -> String {
        format!("@{}", self.username)
    }
}
```

- author_summary메서드를 구현하고 tweet구조체의 summarize메서드를 호출하면 summarize메서드의 기분구현코드가 author_summary메서드를 호출한다.

```rs
let tweet = Tweet {
    username: String::from("horse_ebooks"),
    content: String::from("of course, as you probably already know, people"),
    reply: false,
    retweet: false,
};

println!("1 new tweet: {}", tweet.summary());
```

## 트레이트 매개변수

- Summaty트레이트를 구현하는 타입을 전달할수 있는 item매개변수의 summarize메서드를 호춯하는notify함수를 작성할수 있다.

```rs
pub fn notify<item: impl Summary> {
    println!("Breaking news! {}", item.summary());
}
```

- item매개변수를 실제타입을 정의하는 대신 impl키워드와 트레이트의 이름을 이용해 정의햇다
- 이 매개변수에는 지정된 트레이트를 구현하는 모든 타입을 인수로 전달할수 있다.
- 트레이트 경계문법

```rs
pub fn notify<T: Summarizable>(item: T) {
    println!("Breaking news! {}", item.summary());
}
```

- impl trait문법은 비교적 간단한 경우에는 잘 동작하지만 사실은 트레이트 경계라 부르는 더 긴 문법을 간단히 표현하기 위한 장치이다.
- 트레이트 경계는 제네릭 타입 매개변수를 선언하는 꺽쇠 괄호 안에 사용된다.

```rs
fn some_function<T: Display + Clone, U: Clone + Debug>(t: T, u: U) -> i32 {
```

```rs
fn some_function<T, U>(t: T, u: U) -> i32
    where T: Display + Clone,
          U: Clone + Debug

```

```rs
fn largest<T: PartialOrd>(list: &[T]) -> T
```

```rs
use std::cmp::PartialOrd;

fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
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
