# TIL

## Python project code

### 2021년

#### 8월

##### 9일

- **파이참에서 mark down 활용법 검색**하였음. 추후 코드 리뉴얼 과정에서 정리사항을 이제부터 md 형식으로 정리가능할듯. 
  폴더에서 오른쪽 클릭 --> new --> file --> <이름>.md 를 통해 활용가능.
  [관련 사이트](https://www.jetbrains.com/help/pycharm/markdown.html)
- 현재 code 리뉴얼 과정에 있으며 기본적인 논리연산에서의 더욱 간결하고 python 스러운 표현방식, class 및 numpy 등을 더욱 효율적으로 활용하여 **코드를 깔끔하게 바꾸는 작업** 중.  
- 아직 class 사용 방법 및 어떤 구조에서 사용하는게 좋은지 등이 익숙치 않아 **의도적으로 사용하면서 연습중**

## Basic study

[Mark down basic](mark_down_basic.md)
[git basic](git_basic.md)

### 2021 년

#### 8월

##### 9일

1. [Mark down의 기본적인 사용법](mark_down_basic.md)
2. [Mark down 사용 예시 - python 정리](mark_down_python.md)
3. [git 명령어의 종류 및 이해](git_basic.md)
4. [git status 상세](status.md)

##### 10일

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

- [git branch](branch_basic)
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

##### 11일

- [pandas](pandas_basic.md)
  series, value 등 기본적인 개념 이해, 데이터 입출력, 인덱싱 방법, 데이터 조작, 인덱스 조작

##### 12일

- pandas 심화
  합성, 그룹분석

##### 13일

- matplotlib 기본
