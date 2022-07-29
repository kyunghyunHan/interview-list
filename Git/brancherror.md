# 깃 branch 이동 및 조회 가 안될경우

# 모든 원격브랜치 조회

```
$ git remote -r

```

# 리모트 브랜치 조회

```
$ git branch -r
```

## 모든 브렌치 조회

```
$ branch -a
```

- 조회시 아무것도 조회가 안된다.

# develop 브랜치로 이동 시도

- 브랜치가 업데이트 되지 않았기 떄문에 브랜치가 안보이는 것이였다.

```
$ git remote update
$ git fetch

$ git checkout 브랜치명
```

- 브런치가 업데이트 되지 않는다면 개별로 업데이트를 해주면된다.

# 로컬에 원격 브랜치 복사

$ git checkout -t origin/브랜치명
