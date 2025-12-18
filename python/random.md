# random

```python
import random
```

### random.random()

```python
# 난수 생성
random.randrange(10)        # 0 ~ 9 중 무작위 정수
random.randrange(1, 10)     # 1 ~ 9 중 무작위 정수
random.randrange(1, 10, 2)  # 1, 3, 5, 7, 9 중 하나
```

### random.randint()

```python
random.randint(1, 3)    # 1, 2, 3 중 하나
```

### 셔플

```python
a = ['a', 'b', 'c', 'd', 'e']
random.shuffle(a)
```

## 랜덤 선택

```python
a = ['a', 'b', 'c', 'd', 'e']
random.choice(a)
```

## 랜덤 샘플

- 중복없이 랜덤으로 갯수만큼 추출

```python
a = ['a', 'b', 'c', 'd', 'e']
random.sample(a, 2) 
```

# np.random

### np.random.randint

```python
# 10 이상 20미만 사이의 정수값 랜덤 선택
ary = np.randint(10, 20)
# 10 이상 20미만 사이의 정수값 랜덤으로 5개 선택
ary = np.randint(10, 20, size=5)
```

### np.arange

```python
# 0 이상 10 미만으로 간격 2만큼 생성
ary = np.arange(0, 10, 2)
```





