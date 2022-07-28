# git

Git status 상태확인
Git add 파일명. 중간저장소에 파일저장
Git commit -m ‘내용’
Git pull
Git push
git rm -r ch1
git rm ch1

[] () 태그

C cd desktop/github/deepdive/cs1

-GitHub
소셜 네트워크 이면서 클라우드 같은 서비스
-git
내컴퓨터의 모든 파일의 변화를 보고있으며 여러개의 작업 평행우주를 만들수있다
같은파일을 가지고 여러명과 함께 작업이 가능하다
​
-commit : 파일을 추가하거나 변경내용을 저장소에 저장하는작업
-push : 파일을 추가하거나 변경내용을 원격저장소에 업로드하는 작업
-branch: 병렬로 기록을 해가는 기능 (?)
-git hub 사용법
기본적으로 작은 작업단위로 커밋을 하고 어느정도 작업이 일단락 되면 푸시를 하는것이 일반적이다. 커밋 작업이 알기쉽게 커밋 메시지를 남겨두면 도움이 된다.

1. GitHub에 저장소 작성(git init) 또는 복제(git clone)
2. 파일의 작성,편집
3. 파일의 생성 / 변경 / 삭제 를 git 인덱스에 추가 (git add)
4. 변경결과를 로컬 저장소에 커밋(git commit)
5. 로컬 저장소를 푸쉬해 원격 저장소에 반영 (git push)
   ​
   -mkdir 새로운 디렉토리를 만드는 명령
   -cd 디렉토리를 이동하는 명령
   -git init 깃 저장소를 새로만드는 명령
   -git add (+파일명.확장자 or -A) git인덱스에 추가
   -git commit -m "커밋메시지" 인덱스에 추가된 파일을 커밋 변경을 저장소에 기록하는 작업임

- git status 파일이 추가되었는지 상태확인
- git push origin master 로컬저장소의 변경사항을 github에 있는 원격저장소에 반영
  -git branch //브랜치의 목록
  -git branch 이름// 브랜치의 생성(?)
  -git checkout 이름// 지점의 이동
  -git checkout -b 이름// 지점만들기 및 이동 다음 명령
  -git pull // 원격 브랜치의 코드를 가져오는것
  -git checkout master// 현재분기를 master로 전환
  -git merge 이름//브랜치 결과를 병합
  -git branch -d 이름 // 브랜치 삭제
  ​
  -git status // 저장소의 상태 확인
  -git add // 파일이나 디렉토리를 인덱스에 추가
  -git commit // 익덱스에 추가된 파일이나 폴더의 내용을 저장소에 쓸때 사용
  -git log // 로컬저장소의 커밋 히스토리를 탐색
  -git grep "검색 단어"//저장소 파일내용에서 검색하고자 할때 사용
  -git clone[url] // 기존 원격저장소를 로컬에 다운로드
  -git remote // 원격저장소의 이름목록을 표시
  -git remote -v // 원격저장소에 대한 자세한 목록보기
  -git remote add[name][url] //원격저장소를 추가
  -git remote rm[name]// 원격저장소를 제거
  -git reset //로컬 저장소의 커밋을 취소하기 위하여 사용
  -git merge // 현재 브랜치에서 다른지점에서 변경사항을 병합하는 사용
  ex) bug-fix를 master 브랜치에 병합
  -git checkout master git merge bug-fix
  -git pull // 원격브랜치의 변경사항을 캡처하기 위해 사용
  ex) 로컬저장소의 master 브랜치에 원격저장소 origin의 master 브랜치를 가져옴
  git checkout master git pull origin master
