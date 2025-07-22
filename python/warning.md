# 1. 개요

# 2. 명령어

## 2.1. 일반 warning

### 2.1.1. 무시하기

```python
import warnings
warnings.filterwarnings('ignore')
```

## 2.2. tensorflow warning

### 2.2.1. 무시하기

- `pip install silence_tensorflow` 를 통해 모듈 설치 필요

```python
from silence_tensorflow import silence_tensorflow
silence_tensorflow()
```

## 2.3. transformers logging

### 2.3.1. 무시하기

```python
from transformers import logging
logging.set_verbosity_error()
```



