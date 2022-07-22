# match 흐름 제어 연산자

- 일련의 패턴에 대해 어떤 값을 비교한 뒤 어떤 패턴에 매치되었는지를 바탕으로 코드를 수행하도록해줍니다

```rs
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u32 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

- 예제에서 coin의 타입은 Coin 열거형입
- . 여기서의 첫 번째 갈래는 값 Coin::Penny로 되어있는 패턴을 가지고 있고 그 후에 패턴과 실행되는 코드를 구분해주는 => 연산자가 있습니다. 위의 경우에서 코드는 그냥 값 1

```rs

fn value_in_cents(coin: Coin) -> u32 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}

```

## 값들을 바인딩하는 패턴들

- 매치 갈래의 또 다른 유용한 기능은 패턴과 매치된 값들의 부분을 바인딩할 수 있다는 것입니다. 이것이 열거형 variant로부터 어떤 값들을 추출할 수 있는 방법입니다.

```rs
#[derive(Debug)] // So we can inspect the state in a minute
enum UsState {
    Alabama,
    Alaska,
    // ... etc
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}
```

## Option<T>를 이용하는 매칭

- Option<T>는 coin열거자의 사용 예와 마찬가지로 match표현식에서도 사용할수 있다.

```rs
fn plus_one(x:Option<i32>)->Option<i32>{
    match x{
        None=>None,
        some(i)=>Some(i+1),
    }

    let five = Some(5);
    let six= plus_one(five);
    let none= plus_one(None);
}

```

- plus_one함수를 호출하면 함수본문안의 변수 x 는 Some(5)라는 값을 갖는다
- Some(5)값은 Nonew패턴과 일치하지 않으므로 다음 패턴을 비교한다 Some(5)값은 Some(i)와 일치

## match는 반드시 모든 경우를 처리해야한다

```rs
fn plus_one(x:Option<i32>)->Option<i32>{
    match x {
        Some(i)=> Some(i+1),
    }
}
```

- None값에 해당하는경우를 처리하지 않앗으므로 에러발생

## 변경자(placeholder)

```rs
let some_u8_value= 0u8;
match some_u8_value{
    1=>println!("one"),
     3=>println!("one"),
      5=>println!("one"),
       7=>println!("one"),
       _=>(),
}
```

- \_패턴은 모든 값에 일치함을 의미
- match표현식은 단 한가지 경우만 처리할떄 사용하기에는 다소 장황
