# 1. 개요

- OS는 경로 제어에 특화된 모듈이다.

- 작업경로: 현재 작업 중인 경로

  ```python
  'C:/Users/OOO/Desktop/spyder_project/kidney_cancer/<파일>'
  ```

- 상대경로: 작업경로를 기준으로 결정되는 경로

  ```python
  (작업경로) 'trained_model/xception/<파일>'
  ```

- 절대경로: 작업경로와 상관없이 해당 파일이 위치하는 경로

  ```python
  'C:/Users/OOO/Desktop/spyder_project/kidney_cancer/trained_model/xception/<파일>'
  ```

- 모든 경로 명령어에 있어서 '/' 가 포함되어있으면 그 값을 root 로 인식하게 됨.

# 2. 명령어

## 2.1. 경로

### 2.1.1. 현재 작업중인 파일의 경로(이름 포함)

```python
__file__
# __file__ 은 .py 자체를 실행했을때 (spyder 기준 F5 or cell 실행) 만 인식됨.
# 스크립트 라인별 개별 실행(F9) 로는 인식되지 않음.
```

### 2.1.2. 작업경로 추출

```python
os.getcwd()
```

예시

### 2.1.3. 입력한 path 값에서 파일명(가장 마지막 부분)을 제외한 경로 추출.

```python
os.path.dirname(path)
```

```python
os.path.dirname(
    'Desktop/spyder_project/kidney_cancer/trained_model'
)
return : 'Desktop/spyder_project/kidney_cancer'
```

### 2.1.4. 작업 경로를 기준으로 입력한 path 값에 대한 절대 경로 생성

```python
os.path.abspath(path)
```

```python
os.path.abspath(
    'Desktop/spyder_project/kidney_cancer/trained_model'
)
return : '''C:/Users/XXX/Desktop/spyder_project/kidney_cancer/
					Desktop/spyder_project/kidney_cancer/trained_model'''

# 단, 입력된 path값 자체가 이미 절대경로인 경우에는 그대로 추출됨.
os.path.abspath(
    'C:/Users/XXX/Desktop/spyder_project/kidney_cancer/trained_model'
)
return : 'C:/Users/XXX/Desktop/spyder_project/kidney_cancer/trained_model'
```

### 2.1.5. 경로 추가(생성)

```python
os.path.join(path1, path2, path3, ...)
```

```python
os.path.join('Desktop/spyder_project', 'kidney_cancer/trained_model')
return: 'Desktop/spyder_project/kidney_cancer/trained_model'
```

### 2.1.6. 입력한 path 값에서 파일명(가장 마지막 부분)만 추출.

```python
os.path.basename(path)
```

```python
os.path.basename(
    'Desktop/spyder_project/kidney_cancer/trained_model'
)
return: 'trained_model'
```



```python



```

### 2.1.7. 경로, 파일명 동시 추출

```python
os.path.split(path)
```

```python
os.path.basename(
    'Desktop/spyder_project/kidney_cancer/trained_model'
)
return: ('Desktop/spyder_project/kidney_cancer','trained_model')
```

## 2.2. 파일(폴더) 읽기

### 2.2.1. path 내의 파일 이름 리스트

```python
os.listdir(path) 	# path 내의 파일 이름 리스트
```

### 2.2.2. 특정 확장자 설정

```python
str.endswith('확장자')		# 특정 확장자 설정
>>> import os
>>> fileDir = r"C:\Test"
>>> fileExt = r".txt"
>>> [_ for _ in os.listdir(fileDir) if _.endswith(fileExt)]
```



## 2.3. cmd 명령어 제어

### 2.3.1. cmd 명령어 단순 실행

```python
os.system('<cmd 명령어>')
os.system('dir')
os.system('pyinstaller <파일이름>.py')
```

#### 2.3.2. cmd 명령어 실행값 가져오기

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

