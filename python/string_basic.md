# String

## 명령어

#### 자릿수(format 사용)

```python
num = 12345
f'{num:10d}		# 10칸 자릿수로 출력
f'{num:,d}		# 천 단위 쉼표적용
f'{num:10,d}		# 10칸 자릿수 + 천 단위 쉼표적용
```

#### 자릿수(format 미사용)

```python
'%10s' %'python'
'%.2f' %2.463
```

#### 0 값 채우기 (01, 004)

```python
a = '2'.zfill(4)	# return 0002
```



### 정렬

```python
data = 'string'
f'{data:>16s}'		# 문자열 오른쪽 정렬
f'{data:<16s}'		# 문자열 왼쪽 정렬
```

### 문자열 처리

#### 공백제거

```python
' text '.strip()	# 양쪽 공백 제거
' text '.lstrip()	# 왼쪽 공백 제거
' text '.rstrip()	# 오른쪽 공백 제거
```

### 공백 확인

```python
string.isspace()
```

