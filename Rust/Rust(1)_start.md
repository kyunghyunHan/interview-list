# 설치하기

## 리눅스 macOS install

```
$ curl https://sh.rustup.rs -sSf |sh
```

- 이 명령은 스크립트 파일을 하나 내려 받은 후 rustup도구와 함꼐 최신 안정 버전의 Rust를 설치한다.
- 성공적으로 설치가 완료되면 다음과 같은 결과가 나타난다

```
Rust is install now..Great!!
```

- 설치 스크립트는 사용자가 터미널에 다시 로그인 하는 시점에 Rust 컴파일러를 시스템의 PATH환경변수에 자동으로 등록한다
- 만일 Rust를 설치한 후 터미널을 종료하지 않고 곧바로 사용해보고 싶다면 다음 명령을 통해 시스템 PATH변수에 수동으로 등록한다.

```
$ source $HOME/.cargo/env
```

- 또는 ~/.bash_profile파일에 다음코드 추가

```
export PATH="$HOME/.cargo/bin:$PATH"
```

- rustup도구 컴파일러와 더불어 일종의 linker도 install해야한다 (rust 컴파일시 linker을 실행할수 없다는 에러를 발생시)

## window rustup install

- window사용자라면 https://www.rust-lang.org\tool\install문서에 설명에 따라 설치

## Update 및 delete

- new version update

```
$ rustup update
```

- 제거

```
$ rustup self uninstall
```

## 문제해결

- rust가 제대로 설치 되었는지 확인

```
rustc --version
```

```
rustc 1.62.0 (a8314ef7d 2022-06-27)
```

- 이정보가 출력되지 않는다면 환경변수를 확인!!!
- 여전히 rust가 실행되지 않는다면 rust discode 채널을 이용

```
https://discode.gg/rust-lang
```

- 이 채널을 통해 다른 러스트시안(rustaceans)들과 채팅을 할 수 있다.
- 또다른 방법 밑의 주소 방문

```
https://users.rust-lang.org/     ->개발자 포럼
http://stackoverflow.com/questions/tagged/rust/ ->스택 오버 플로우
```

## 첫번쨰 progream작성

- Helloworld를 작성해보기

## project 디렉토리 만들기

- 터미널을 열고 projects디렉토리 생성 후 hello_word를 위한 디렉토리 생성

```
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

- window 환경

```
mkdir "%USERPROFILE%/projects"
cd "%USERPROFILE%/projects"
mkdir hello_world
cd hello_world
```

## RUST progream start

- main.rs 생성

```rs
fn main(){
    println!("hello, world");
}
```

- 코드 입력후 터미널 창에서

```
$ rustc main.rs
$ ./main
hello, world
```

- window

```
 rustc main.rs
 .\main.exe
hello, world

```

## Rust프로그램 살펴보기

```rs
fn main(){

}
```

- 이 코드는 rust에서 함수를 정의하는 코드
- 이 함수는 실행가능한 rust프로그램에서 가장 첫번쨰로 실행되는 함수
- 첫번쨰 줄은 매개변수도 없고 return값도 없는 main이라는 이름의 함수를 선언한다.
- 함수의 본문은 중괄호{}로 둘러 쌓여 있다.rust에서는 모든 함수의 본문 코드를 중괄호로 쌓여야 한다.

```rs
println!("hello, world")
```

- rust에서는 들여쓰기 탭이 아니라 공백문자 4개를 이용한다.
- println!은 러스트매크로라 부르는 것이다.
- 매크로 대신 함수를 사용했다면 println만 사용했을 것이다.
- 각 구문은 ;(세미클론) 으로 끝난다. 이 기호는 표현식이 완료 되었으며 다음 표현이 시작한다는 것을 의미한다.

## 컴파일과 실행의 분리

- rust프로그램을 사용하려면 rust컴파일러로 프로그램을 먼저 컴파일 해야한다.

```
$ rustc main.rs
```

- rust는 컴피일이 성공적이면 실행가능한 바이너리 파일을 생성한다.

```
$ ls
main main.rs
```

- window

```
dir/B
main.exe
main.pdb
main.rs
```

- 이 명령을 실행하면 .rs확장자가 부여된 소스코드 파일,실행파일,윈도우용 디버깅 정보를 가진 .pdb확장자를 가진 파일까지 확인가능하다.
- 실행

```
 $ ./main.rs    or    .\main.exe
```

- 성공적이면 hello wolrd출력
