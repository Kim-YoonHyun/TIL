# 1. 개요

# 2. 명령어

## 2.1. 자릿수 표현

### 2.1.1. format 활용

```python
num = 12345
f'{num:10d}		# 10칸 자릿수로 출력
f'{num:,d}		# 천 단위 쉼표적용
f'{num:10,d}		# 10칸 자릿수 + 천 단위 쉼표적용
```

### 2.1.2. format 미활용

```python
'%10s' %'python'
'%.2f' %2.463
```

### 2.1.3. 0값 채우기(1 -> 01, 001)

```python
a = '2'.zfill(4)	# return 0002
```

## 2.2. 정렬

```python
data = 'string'
f'{data:>16s}'		# 문자열 오른쪽 정렬
f'{data:<16s}'		# 문자열 왼쪽 정렬
```

## 2.3. 문자열 처리

### 2.3.1. 공백 제거

```python
' text '.strip()	# 양쪽 공백 제거
' text '.lstrip()	# 왼쪽 공백 제거
' text '.rstrip()	# 오른쪽 공백 제거
```

### 2.3.2. 공백 확인

```python
string.isspace()
```

### 2.3.3. 이모티콘 제거

- utf8 이 아닌 글자 제거

```python
def rmEmoji(inputData):
    return inputData.encode('utf-8', 'ignore').decode('utf-8')
```

- ascii 값이 없는 글자 제거

```python
# removing emoji
def rmEmoji_ascii(inputString):
    return inputString.encode('ascii', 'ignore').decode('ascii')

print(rmEmoji_ascii('🏡 corpo'))
```

## 2.4. 문자 병경

### 2.4.1. replace

```python
a = 'hello world'
a.replace('hello','hi')
```

