# git 특강

## GUI 와 CLI 의 차이

GUI(Graphic User Interface): 그래픽을 통한 제어

CLI(Command Line Interface): 줄 단위를 통한 제어

## CLI 기본 명령어

- 프롬프트 기본 인터페이스
  - 컴퓨터 정보
  - 디렉토리
  - $

| 명령어 | 풀네임                  | 의미                 | 예시        |
| ------ | ----------------------- | -------------------- | ----------- |
| ls     | list                    | 목록                 |             |
| pwd    | print working directory | 현재 디렉토리 출력   |             |
| cd     | change directory        | 디렉토리 변경        |             |
| .      |                         | 현재 폴더            |             |
| ..     |                         | 상위폴더             |             |
| mkdir  | make directory          | 폴더 만들기          |             |
| touch  |                         | 0 byte의 파일 만들기 | touch a.png |

## git 기본 명령어

### git 상태 확인([상세설명](status.md))

```bash
$ git status
```

### 변경사항을 staged

```bash
$ git add . 			# 저장소 파일 전체
$ git add c.txt 		# 특정 파일 
$ git add a.txt b.docx  # 여러개 파일 동시에
$ git add "*.txt" 		# 확장자 명으로 동시에, *: 뭐든지 들어갈수 있다는 의미
$ git add test_folder/	# 특정 폴더
$ git restore --staged a.txt 	# a.txt 를 add 에서 삭제 (최초 커밋 이후)
$ git rm --cached a.txt 		# a.txt 를 add 에서 삭제 (최초 커밋 전)
```

### commit

#### commit 실행 및 수정

```bash
$ git commit -m '<commit mesage>'	# 커밋 하기
$ git commit --amend				# 커밋 수정(push 전에만 가능)
$ git re <삭제된 파일 이름>            # 없어진 파일을 commit 할 경우
```

#### 현재 저장소에 기록된 commit 조회

```bash
$ git log				# 모든 commit 조회
$ git log -1 			# 최근 1개만
$ git log --oneline		# 한 줄로 조회
$ git log -2 --oneline	# 최근 2개만 한 줄로
```

#### 특정 commit 으로 이동

```bash
$ git checkout <commit>
$ git checkout -		# 최신 HEAD commit 으로 이동
```

#### commit 삭제

```bash
$ git reset --soft HEAD^	# commit을 취소하고 파일은 staged 상태로 디렉터리 보존
$ git reset --mixed HEAD^	# commit을 취소하고 파일은 unstaged 상태로 디렉터리 보존(기본)
$ git reset HEAD^			# 위와 동일
$ git reset HEAD~2			# 마지막 2개의 commit 취소
$ git reset --hard HEAD^	# commit을 취소하고 파일도 삭제
```



---

### 원격저장소

#### 원격저장소와 로컬 저장소 연결

```bash
$ git remote add origin http://github.com/<username>/<저장소이름>.git
# origin 이라는 이름으로 <주소> 를 remote 에 추가
```

#### push & pull

```bash
$ git push origin master
# git push -u origin master 를 할 경우 이후 그냥 git push로 가능
$ git pull origine master
$ git push --set-upstream origin <branch_name> # branch 자체를 push
$ git push origin master -f #강제 push
```

#### 원격저장소가 있는지 없는지

```bash
$ git remote -v
```

#### remote 삭제

```bash
$ git remote rm
```

#### main 을 master로 변경하는 경우

```bash
$ git branch -M master
```

#### 현재 저장소 내용 임시공간에 보관

```bash
$ git stash				# 임시 공간에 보관
$ git stash pop			# 임시 공간에 보관된 정보 가져옴
```

#### git 수정 이전으로 내용 되돌리기

```bash
git reset --hard				# 모든 변경 파일 되돌리기
git checkout -- path/hello.c	# path/hello.c 의 변경 취소
```

### github 의 내용을 가져올 때

#### 기본 흐름

```bash
git init
git remote add origin https://github.com/<user_name>/<repo_name>.git
git pull origin master
```

#### 사용자 이름 설정

```bash
git config --global user.name 'My name'
git config --global user.email 'my_email@example.com'
```







