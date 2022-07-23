## 함수 매개변수

- 함수는 함수 고유한 부분인 특별한 변수 매개변수를 갖는 형식으로 선언될 수 있습니다
- 매개변수 전달은 {}포함

```rs
fn main() {
    another(5);
}
fn another(x: i32) {
    println!("the value:{}", x)
}
```

```rs
fn main() {
    another(5, 6);
}

fn another(x: i32, y: i32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}

```

## 구문과 표현식

- 하나의 구문을 갖는 함수

```rs
fn main() {
    let y = 6;
}
```

- 표현식
- 세미클론으로 끝나지 않기떄문에 반환값으로 된다

```rs
fn main() {
    let x = 5;

    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {}", y);
}
```

## 반환값을 갖는 함수

```rs
fn five()->i32{
    5
}
fn main(){
    let x = five();
    println!("The value of x is: {}", x);
}
```

- 5가 반환값

```rs
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {}", x);
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```
