# 1. 개요

set은 집합을 뜻한다.

set 으로 변환할 경우 중복이 제거된다.

# 2. 명령어

## 2.1. 기본 

```python
a = [1, 2, 3, 4, 5, 5, 5, 6, 7, 7]
print(set(a))

# return
# {1, 2, 3, 4, 5, 6, 7}
```

## 2.2. 연산

### 2.2.1. 교집합

```python
set1 = set([1,2,3,4,5,6])
set2 = set([3,4,5,6,8,9])

print(set1 & set2)
print(set1.intersection(set2))

# return
# {3, 4, 5, 6}
# {3, 4, 5, 6}
```

### 2.2.2. 합집합

``` python
set1 = set([1,2,3,4,5,6])
set2 = set([3,4,5,6,8,9])


print(set1 | set2)
print(set1.union(set2))

# return
# {1, 2, 3, 4, 5, 6, 8, 9}
# {1, 2, 3, 4, 5, 6, 8, 9}
```

### 2.2.3. 차집합

```python
set1 = set([1,2,3,4,5,6])
set2 = set([3,4,5,6,8,9])


print(set1 - set2)
print(set1.difference(set2))

{1, 2}
{1, 2}
```

### 2.2.4. 대칭 차집합

```python
set1 = set([1,2,3,4,5,6])
set2 = set([3,4,5,6,8,9])

print(set1 ^ set2)

{1, 2, 8, 9}
```

