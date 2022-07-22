# Address already in use

- 서버를 실행하는데

```
build httpd server faild. Address already in use
```

- 이런 에러가 종종 발생한다.
- 포트가 이미 다른 프로세스에서 사용중이기 떄문
- 이를 종료하려면

```
$ lsof -i:<포트번호>
$ lsof -i:8080
```

- 를 입력하여 포트 번호를 확인한다.

- 포트번호를 확인 한 후 위에 보이는 PID를 확인하여 다음과 같이 입력한다.

```
kill -9 <PID>
```

그러면 성공적으로 포트를 종료시킨걸 확인할수 있다.
