# dictionary

## 기본 명령어

### dict 생성

#### key list 와 value list 를 통해 생성

```python
key_list = ['a', 'b', 'c']
value_list = [1, 2, 3]

dict(zip(key_list, value_list))
```

### key 추출

```python
dict.keys()	  # dict의 key를 가져옴
list(dict.keys())	# key 값들을 list 형태로 추출
```

```python
dict.items()  # dict의 key와 value 쌍을 가져옴
```

### 값 관련

#### 값 추가

```python
example = {}

example['a'] = 1
```

#### 값 수정

```python
example = {'a':1}

example['a'] = 2
```

#### 값 삭제

```python
example = {'a':1, 'b':2}

del(example['a'])
```

#### 키값 삭제 + 키에 할당된 값 추출

```python
example = {'a':1, 'b':2}

a_value = example.pop('a')
```



