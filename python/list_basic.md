# 1. 개요

# 2. 명령어

## 2.1. 갯수세기

```python
a = [1, 2, 3]
a.count(1)
```

## 2.2. 

```python
list(string)		# string 한 글자씩 전부 띄움 
.split(',')			# ,로 단어 구분
','.join(list)		# 리스트 내 단어들을 ,로 연결
```

## 인덱스 관련

### 리스트 값을 통해 인덱스 값 추출하기

```python
a = [100, 200, 300]
idx = a.index(200)
```

## 2.3. 리스트 내 인자 제거

```python
a = ['100', '2', 'asd', 'ccc']
a.remove('asd')
a.pop(2)
```

### 2.3.1. 정렬

```python
a.sort(reverse=True)
```

### 2.3.2. 비복원추출

```python
import random

my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 리스트에서 크기가 k인 비복원 추출
k = 3
sampled_list = random.sample(my_list, k)
```



