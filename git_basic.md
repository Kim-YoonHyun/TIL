# git 특강

## 1. git

### GUI 와 CLI 의 차이

GUI(Graphic User Interface): 그래픽을 통한 제어

CLI(Command Line Interface): 줄 단위를 통한 제어

### CLI 기본 명령어

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

### git 기본 명령어

1. git add 
   - git add c.txt : 특정 파일
   - git add a.txt b.docx : 여러개 파일 동시에
   - git add "*.txt" : 확장자 명으로 동시에, *: 뭐든지 들어갈 수 있다는 의미
   - git add test_folder/ : 특정 폴더
2. git commit -m '커밋 메시지'
   - ㅁㄴㅇ

3. git status
   - git 저장소에 있는 파일의 상태를 확인하기 위하여 활용
     - 파일의 상태를 알 수 있음
4. git log
   - 현재 저장소에 기록된 커밋을 조회
   - 다양한 옵션을 통해 로그를 조회할 수 있음
     - git log -1 : 최근 1개만
     - git log --oneline : 한줄로
     - git log -2 --online : 최근 2개만 한 줄로

[노션 링크](https://www.datacamp.com/community/blog/python-numpy-cheat-sheet)

### 원격저장소 추가

```bash
$ git remote add origin <주소>
# origin 이라는 이름으로 <주소> 를 remote 에 추가
```

### 원격 저장소 push

```bash
$ git push origin master
```

### remote 삭제

```bash
$ git remote rm
```

git push -u origin master 를 할 경우 그냥 git push로 가능하지만







