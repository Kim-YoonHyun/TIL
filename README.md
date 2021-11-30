# TIL

## 2021년

### 8월

9일

- **파이참에서 mark down 활용법 검색**하였음. 추후 코드 리뉴얼 과정에서 정리사항을 이제부터 md 형식으로 정리가능할듯. 
  폴더에서 오른쪽 클릭 --> new --> file --> <이름>.md 를 통해 활용가능.
  [관련 사이트](https://www.jetbrains.com/help/pycharm/markdown.html)
- 현재 code 리뉴얼 과정에 있으며 기본적인 논리연산에서의 더욱 간결하고 python 스러운 표현방식, class 및 numpy 등을 더욱 효율적으로 활용하여 **코드를 깔끔하게 바꾸는 작업** 중.  
- 아직 class 사용 방법 및 어떤 구조에서 사용하는게 좋은지 등이 익숙치 않아 **의도적으로 사용하면서 연습중**

- python의 기본적인 불문률

  - [python style guide](https://www.python.org/dev/peps/pep-0008/)

    ```
    # 예시
    a = (b*5 + 5) / 2   	# 핵심 연산자를 띄우고 보기 편하게 묶는다.
    if a == (b*5 + 5)/2:
    ```

  2. 함수이름/변수이름 => snake_case

  3. 클래스이름 => CamelCase

- [commit 영어사전](https://blog.ull.im/engineering/2019/03/10/logs-on-git.html)

- [gitignore.io](https://www.toptal.com/developers/gitignore)

  - python, window 등 키워드를 검색하면 해당 소프트웨어 관련 보편적인 ignore 키워드가 표시됨.
  - 소스코드를 제외한 환경설정 코드 등, 자신의 환경 이외에는 쓸모없을 수 있는 각종 설정 파일들을 제외시키기 위한 목점이 큼.
  - 특정 확장자로 설정 가능. 예시: *.txt
    특정 폴더 설정가능. data/

- github pages

  1. git 업로드할 폴더내부에 **index**.html 생성(다른 이름은 X)

  2. <username>.github.io 로 원격저장소 이름 설정

  3. setting 에서 아래로 내려서 GitHub Pages에 들어감
  4. - [x] Your site is published at https://<username>.github.io/ 되어 있으면 생성 완료

- git branch
  - 버전관리에 필수적
  - 협업시 각자의 branch를 통해 각자의 버전을 작성하며
    이를 통합 및 조정하는 과정은 팀장의 몫
    
  
- git stash

  모든 커밋 시점을 복원이 가능. 되돌리기 가능.
  그래서 일반적으로 모든 작업을 할 때는 커밋하면서 이어나감.
  가끔 작업을 하던 도중 어떠한 것을 반영해야할 때가 있음.
  작업 하던 것을 잠시 보관 > stash

  ```bash
  $ git stash
  ```

  임시공간 보관하고

  ```bash
  $ git stash pop
  ```

  다시 꺼내옴

11일

- [pandas](pandas_basic.md)
  series, value 등 기본적인 개념 이해, 데이터 입출력, 인덱싱 방법, 데이터 조작, 인덱스 조작

12일

- pandas 심화
  합성, 그룹분석

13일

- matplotlib 기본

17일~20일

- crawling 기본, 유튜브 랭킹, 인스타그램 맛집 등

21일

- 자료구조+알고리즘을 전반적으로 정리
- 자료구조(요리의 재료 및 다듬는 방법), 알고리즘(요리 방법)
- 표현법 : 언어, 순서도, 의사코드, 코딩.....
- 성능 : 빅오 표기법 (그래프)
- 간단한 동물원 알고리즘 >> 정답이 아니라 최적답이 있을 수 있음
- 자료구조 종류:
  1. 선형: 선형리스트, 단순연결리스트, 원형연결리스트, 스택, 큐
  2. 비선형: 트리, 그래프

23일 

- crawling 자체 실습 및 각종 명령어 정리.
- matplotlib, pandas 정보 추가

25일

- class 내용 추가 정리
- 기존 코드를 함수, 변수, 코드 순으로 재정리

31일

- crawling, git 내용 추가
- word cloud 생성 방법 정리

### 9월

~16일

- 텍스트 전처리

28일

- git basic 내용 수정
- Linux 커널 관련 기본 지식 추가

30일

- mark down 글자 색 변경 추가

### 10월

5일

- 팀워크 code 제작시 가독성 좀 더 신경 쓰기

7일

- 엑셀 가계부를 활용해서 Power BI 보고서 만들기 연습

12일

- CT영상판독 정리 추가 수정
- ADsP 데이터 마트 부분 진행 완료

14일

- 통계 자료획득, 통계분석, 확률계산, 통계자료, 인과관계 부분 정리
  관련 용어들 정리하고 기록.

15일

- 가설, 추정, 회귀분석 부분 정리
- 가능하면 너무 이론적으로 파고들지 않고 적당히 진행 필요

### 11월

2일

- github profile 오픈소스 파악

5일 

- R 기초 학습 중

8일

- R 기초 코드 정리

11일

- json 내용 추가

15~16

- GUI 추가

- 스크롤바 설정하는 방법에 대해서 공부

17일

- dict 값 제거 부분 추가
- list 값 찾기 부분 추가
- string 공백 여부 확인 추가

18일

- pyinstaller 설치를 통해 GUI를 exe 파일로 전환하기
  윈도우 cmd에 pip install pyinstaller 를 설치해야한다.
- 파이썬에서 cmd 명령어 사용하기.
  os.system을 통해서 제어
- class 에서 self, method, instance 의 개념을 다시 한번 정리.
  instance: class 에 할당되는 객체
  method: 클래스에 정의되는 동작
  self: class 에 할당된 인스턴스
- GUI: 위젯에 바인딩부분에서 event를 제외하고 변수를 넣으면 자동실행되는 현상을 어떻게 제어불가능한지 여부를 찾아보고 있음. > event 형식 아니면 안됨. > 변수를 직접 불러오는 방식으로 사용.
  현재 수정버튼 작업중.

19일

- dict 에 내용 추가.
  pop, 
- GUI 에 내용 추가.
  Canvas, canvas 그리기, 위젯 수정

30일

- QGIS 오류 수정 방법 확인
