# OS & glob

- OS는 경로, glob은 파일 리스트를 뽑을 때 특화된 모듈이다.

## OS

```python
os.path.basename(__file__)		# 현재 스크립크 명(경로 제외)
os.listdir(path) 	# path 내의 파일 이름 리스트
str.endswith('확장자')		# 특정 확장자 설정
>>> import os
>>> fileDir = r"C:\Test"
>>> fileExt = r".txt"
>>> [_ for _ in os.listdir(fileDir) if _.endswith(fileExt)]
```

### 현재 작업 폴더 얻기

```python
os.getcwd()
```

### 절대경로 얻기

```python
os.path.abspath(path)
```

### 경로에서 디렉토리명 추출

```python
os.path.dir(path)
```

### 경로에서 파일명 추출

```python
os.path.basename(path)
```

### 현재 작업 파일의 경로 추출

```python
os.path.dirname(os.path.abspath("__file__"))
```

### 디렉토리, 파일명 동시 추출

```python
os.path.split(path)
```

### cmd 명령어 제어

#### cmd 명령어 단순 실행

```python
os.system('<cmd 명령어>')
os.system('dir')
os.system('pyinstaller <파일이름>.py')
```

#### cmd 명령어 실행값 가져오기

```python
f = os.popen('<cmd 명령어>')
print(f.read())
```





## glob

```python
from glob import glob
```

```python
glob('*.txt')		# 현재 디렉터리의 .txt 파일
glob(r'C:\U*')		# C:\ 경로에서 U로 시작하는 파일. 이때 r 은 원시(raw) 문자열
```

