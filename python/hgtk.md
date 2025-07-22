# 1. 개요

# 2. 명령어

## 2.1. 초기 import

```python
import hgtk
```



### 2.1.1. 한글 음절 분리

```python
word = hgtk.letter.decompose('갈')
```

### 2.1.2. 한글 음절 결합

```python
word = hgtk.letter.compose('ㄱ', 'ㅏ', 'ㅁ')
```

