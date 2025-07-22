# 1. 개요

# 2. 명령어

# 2.1. 기본 import 

```python
import random
```

## 난수 생성

```python
random.random()
```

## 범위 내 정수 무작위 추출

```python
random.randrange(1, 7)	# 1 에서 (7-1) 까지의 랜덤 정수
```

## 셔플

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

