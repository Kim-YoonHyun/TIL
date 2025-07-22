# 1. 개요

# 2. 명령어

## 2.1. 배열 생성

### 2.1.1. 인자값 0

```python
np.zeros((2, 3))	# shape=(2, 3)
```

### 2.1.2. 인자값 1

```python
np.ones((2, 3))
```

### 2.1.3. 인자값 사용자지정

```python
np.full((2, 3), 10)	# shape=(2, 3), 모든 인자 값=10
```

### 2.1.4. 대각선1, 나머지0

```python
np.eyes(3)
```

### 2.1.5. shape 지정

```python 
np.array(range(15)).reshape((3, 5))
```

### 2.1.6. 수열

```python
np.arange(1, 10, 2)	# 1에서 10까지 2의 간격으로 생성
```

### 2.1.7. 일정간격 분할 생성

```python
np.linspace(0.0, 5.0, 4) # 0.0 에서 5.0 까지 4분할
```

### 2.1.8.  조건 필터

```python
a = np.array([1, 2, 3, 4, 5, 6])
a[a > 3]
# return [4, 5, 6]
```

### 2.1.9. 상삼각행렬

```python
a = np.array([[1, 2, 3, 4], 
              [5, 6, 7, 8],
              [9, 10, 11, 12],
              [13, 14, 15, 16]])
b = np.triu(a)
'''
array([[ 1,  2,  3,  4],
       [ 0,  6,  7,  8],
       [ 0,  0, 11, 12],
       [ 0,  0,  0, 16]])      
'''
c = np.triu(a, k=1)
'''
array([[ 0,  2,  3,  4],
       [ 0,  0,  7,  8],
       [ 0,  0,  0, 12],
       [ 0,  0,  0,  0]])
'''
d = np.triu(a, k=-1)
'''
array([[ 1,  2,  3,  4],
       [ 5,  6,  7,  8],
       [ 0, 10, 11, 12],
       [ 0,  0, 15, 16]])
'''
```

### 2.1.10. 범위 내 랜덤 생성

```python
np.random.randint(0, 10, size=20) # 범위 0 ~ 9 에서 20 크기만큼 생성
np.random.rand(5, 8) # 범위 0 ~ 1 에서 (5, 8) 크기만큼 생성
```



## 2.2. random

````python
np.random.randint(0, 10, size=20)	# 범위 0 ~ 10-1 에서 20 크기만큼 생성
np.random.choice(5, 3)					# 범위 5에서 값 3개 복원 추출
np.random.choice(5, 3, replace=False)	# 범위 5에서 값 3개 비복원 추출
np.random.choice([1, 2, 3, 4])	# 리스트 내에서 값 하나 랜덤 선택
````

## 2.3. 삽입

```python
numpy.insert(arr, obj, values, axis=None)
```

## 2.4. 최댓(최솟)값

최댓값 문법의 max 부분을 min 으로 바꾸면 최솟값 문법이 된다.

### 2.4.1. 최댓값 추출

```python
np.max(a, axis=0)				# a에서 최대값 추출
np.maximum(a, b)		# a, b 를 비교하여 최대값으로 재구성
np.maximum.reduce(a)	# a에서 axis에 따른 최대값으로 재구성
```

### 2.4.2 최댓값의 index 추출

```python
np.argmax()
```

## 2.5. 인덱싱

### 2.5.1. index 를 통한 추출

```python
np.take(a, index, axis=0)		# a에서 axis를 기준으로 지정한 index 값을 추출
```

