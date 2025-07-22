# 1. 개요

파이썬 exe 파일 제작.

우선 윈도우에 python이 설치되어 있어야함.
python 설치시 add to PATH 체크 필요.ㅔㅕ

exe를 만들고자 하는 py 파일이 있는 곳에서
shift + 마우스 우클릭 -> powershell 을 통해 진행

# 2. 명령어

## 2.1. exe 파일 

### 2.1.1. 기본 생성법

```bash
pyinstaller <파일이름>.py
```

### 2.1.2. 실행 시 콘솔 창 제거

```bash
pyinstaller -w <파일이름>.py
pyinstaller --noconsole <파일이름>.py
```

### 2.1.3. exe 실행파일 하나만 생성.

```bash
pyinstaller -F <파일이름>.py
pyinstaller --onefile <파일이름>.py
```

### 2.1.4. exe 파일 이름 지정

```bash
pyinstaller -n <exe 이름>.exe <파일이름>.py
```

### 2.1.5. icon 지정

```bash
pyinstaller --icon=<icon>.ico <파일이름>.py
```

## 2.2. .spec 파일

exe 파일을 만든 py 파일에서 활용하는 각종 data 및 모듈을 추가할 수 있는 곳.

### 2.2.1. exe에 파일 또는 폴더 포함

`datas = []` 에 
`('프로젝트 내의 경로', '실행파일에 적용할 형태')`입력.

```python
datas = [('./image1.png', '.'),				# 해당 경로의 image1.png 파일을 파일 형태로
        ('./folder1', './folder2')] 		# 해당 경로의 folder1 폴더를 folder2 폴더 형태로.
```

모든 설정이 끝나면

```bash
pyinstaller <파일이름>.spec
```

을 통해 spec을 업데이트
