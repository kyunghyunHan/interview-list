# git pull error(git pull 충돌 에러)

- git pull 충돌 에러
- (Your local changes to the following files would be overwritten by merge)

## 상황

- git pull 시 Your local changes to the following files would be overwritten by merge 라는 메세지와 함꼐 에러 발생

## 해결

- Please commit your changes or stash them before you merge.

- marge하기전에 commit또는 stash를 하라는 뜻이다.
- git stash : 마무리 되지 않은 작업을 스택에 임시저장

```
$ git stash   //스택에 임시저장
$ git pull origin <branch name> //git 최신 소스 다운
$ git stahs pop //변경사항 적용 및 스택 제거
```
