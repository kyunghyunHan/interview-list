# 반복문을 이용해 다중 입력 지원하기

```rs
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main(){
println!("숫자를 맞혀 봅시다");
let secrect_number=rand::thread_rng().gen_range(1,101);
println!("사용자가 맞혀야 할 숫자:{}",secrect_number);

loop{


println!("정답이라고 생각하는 숫자를 입력하세요");
let mut guess = String::new();
io::stdin().read_line(&mut guess).expect("입력한 값을 읽지 못했습니다.");
let guess:u32 = guess.trim().parse().expect("입력한 값이 올바른 숫자가 아닙니다.")
println!("입력한 값:{}",guess);
match guess.cmp(&secreat_number){
Ordering::Less =>println!("입력한 숫자가 작습니다."),
c =>println!("입력한 숫자가 큽니다."),
Ordering::Equal =>println!("정답"),

     }
}
}

```

- 숫자를 입력해 달라는 요청 매세지 부터 나머지 모든 코드를 반복문 안으로 옮겻다. 반복문 안의 모든 코드마다 네게의 공백을 이용해 들여 쓰기를 해줘야 한다.
- 이코드는 끝없이 다른 숫자를 입력하라고 요구 할것이다.(무한 루프)

```
cargo run
quit
```

- quit를 입력하면 게임을 종료할수 있지만 숫자가 아닌 어떤 값을 입력해도 종료댄다

## 숫자를 맞히면 종료하기

```rs
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main(){
println!("숫자를 맞혀 봅시다");
let secrect_number=rand::thread_rng().gen_range(1,101);
println!("사용자가 맞혀야 할 숫자:{}",secrect_number);

loop{


println!("정답이라고 생각하는 숫자를 입력하세요");
let mut guess = String::new();
io::stdin().read_line(&mut guess).expect("입력한 값을 읽지 못했습니다.");
let guess:u32 = guess.trim().parse().expect("입력한 값이 올바른 숫자가 아닙니다.")
println!("입력한 값:{}",guess);
match guess.cmp(&secreat_number){
Ordering::Less =>println!("입력한 숫자가 작습니다."),
c =>println!("입력한 숫자가 큽니다."),
Ordering::Equal =>{println!("정답");
break;
}

     }
}
}

```

- 정답을 출력하는 줄 다음에 break구문을 추가하면 사용자가 숫자를 맞혓을떄 반복문에서 탈출할수 있다.

## 올바르지 않은 입력 처리하기

```rs
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main(){
println!("숫자를 맞혀 봅시다");
let secrect_number=rand::thread_rng().gen_range(1,101);
println!("사용자가 맞혀야 할 숫자:{}",secrect_number);

loop{


println!("정답이라고 생각하는 숫자를 입력하세요");
let mut guess = String::new();
io::stdin().read_line(&mut guess).expect("입력한 값을 읽지 못했습니다.");
let guess:u32 = guess.trim().parse(){
    Ok(num)=>num,
    Err(_)=>continue,
}
println!("입력한 값:{}",guess);
match guess.cmp(&secreat_number){
Ordering::Less =>println!("입력한 숫자가 작습니다."),
c =>println!("입력한 숫자가 큽니다."),
Ordering::Equal =>{println!("정답");
break;
}

     }
}
}
```

- 보통 에러가 발생했을떄 프로그램을 종료하는방식에서 에러를 처리하는 방식으로 바뀌었을떄 expect메서드 호출을 match표현식으로 변경한다.
- parse메서드가 Result타입을 리턴하여 Result타입은 Ok 또는 Err값을 표현하는 열거자 였음을 기억하면 이해가 될 것이다.
- parse메서드는 문자열을 숫자로 문제없이 변환하면 변환된 숫자를 가진 Ok값을 리턴한다.
- Ok값은 첫번쨰 가지의 패턴과 일치하며 match표현식은 parse메서드가 생성해 Ok값에 넣어둔 num값을 리턴한다. 그리고 이숫자는 guess변수의 선언에 대입된다.
- parse메서드는 문자열을 숫자로 변환하지 못하면 관련되 에러에 대한 정보를 담은 Err값을 리턴한다.Err값은 첫번쨰 가지의 Ok(num)패턴과 일치하지는 않지만 ,두번쨰 가지의
  Err(\_)패턴에 일치한다.여기서 사용한 \_은 모든 값을 표현하는 문자다.

  ```
  $ cargo run


  ```

- 이 프로그램은 여전히 정답을 함께 출력하고 있다
- 최종버전

```rs
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main(){
println!("숫자를 맞혀 봅시다");
let secrect_number=rand::thread_rng().gen_range(1,101);
println!("사용자가 맞혀야 할 숫자:{}",secrect_number);

loop{


println!("정답이라고 생각하는 숫자를 입력하세요");
let mut guess = String::new();
io::stdin().read_line(&mut guess).expect("입력한 값을 읽지 못했습니다.");
let guess:u32 = guess.trim().parse(){
    Ok(num)=>num,
    Err(_)=>continue,
}
println!("입력한 값:{}",guess);
match guess.cmp(&secreat_number){
Ordering::Less =>println!("입력한 숫자가 작습니다."),
c =>println!("입력한 숫자가 큽니다."),
Ordering::Equal =>{println!("정답");
break;
}

     }
}
}
```
