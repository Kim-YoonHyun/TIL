# 1. 개요

- colab 에서 코딩을 진행하는 경우 세팅을 다르게 해야하는 경우가 발생

# 2. csv

## 2.1. 파일 읽기

- pandas 뿐만 아니라 io 모듈도 같이 필요함

### 2.1.1. 파일 업로드

```python
from google.colab import files
uploaded = files.upload()
```

이후 파일을 선택하면 업로드 진행

### 2.1.2. csv 파일 읽기

```python
import pandas as pd
import io

data = pd.read_csv(io.BytesIO(uploaded['<파일이름>.csv']))
```





