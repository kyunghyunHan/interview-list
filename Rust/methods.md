# Methods

- 함수와 유사
- fn키워드와 이름을 가지고 선언되고 , 파라미터와 반환값을 가지고 있으며 어딘가로부터 호출되었을 떄 실행될 어떤 코드를 담고있습니다
- 메소드는 함수와는 달리 구조체의 내용 안에 정의

## 메소드 정의하기

```rs
#[derive(Debug)]
struct Rectangle {
    length: u32,
    width: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.length * self.width
    }
}

fn main() {
    let rect1 = Rectangle { length: 50, width: 30 };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

## Rectangle 구조체 상에 area 메소드 정의하기

- Rectangle의 내용 안에 함수를 정의하기 위해서, impl (구현: implementation) 블록을 시작합니다.
- 그런다음 area함수를 impl중괄호 안으로 이동하고 첫번쨰 매개변수의 이름과 함수 본문 안에서의 참조를 모두 self로 바꾼다.
- main함수 안에서는 area함수를 호출하면서 rect1변수를 인수로 전달하고 있다.
- 메서드 문법은 인스턴스 다음에 따라온다 즉 인스턴스 다음 마침표,메서드의 이름,괄호,그리소 필요한 인수를 전달하면 댄다
- area메서드의 시그니처를 보면 rectangle:&Rectangle대신 단순히 &selp를 사용하고 있다.
- 이함수는 이제 Rectangle구조체의 컨텍스트 안에 존재하므로 러스트는 self의 타입이 Rectangle라는것을 알고 있다.
- 이 함수는 단순히 구조체의 데이터를 읽을 뿐 값을 쓰지는 않기 떄문에 굳이 소유권을 가질 필요가 없다
- 만일 메서드를 변경하고자 할떄는 첫번쨰 매개변수를 &mut self와 같이 선언해야한다

## 더많은 매개변수를 갖는 메서드

```rs
fn main() {
    let rect1 = Rectangle {
        length: 50,
        width: 30,
    };
    let rect2 = Rectangle {
        length: 40,
        width: 10,
    };
    let rect3 = Rectangle {
        length: 45,
        width: 60,
    };

    println!("Can rect1 hold rect2? {}", rect1.can_hold(&rect2));
    println!("Can rect1 hold rect3? {}", rect1.can_hold(&rect3));
}
```

## 연관함수

- impl블록의 또다른 유용한 기능은 self매개변수를 사용하지 않는 다른 함수도 정의 할수 있다는점이다.(연관함수)
- 이 함수들은 인스턴스를 직접 전달 받지 않기 떄문에 메서드가 아니라 함수다지금까지 사용한 String::from 도 연관함수
- 연관함수는 구조체의 새로운 인스턴스를 리턴하는 생성자를 구현할떄 사용
- 하나의 매겨변수를 전달받아 이 값을 너비와 높이로 지정하는 연관함수를 정의할수도 있다.

```rs
impl Rectangle{
    fn square(size:u32)->Rectangle{
        Rectangle{width:size,height:size}
    }
}
```

- 연관함수를 호춯하려면 구조체의 이름과 ::문법사용
- let sq = Rectangle::square

## 여러개의 impl블록

```rs
impl Rectangle {
    fn area(&self)->u32{
        self.width*self.height
    }
}
impl Rectangle{
    fn can_hold(&self,other:&Rectanglge)->bool{
        self.width>other.width && self.height>other.height
    }
}
```

- 이처럼 굳이 여러개의 impl블록을 선언할 필요는 업지만 제네릭타입과 트레이트에대해서 사용

## 요약

- 구조체는 프로그램안에소 특정한 의미가 있는 사용자 정의 타입을 선언하기 위한 개념
- 구조체를 이용하면 관련된 데이터를 모아 개별적으로 이름을 부여할수 있어 코드를 더욱더 깔끔하게 작성할수 있다.
- 메서드는 구조체의 인스턴스에 원하는 동작을 부여하며, 연관함수는 구조체의 인스턴스가 없는 상황에서 구조체에 적용할수 있는 기능을 구분짓는데 활용
