# Cargo

- cargo는 러스트의 빌드 시스템 이자 패키지 관리자다.
- cargo는 코드의 build,코드가 의존하는 라이브러리 다운로드,이런 라이브러리의 빌드 등 다양한 작업을 대신 해주기 떄문이다
- 러스트를 설치하였다면 cargo역시 설치 되어 있다.

```
$ cargo --version
```

- version번호가 출력되어 있다면 cargo설치가 잘 된 것이다.
- command not found같은 erorr를 보게 된다면 rust를 재설치 해야 한다.

## cargo를 이용해 project생성

- projects디렉토리로 돌아간후 운영체제와 상관없이 명령 실행

```
$ cargo new hello_cargo

$ cd hello_cargo
```

- 첫번쨰 명령은 hello_cargo라는 새로운 디렉토리 생성한다.
- hello_cargo디렉토리를 들어가보면 main.rs가 보관된 src 디렉토리와 Cargo.toml파일을 생성해 준것을 확인 할 수 있다.
- cargo new명령이 생성한 Cargo.toml코드

```
[Package]
name= 'hello_cargo'
version= "0.1.0"
authors = ["작성자이름<메일주소>"]
edition= "2020"

[dependencies]
```

- package는 패키지의 설정을 관리하기 위한 섹션
- 섹션 제목 다음 네줄은 이름,버전,작성자,에디션 설정 정보를 지정하는 코드다.
- cargo는 현재 시스템 환경에서 작성자의 이름과 메일 주소를 읽기 떄문에 정보가 오르지 않다면 해당 정보를 수정하도록 하자
- dependencies는 프로젝트의 의존 라이브러리 목록을 관리하는 섹션이 시작하는 부분이다.

## cargo build 및 실행

```
$ cargo build
```

- 이 명령을 실행하면 현재 디렉토리가 아닌 target/debug/hello_cargo경로에 실행 파일이 생성된다.
- 이 실행 파일은 다음 명령으로 실행

```
$ ./target/debug/hello_cargo
Hello,world!
```

- 지금까지 cargo build명령을 통해 프로젝트를 빌드 한 후 target/debug/hello_cargo파일을 실행햇지만 한줄의 명령으로 컴파일 후 결과파일을 실행할수 있다.

```
$ cargo run
Hello,world!
```

- 그런데 이번에 hello_cargo프로젝트의 컴파일을 실행햇다는 결과가 출력되지 않았다.
- 그이유는 소스코드 파일이 변경되지 않았음을 탐지하고 곧바로 바이너리를 실행 했기 떄문이다.
- cargo 는 cargo check라는 명령 또한 지원한다.(코드의 컴파일 여부를 신속하게 검사)

```
$ cargo check
```

- cargo check는 실행 명령을 생성하는 과정을 생략하므로 cargo build명령보다 훨씬 빠르게 실행된다.

- 따라서 코드를 작성하는동안 작업 결과에 오류가 없는지를 계속 확인하려면 cargo check명령을 이용하는 편이 빠르다.
- 정리

```
cargo build와 cargo check명령
cargo run
cargo는 빌드 결과물과 소스와 다른 디렉토리인  target/debug/ 따로 저장
```

## 릴리즈를 위한 빌드

- 프로젝트를 릴리즈할 준비가 되면 cargobuild --release를 이용하여 최적화된 컴파일을 실행 할수 있다.

- 이 명령어는 target/debug/ 가 아닌 target/release경로에 실행 파일을 생성한다.
- 이떄 실행되는 최적화 덕분에 코드는 더 빠르게 실행 되지만 프로그램을 컴파일 하는 시간은 더 길어 진다.

- 코드 내려받기

```
$ git clone 주소
$ cd 프로젝트
$ cargo build
```
