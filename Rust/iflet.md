# if let을 이용한 간결한 흐름제어

```rs
let some_u8_value= Some(0u8);
match some_u8_value{
    Some(3)=>println!("three"),
    _=>(),
}
```

- 이 코드는 \_->()패턴때문에 전체코득 장황해보인다

```rs
if let Some(3)=some_u8_value{
    println!("three");
}
```

- iflet문법을 사용하면 전체적으로 입력해야할 코드의 양도 적고 들여쓰기도 줄며 불필요한 코드도 줄어든다
- iflet구문은 else구문을 포함할수 있다.else블록의 코드는 match표현식에서 \_패턴에 연관된코드블록에 해당한다.

```rs
let mut count=0;
match coin{
    Coin::Quater(state)=>println!("{:?}주의 25센트 동전!",state),
    _=>count +=1,
}
```

- 또는

```rs
let mut count = 0;
if let Coin::Quarter(state)= coin{
    println!("{:?}주의 25센트 동전!",state);
}else{
    count+=1;
}
```

- 요약
