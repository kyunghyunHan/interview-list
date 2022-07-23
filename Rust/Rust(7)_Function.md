# 함수

- 함수는 Rust에서 마니 사용되는 개념이다
- Rust는 함수와 변수의 이름에 스네이크 케이스를 사용한다.스네이크 케이스란 소문자와 함께 단어를 밑줄 기호로 구분하는 형식을 말한다.

```rs
fn main() {
    println!("hello");
    another_fuction();
}

fn another_fuction() {
    println!("another")
}

```

- Rust에서는 함수 선언은 fn키워드와 함수의 이름 그리고 괄호로 구성된다.중괄호는 컴파일러에게 함수의 본문과 끝을 알려준다.
