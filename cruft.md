`cruft` 는 프로젝트 코드가 얼마나 바뀐 것을 감시하는 도구가 아니라 "baseline" 의 몇 번째 버전에 머물러 있는가 를 확인함.
즉, 한번 `cruft` 를 통해 `baseline` 을 받았다면 그 `baseline` 자체가 업데이트 되어 `update` 시도되는게 아닌 이상 개발 디렉토리에서는 **변경이 있다고 감지하지 않음**

**주의점**: 
사용되는  `cruft` 는  `pip install cruft` 를 통해 활용되는 패키지이며 `sudo apt install cruft` 로 설치되는 Linux 의 `cruft` 와는 다른 것이다.

자동 업데이트의 방향성은 항상 baseline --> workspace 이기 때문에 workspace 에서 baseline 으로의 업데이트는 개발자 본인이 직접 수정하는 식으로 해야한다.

## 생성 순서

1. {{cookiecutter.<변수명>}} 을 적용한 형태로 baseline 폴더&파일 구조를 작성
   폴더 구조 예시

   ```Plaintext
   library/  (git 저장소)
   ├── {{cookiecutter.package_name}}/      <-- 패키지이름
   │   ├── {{cookiecutter.package_name}}   <-- 내부 설정 파일
   │   │   ├── utils                       <-- 패키지 내부 서브 모듈
   │   │   │   ├── __init__.py
   │   │   │   └── utils.py
   │   │   └── __init__.py
   │   ├── docs                            <-- 함수상세.md 모음 문서폴더
   │   │   └── def.md
   │   ├── scripts                         <-- 스크립트 코드 모음 폴더
   │   │   └── upload.py                   <-- PyPI 업로드 자동화 코드
   │   ├── test                            <-- 패키지 테스트 폴더
   │   ├── .pypirc                         <-- (미사용)
   │   ├── MANIFEST.in                     <-- 공통 배포 파일(미사용)
   │   ├── pyproject.toml                  <-- 내부 설정 파일
   │   └── README.md                       <-- 설명 파일
   ├── cookiecutter.json         <-- 설정값 정의 파일 (새로 생성)
   └── .gitignore                <-- git 전용 ignore 설정 파일
   ```

   파일 내부 값 예시
   ```toml
   ...
   [project]
   # 패키지 이름 (pip install 시 사용될 이름)
   name = "{{cookiecutter.package_name}}"
   # 버전
   version = "{{cookiecutter.version}}"
   # 패키지 설명
   description = "{{cookiecutter.description}}"
   # 최소 지원할 파이썬 버전
   requires-python = "{{cookiecutter.python_ver}}"
   # 작성자 정보
   authors = [
     {
       name = "{{cookiecutter.author_name}}", 
       email = "{{cookiecutter.email}}" 
     }
   ]
   ...
   ```

2. `cookiecutter.json` 생성
   폴더트리 및 파일 내부의 {{}} 에 선언된 부분에 대해서 cruft 기반 baseline 을 create 할 때 각 변수에 대응되는 설정용 파일이라고 볼 수 있다. 기본적으로 json 형태이며 key 값은 {{}} 에 설정된 변수명과 동일하게 해야하고 values 값의 경우 "질문 시 () 안에 적히는 예시 문구" 의 역할이며 **값을 미리 정하는 것이 아니다.**

   ```json
   {
     "package_name": "패키지 이름 입력",
     "version": "버전 예시: 0.0.1",
     "description": "패키지 설명 입력",
     "python_ver": "예시: >=3.10",
     "author_name": "이름 입력",
     "email":"이메일 입력"
   }
   ```

   value 에 리스트로 넣는 경우 생성시 방향키로 선택가능함
   
   ```json
   {
     "package_name": "패키지 이름 입력",
     "version": "버전 예시: 0.0.1",
     "type":["system", "package"],  
     "description": "패키지 설명 입력",
     "python_ver": "예시: >=3.10",
     "author_name": "이름 입력",
     "email":"이메일 입력"
   }
   ```
   
   
   
3. github 연동 진행 
   `cruft` 는 `git` 을 통해 작동하는 도구이므로 반드시 `git` 과 연동이 되어있어야 한다.
   `cruft` 을 위한 특별 설정은 필요없고 github 연동까지의 과정을 평범하게 진행

## 활용 순서

1. `cruft` 기반 baseline 받아오기
   github 에 올라가 있는 내용을 불러온다. 이 과정에서 "질문" 이 진행되며 값을 전부 입력시 입력된 값이 적용되어있는 baseline 기반의 폴더트리가 생성된다.

   ```bash
   cd /path/to/workspace
   cruft create https://github.com/<username>/<저장소이름>
   ```

   이때 만약 linux 에 설치된 `cruft` 와 겹쳐져서 옵션 오류가 발생하는 경우
   ```bash
   python -m cruft create https://github.com/<username>/<저장소이름>
   ```

2. 결과 확인

3. baseline --> 작업 디렉토리 업데이트
   만약 baseline 이 업데이트 되었다면 **cruft create 로 생성한** 작업 디렉토리 내부에서 

   ```bash
   cruft update
   # 패키지 겹침 오류 발생 시
   python -m cruft update
   ```

   이때, `cruft` 의 `git` 활용 특성상 해당 작업디렉토리 또한 `git` 이 만들어져 있어야 한다. 단, github 연동은 필수사항이 아니며 commit 까지만 되어있으면 충분함.

4. 선택지 선택
   정상 진행 시 y, n, s, v 의 4개 선택지가 나오며 선택지의 의미는 아래와 같다.

   ```bash
   y : 변경 사항 즉시 적용
   n : 변경 사항 무시(기존 파일을 보존하는 용도 등)
   v : 뭐가 바뀌었는지 눈으로 확인
   s : 변경될 전체 내용을 보여주거나 보류
   ```

   `v` 를 통해 변경사항을 확인하고 `y` 로 진행하는 것을 추천
   이때, `v` 로 내용을 확인하면 terminal 에 출력되는데 `q` 를 입력하면 나올 수 있다.

### 연결

cruft create 를 해서 baseline 을 통해 생성한 적이 없는 기존의 개발 작업 디렉토리의 경우 link 를 통해 
"현 작업 디렉토리는 baseline 에서 태어난 것" 이라고 강제선언할 수 있음.

```bash
python -m cruft link https://github.com/<username>/<저장소이름>
```

단, 이 경우는 "현재 작업 디렉토리" 는 구버전인데 baseline 은 최신버전임에도 불구하고 지금은 최신버전이다 라고 선언해버리기에 의도한 버전 업데이트가 되지 않는다.
이 때문에 첫 옮김에 한해서는 직접 수동으로 옮기고 그 다음 link 를 진행하여 현재를 최신이다라고 보는 것이 최선이다.
단, 일일히 눈으로 보기보단 vscode 의 compare 기능 등을 사용하는 것이 추천된다.
