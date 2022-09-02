# 1. 개요

- 이미지를 학습용 tensor 로 활용할 때 주로 활용하는 모듈
- 데이터로더에 보통 사용됨.
- 이미지의 메타정보를 불러오며, transforms 을 통해 값을 추출 가능
- 추출되는 값은 기본적으로 1/255 값이 곱해져 있음
- () 의 shape 으로 불러옴.

# 2. 명령어

## 2.1. 기본 import

```python
from PIL import Image
```

## 2.2. numpy 에서 PIL 변환

```python
pil_image = Image.fromarray(np_ary)
```



### 2.1.1. 상세 분류(설명)
