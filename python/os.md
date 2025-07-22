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

## 경로

### os.path.abspath

- 현재의 작업 디렉토리 기준 절대경로 생성

  ```python
  # 현재의 작업 디렉토리 경로 : /home/kimyh/python/project
  os.path.abspath('data')
  # --> /home/kimyh/python/project/data
  ```

- 실행중인 python 파일이 물리적으로 현재 존재하는 위치
  ```python
  # 파이썬 파일의 현재 위치 : /home/kimyh/python/project/whole_module/code.py
  os.path.abspath(__file__)
  # --> /home/kimyh/python/project/whole_module/code.py
  ```

만약 config.ini 에서 경로변수를 만든다음 값을 적어넣었다고 가정했을때

```ini
save_path = /data/kimyh/result
save_path = ./result
```

```python
os.path.abspath(save_path)
```

첫번째 방식은 설정된 값이 애초에 절대경로이기에 abspath() 를 해줘도 실질적인 차이는 없음. 그렇다면 무슨의미가 있는가? 하면 바로 상대경로를 지정해줬을 때의 방어가 가능하다는것에 의미가 있음. 두 번째 방식으로 한 경우 os.path.abspath() 를 하지 않으면 실행한 작업 디렉토리 장소에 따라서 위치가 변경될 위험이 있음.
즉, os.path.abspth() 는 위험을 방어하는 용도로 쓴다고 볼 수 있으며 절대경로값을 설정한것에 쓰면 어짜피 똑같은데 무슨 차이가 있냐는 건 중요한 논점이 아님.

### os.path.dirname

- 입력받은 경로의 디렉토리 부분 (가장 하위의 경로 제외) 만 추출

  ```python
  os.path.dirname('/home/kimyh/python/project/whole_module')
  # --> '/home/kimyh/python/project'
  ```

- 현재 실행 파일의 디렉토리까지만의 위치

  ```python
  os.path.dirname(os.path.abspath(__file__))
  # --> os.path.dirname('/home/kimyh/python/project/whole_module/code.py')
  # --> /home/kimyh/python/project/whole_module
  ```

```python
os.getcwd()
```

예시

### 경로 추가(생성)

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

### 2.2.3. 파일 유무 확인

```python
os.path.isfile(path)
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

## 2.4.폴더 생성

### 2.4.1. 단일 폴더 생성

```python
os.mkdir('<path>/폴더이름')	
```

### 2.4.2. 다중 폴더 생성

```python
os.makedirs('<path>/폴더1/폴더2/폴더3')
```



## glob

```python
from glob import glob
```

```python
glob('*.txt')		# 현재 디렉터리의 .txt 파일
glob(r'C:\U*')		# C:\ 경로에서 U로 시작하는 파일. 이때 r 은 원시(raw) 문자열
```

