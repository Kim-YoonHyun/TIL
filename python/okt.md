# 1. 개요

# 2. 명령어

## 2.1. 설치

```bash
pip install konlpy
pip install JPype1
```

## 2.2. 기본 import

```python
from konlpy.tag import Okt
okt = Okt()
text = "아버지가 방에 들어가신다."
```

## 2.3. 분석

### 2.3.1. 형태소 분석

```python
okt.morphs(text)
```

### 2.3.2. 문구 추가

```python
okt.pos(text)
```



