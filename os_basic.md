# OS

## 명령어 모음

```python
os.path.basename(__file__)		# 현재 스크립크 명(경로 제외)
os.listdir(path) 	# path 내의 파일 이름 리스트
str.endswith('확장자')		# 특정 확장자 설정
>>> import os
>>> fileDir = r"C:\Test"
>>> fileExt = r".txt"
>>> [_ for _ in os.listdir(fileDir) if _.endswith(fileExt)]
```

