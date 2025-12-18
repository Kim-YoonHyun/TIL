# 1. 개요

버전관리를 위한 커밋 도구

## 1.1. 최초 커밋 시

```bash
1. $ git init (2.1)
2. $ git status (2.2)  (필수는 아님)
3. $ git add . (2.3.1)
4. $ git commit -m 'initial commit'  (2.4.1)
5. github 사이트에 원격저장소 생성
6. $ git remote add origin https://github.com/<username>/<저장소이름>.git  (2.8.1)
7. $ git push origin master (2.8.2)
8. github에서 push 확인
```

## 1.2. 최초 커밋 이후

```bash
1. $ git status (2.2)  (필수는 아님)
2. $ git add . (2.3.1)
3. $ git commit -m 'initial commit'  (2.4.1)
4. $ git push origin master (2.8.2)
5. github에서 push 확인
```

# 2. 명령어

## 2.1. git 시작

- 해당 명령어를 최초로 git 버전 관리가 가능해짐
- 숨김파일 형식으로 .git 폴더가 만들어짐
- <span style="color:red">절대로 상위폴더(또는 하위폴더)에 추가로 init 을 하면 안됨</span>
  이 경우 git이 꼬여서 에러가 발생하기에 모든 .git 폴더를 지워버리고 새로 git을 시작하는것 밖에 답이 없음.

```bash
$ git init
```

## 2.2. 상태 확인

- 파일의 변경 사항, stage 여부 등 상태(status) 확인 가능
- 보통 커밋 전 가장 먼저 확인용으로 입력

```bash
$ git status
```

## 2.3. 변경사항을 staged

### 2.3.1. 파일 전체

- 100MB 넘어가면 에러 발생

```bash
$ git add .
```

### 2.3.2. 특정 파일(폴더) 지정

````bash
$ git add a.txt			# 파일 하나
$ git add a.txt b.docx	# 파일 여러개
$ git add *.txt			# 특정 확장자만 (*은 뭐든지 가능하다는 의미)
$ git add test_folder/	# 특정 폴더 및 모든 하위 파일
````

### 2.3.3. stage 에서 제거(최초 커밋 이후)

``` bash
$ git restore --staged a.txt	# a.txt를 add 에서 삭제
```

### 2.3.4. stage 에서 제거(최초 커밋 전)

```bash
$ git rm --cached a.txt
```

## 2.4. commit

### 2.4.1. 기본 커밋

```bash
$ git commit -m '<커밋 메세지>'
```

### 2.4.2. 커밋 수정

- push 후에는 불가능

```bash
$ git commit --amend 
```

### 2.4.3. 없는 파일 커밋

```bash
$ git re <삭제된 파일 이름>
```

## 2.5. commit 조회

- 일정 이상 넘어가면 과하게 많은 커밋내역으로 에러 발생

### 2.5.1. 모든 커밋 내역 조회

```bash
$ git log
```

### 2.5.2. 최근 1개 커밋 조회

```bash
$ git log -1
```

### 2.5.3. 커밋 내역 한줄 조회

```bash 
$ git log --oneline
```

### 2.5.4. 최근 4개 한줄로

```bash
$ git log -2 --oneline
```

## 2.6. commit 이동

### 2.6.1. 특정 commit 이동

```bash
$ git checkout <commit id>
```

### 2.6.2. 최신 HEAD commit 이동

```bash
$ git checkout -
```

## 2.7. commit 삭제

- 가능하면 commit은 **삭제하지 않고** 갱신만 이어가는게 좋음.

### 2.7.1. 커밋취소+파일 unstaged

- 옵션이 없을경우 default 설정임

```bash
$ git reset --mixed HEAD^
$ git reset HEAD^
```

### 2.7.2. 커밋취소+파일 staged

```bash
$ git reset --soft HEAD^
```

### 2.7.3. 특정 커밋 취소

```bash
$ git reset HEAD~2	# 마지막 2개 커밋 취소
```

### 2.7.4. 커밋취소 + 파일삭제

- 별로 추천하지 않음

```bash
$ git reset --hard HEAD^
```

## 2.8. remote(원격저장소)

- github 사이트에 계정 및 원격저장소가 생성되어있다는 가정 하에 진행

### 2.8.1. git 폴더와 연결

```bash
$ git remote add origin https://github.com/<username>/<저장소이름>.git
```

### 2.8.2. push

- 커밋 내역을 원격저장소에 저장

- 최초커밋 및 git 폴더와 연결되어있지 않으면 불가능

```bash
$ git push origin master
```

### 2.8.3. 강제 push

```bash
$ git push origin master -f
```

### 2.8.4. pull

- 원격저장소의 커밋 내역을 불러옴

```bash
$ git pull origin master
```

### 2.8.5. 원격저장소 유무

```bash
$ git remote -v
```

### 2.8.6. 원격저장소 연결 삭제

```bash
$ git remote rm
```

### 2.8.7. 원격저장소 url 변경

```bash
$ git remote set-url origin https://github.com/<username>/<remotename>.git
```

### 2.8.8. clone

```bash
$ git clone https://github.com/<username>/<remotename>.git
```

로컬에서는 상관없지만 다른 곳에서 할 경우 (예: 회사 --> 집) 토큰이 필요함.

참고: [토큰생성방법](https://yian.tistory.com/38)



## 2.9. 그 외

### 2.9.1. main 을 master로 변경

- 위의 명령어로만 활용하면 이 경우는 없음

```bash
$ git branch -M master
```

### 2.9.2. 현재 내용 임시공간에 보관

```bash
$ git stash
```

### 2.9.3. 임시공간에 보관된 정보 가져옴

```bash
$ git stash pop
```

### 2.9.4. git 되돌리기

```bash
$ git reset --hard
```

### 2.9.5. 사용자 이름 설정

- 이걸 해주지 않으면 잔디가 생성되지 않는 경우가 있음

```bash
git config --global user.name 'My name'
git config --global user.email 'my_email@example.com'
```

## 원격 서버에서 ssh git push&clone

1. git init ~ git commit 까지는 동일

2. SSH key 생성 후 등록 (이미 한 경우 생략)

   ```bash
   # key 생성
   ssh-keygen -t ed25519 -C "your_email@example.com"
   # 공개 key 등록
   cat ~/.ssh/id_ed25519.pub
   # 위의 명령어로 출력되는 내용을 github 에 등록
   GitHub접속 → [Settings] → [SSH and GPG keys] → [New SSH key] 
   # ssh 연결 테스트
   ssh -T git@github.com
   ```

3. github 에 레파지토리 생성

4. SSH 방식으로 remote 연결

   ```bash
   git remote add origin git@github.com:your-username/my-project.git
   ```

5. push 진행

6. clone 을 하려는 경우
   ```bash
   git clone git@github.com:Kim-YoonHyun/mypackage.git
   ```


### git 커밋 메시지 명세

```bash
<type>: <short summary>

feat: 새로운 기능
fix: 버그 수정
docs: 문서 변경
refactor: 리팩토링 (기능 변화 없음)
test: 테스트 코드 추가/수정
chore: 빌드 설정, 패키지 업데이트 등 자잘한 작업
```

