# 1. 개요

python pandas 에 대한 명령어 모음

# 2. 명령어

## 논리

### 이상치 관련

- 특정값을 이상치로 변경

```python
data = {'A': [5, 12, 8, 10, 15], 'B': [10, 20, 5, 7, 3]}
df = pd.DataFrame(data)
'''
    A   B
0   5  10
1  12  20
2   8   5
3  10   7
4  15   3
'''
```

전체 df 에 대해서 A 열의 값이 10 이상인 행의 값을 전부 NaN 으로 변경

```python
df[df['A'] >= 10] = np.nan
'''
     A     B
0  5.0  10.0
1  NaN   NaN
2  8.0   5.0
3  NaN   NaN
4  NaN   NaN
'''
```

전체 df 에 대해서 A 열의 값이 10 이상인 A 열의 값만을 전부 NaN 으로 변경

```python
df['A'][df['A'] >= 10] = np.nan
'''
     A   B
0  5.0  10
1  NaN  20
2  8.0   5
3  NaN   7
4  NaN   3
'''
```

## 행 관련

```python
.dropna(subset=['col1'])	# col1 기준 결측치 삭제
.drop(columns='col1')		# col1 삭제
.sort_values(by='col1')		# col1 기준 정렬
.rename(columns={'old_nm':'new_nm'})  # 이름 변경

# 'speed' 단어 포함한 행만 추출
filtered = df[df['value1'].str.contains('speed', case=False, na=False)]
'''
case=False : 대소문자 미구분
na=False NaN 값이 있는 경우 에러가 아닌 False 취급
'''
```







