# numpy

## 기본 명령어

### random

````python
np.random.choice(5, 3)					# 범위 5에서 값 3개 복원 추출
np.random.choice(5, 3, replace=False)	# 범위 5에서 값 3개 비복원 추출
````

### 인덱싱

```python
numpy.insert(arr, obj, values, axis=None)
```

```python
np.max(a, axis=0)				# a에서 최대값 추출
np.maximum(a, b)		# a, b 를 비교하여 최대값으로 재구성
np.maximum.reduce(a)	# a에서 axis에 따른 최대값으로 재구성
np.linspace(0.0, 5.0, 4) # 0.0 에서 5.0 까지 4분할
np.take(a, index, axis=0)		# a에서 axis를 기준으로 지정한 index 갑을 추출
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



### 전치

```python
np.transpose(a)		# a를 전치
np.swapaxes(a, axis1, axis2)	# a의 axis1과 axis2를 변경
```



