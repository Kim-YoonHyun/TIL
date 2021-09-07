# OS & glob

- OS는 경로, glob은 파일 리스트를 뽑을 때 특화된 모듈이다.

## 명령어 모음

### OS

```python
os.path.basename(__file__)		# 현재 스크립크 명(경로 제외)
os.listdir(path) 	# path 내의 파일 이름 리스트
str.endswith('확장자')		# 특정 확장자 설정
>>> import os
>>> fileDir = r"C:\Test"
>>> fileExt = r".txt"
>>> [_ for _ in os.listdir(fileDir) if _.endswith(fileExt)]
```

### glob

```python
from glob import glob
```

```python
glob('*.txt')		# 현재 디렉터리의 .txt 파일
glob(r'C:\U*')		# C:\ 경로에서 U로 시작하는 파일. 이때 r 은 원시(raw) 문자열
```

