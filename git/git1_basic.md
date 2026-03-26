버전관리를 위한 커밋 도구

# 방법

1. git 생성
   ```bash
   git init
   ```

2. 상태 확인
   ```bash
   git status
   ```

3. 전부 add
   ```bash
   git add .
   ```

4. 커밋 진행
   ```bash
   git commit -m "initial commit"
   ```

5. github 사이트에서 원격저장소(레파지토리) 직접 생성

6. 생성된 레파지토리와 연동
   ```bash
   git remote add origin https://github.com/<username>/<저장소이름>.git
   ```

   ssh 로 인한 원격서버 연동시(key 설정 필요 --> 하단 상세 참고)
   ```bash
   git remote add origin git@github.com:<username>/<저장소이름>.git
   ```

7. push 진행
   ```git
   git push origin master
   ```

8. github 에서 push 되었는지 확인

9. 이후에는 remote 추가 과정없이 바로 진행 가능
   ```bash
   $ git status
   $ git add .
   $ git commit -m 'initial commit'
   $ git push origin master
   ```

# 상세

## commit 전

- git 시작

  - 해당 명령어를 최초로 git 버전 관리가 가능해짐

  - 숨김파일 형식으로 .git 폴더가 만들어짐

  - ~~<span style="color:red">절대로 상위폴더(또는 하위폴더)에 추가로 init 을 하면 안됨</span>~~
    ~~이 경우 git이 꼬여서 에러가 발생하기에 모든 .git 폴더를 지워버리고 새로 git을 시작하는것 밖에 답이 없음.~~

  - git 의 상위에 .git 을 생성하고 하위 .git 의 커밋 이력을 전부 옮기는 식으로 기존 커밋 보존 가능
    ```bash
    git init
    ```


- 상태 확인

  - 파일의 변경 사항, stage 여부 등 상태(status) 확인 가능

  - 보통 커밋 전 가장 먼저 확인용으로 입력하며 **필수 과정은 아님**
    ```bash
    git status
    ```


- 내부 내용 올리기

  - 100MB 넘어가면 에러 발생

  - 전체 올리기

    ```bash
    git add .
    ```

  - 특정 파일 지정
    ```bash
    $ git add a.txt			# 파일 하나
    $ git add a.txt b.docx	# 파일 여러개
    $ git add *.txt			# 특정 확장자
    $ git add test_folder/	# 특정 폴더 및 모든 하위 파일
    ```
    
  - add 취소
    ```bash
    git restore --staged .
    ```
  
    

## 내역 삭제

- stage 제거 (커밋 전)

  ```bash
  git rm --cached a.txt
  ```

- stage 제거 (커밋 후)
  ```bash
  git restore --staged a.txt
  ```

## commit

- 기본
  ```bash
  git commit -m "<메시지>"
  ```

- 수정(push 이후 불가능)
  ```bash
  git commit --amend
  ```

- "삭제된 상태" 를 commit
  ```bash
  git re <삭제된 파일명>
  ```

- 내역 조회
  ```bash
  git log
  ```

- 최근 1개 커밋 조회
  ```bash
  git log -1
  ```

- 커밋 내역 한줄 조회
  ```bash
  git log --oneline
  ```

- 최근 n 개 1줄로 조회
  ```bash
  git log -2 --oneline
  ```

## commit 이동

- 특정 id 이동
  ```bash
  git checkout <commit id>
  ```

- 최신 head 커밋 이동
  ```bash
  git  checkout -
  ```

## commit 삭제

- 취소 + unstaged
  ```bash
  git reset --mixed HEAD^
  git reset HEAD^
  ```

- 취소 + staged
  ```bash
  git reset --soft HEAD^
  ```

## commit 취소

- 마지막 2개 취소
  ```bash
  git reset HEAD~2
  ```

- 취소 + 파일삭제
  ```bash
  git reset --hard HEAD^
  ```

## remote 설정

- 기본적으로 github 계정 및 연동가능한 저장소가 존재해야 진행 가능

- git 대상 폴더와 연결
  ```bash
  git remote add origin https://github.com/<username>/<저장소이름>.git
  ```

- 원격서버에서 연결
  ```bash
  git remote add origin git@github.com:<username>/<저장소이름>.git
  ```

- 원격 저장소 유무 확인
  ```bash
  git remote -v
  ```

- 연결 삭제
  ```bash
  git remote rm
  ```

- url 변경
  ```bash
  $ git remote set-url origin https://github.com/<username>/<저장소이름>.git
  ```

## push

- 커밋 내역을 원격저장소에 저장

- 최초커밋 및 git 폴더와 연결되어있지 않으면 불가능
  ```bash
  git push origin master
  ```

- 강제 push
  ```bash
  git push origin master -f
  ```

- 대용량 파일을 올리는 경우
  LFS 를 쓰는 것을 할 수 있다.
  ※ 무료계정은 LFS 저장소를 1GB 까지만 제공함

  - LFS 설치
    ```bash
    curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
    sudo apt-get install git-lfs
    ```

  - 활성화
    ```bash
    git lfs install
    ```

  - 이미 설치된 100MB 파일을 LFS 로 전환
    ```bash
    git lfs migrate import --everything --include="대용량_파일명.확장자"
    ```

    

## pull

- 원격저장소의 커밋 내역을 불러옴
  ```bash
  git pull origin master
  ```

## clone

```bash
$ git clone https://github.com/<username>/<remotename>.git
```

로컬에서는 상관없지만 다른 곳에서 할 경우 (예: 회사 --> 집) 토큰이 필요함.

참고: [토큰생성방법](https://yian.tistory.com/38)

## 그 외

- main 을 master로 변경
  ```bash
  git branch -M master
  ```

- 현재 내용 임시공간에 보관
  ```bash
  git stash
  ```

- 임시공간에 보관된 정보 가져옴
  ```bash
  git stash pop
  ```

-  git 되돌리기
  ```bash
  git reset --hard
  ```

- 사용자 이름 설정
  이걸 해주지 않으면 잔디가 생성되지 않는 경우가 있음

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

   - remote 삭제하는 방법
     ```bash
     git remote remove origin
     ```

     

5. push 진행

6. clone 을 하려는 경우
   ```bash
   git clone git@github.com:Kim-YoonHyun/mypackage.git
   ```


## git 커밋 메시지 명세

```bash
<type>: <short summary>

feat: 새로운 기능
fix: 버그 수정
docs: 문서 변경
refactor: 리팩토링 (기능 변화 없음)
test: 테스트 코드 추가/수정
chore: 빌드 설정, 패키지 업데이트 등 자잘한 작업
```