```python
np.take_along_axis(a, ai, axis)		# a를 ai에 따라 axis의 값으로 재구성
# ai 는 numpy array를 넣어야한다.
# a와 ai의 차원수는 동일해야한다. a.shape = (2, 1, 3, 4) ---> ai.shape = (1, 1, 2, 1)
# ai에서 조정하는 구조와 axis의 위치는 일치해야한다.
# axis 를 기준으로 ai에 지정한 모든 index 값으로 재구성됨
# 예시
# 1. axis 0 을 기준으로 index 값을 조정하고 싶은 경우
a = np.arange(0, 100).reshape(2, 5, 2, 5)
ai = np.array([
    [
        [
            [0] # axis 0의 모든 index 0의 값으로 이루어진 ary
        ]
    ],
    [
        [
            [0]	# axis 0의 index 0의 값으로 이루어진 ary가 추가됨
        ]
    ]
])
b = np.take_along_axis(a, ai, axis=0)

# 2. axis 1을 기준으로 index 값을 조정하고 싶은 경우
ai = np.array([
    [
        [
            [1] # axis 1의 모든 index 1의 값으로 이루어진 ary
        ],
        [
            [1] # axis 1의 모든 index 1의 값으로 이루어진 ary
        ]
    ]
])
b = np.take_along_axis(a, ai, axis=1)

# 3. axis 2을 기준으로 index 값을 조정하고 싶은 경우
ai = np.array([
    [
        [
            [0], # axis 2의 모든 index 1의 값으로 이루어진 ary
            [0]  # axis 2의 모든 index 0의 값으로 이루어진 ary 추가
        ]
    ]
])
b = np.take_along_axis(a, ai, axis=2)

# 3. axis 3을 기준으로 index 값을 조정하고 싶은 경우
ai = np.array([
    [
        [
            [0, 0] # axis 3의 모든 index 0, 0의 값으로 이루어진 ary
        ]
    ]
])
b = np.take_along_axis(a, ai, axis=2)

# 4. 모든 index 값이 아닌 지정 index 값을 불러오고 싶은 경우
ai = np.array([
    [
        [
            [0], # axis 2의 index 0의 값으로 이루어진 첫번째 ary
        ]
    ],
    [
        [
            [1]	 # axis 2의 index 1의 값으로 이루어진 두 번째 ary
        ]
    ],
    [
        [
            [0]	 # 입력된 a의 크기보다 크게 설정하는 것은 불가능
        ]
    ],
])
b = np.take_along_axis(a, ai, axis=2)
```

## 2.6 구조 변경

### 2.6.1. 전치

```python
np.transpose(a)		# a를 전치
np.swapaxes(a, axis1, axis2)	# a의 axis1과 axis2를 변경
```

### 2.6.2. 상하 반전

```python
a = np.arange(12).reshape(4, 3)
a_flipud = np.flipud(a)
```

### 2.6.3. 좌우 반전

```python
a = np.arange(12).reshape(4, 3)
a_fliplr = np.fliplr(a)
```

### 2.6.4. 상하좌우 반전

```python
a = np.arange(12).reshape(4, 3)
a_flip = np.flip(a)
```



## 2.7. 원소 찾기 & 대체

```python
ary = np.array([1, 2, 3, 4, 5])

np.where(ary > 3)	# 3보다 큰 값은 True 아니면 False
np.where(ary > 3, 30, -3)	# 3보다 큰 값은 30 아니면 -3
np.where(ary > 3, 30, ary)	# 3보다 큰 값은 30 아니면 그대로
```



## 2.7. take

### 2.7.1 생성시 적용

```python
a = [4, 3, 5, 7, 6, 8]
indices = [0, 1, 4]
np.take(a, indices)
array([4, 3, 6])
```

## 2.8. 숫자 세기

### 2.8.1. np.count

```python
a = [4, 3, 4, 4, 6, 8]
a.count(4)
```



### 2.8.1. np.unique

```python
unique_ary = np.unique(value_list)
```

#### option - return_counts

```python 
unique_ary, count_idx = np.unique(value_list, return_counts=True)
```

## 2.9. 복제

```python
a = np.array([5, 6])

np.tile(a, 3)
# [5, 6, 5, 6, 5, 6]

np.tile(a, (2, 2))
'''
[
    [5, 6, 5, 6],
    [5, 6, 5, 6]
]
'''
```

## 2.10. 타입 변경

```python
a = a.astype(int)
a = np.array(a, dtype=int)
```

## 2.11. 결측치

### 2.11.1. 

```python
a = ary
np.isnan(a)
```

### 2.11.2. 결측값을 0

```python
np.nan_to_num(a)
```

### 2.11.3. 결측값을 특정 숫자

```python
np.nan_to_num(a, nan = -1)
```

## 2.12. 연산

### 2.12.1. 제곱

```python
np.square(ary)
```

### 2.12.2. 제곱근

```python
np.sqrt(ary)
```

### 2.12.3. 표준편차(STD)

```python
np.std(ary)
```

