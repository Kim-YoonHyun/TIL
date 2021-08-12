# Pandas

csv 파일을 읽고 쓸 수 있음.
시리즈와 데이터프레임을 만들 수 있음.

## 기본적인 개념

### Series

> 기본적인 생성법

```python
s = pd.Series([100, 200, 300])	# values
s = pd.Series([100, 200, 300], index=['인덱스0', '인덱스1', '인덱스2'], dtype=np.int32)	# value, index
s2 = pd.Series({'인덱스0':300, '인덱스1':200, '인덱스2':100})	# dictionary (잘 쓰이진 않음)
```

> 불러오기

```python
# index 불러오기
s.index			# 모든 index를 index로 불러옴
s.index.values	# 모든 index를 array로 불러옴
-------------------------------------------------------------------
# value 불러오기
s[0]		 # 0 번째 행의 value 추출
s['인덱스0']  # '인덱스0' 행의 value 추출
s.인덱스0	   # .한글 가능
s.values	 # 모든 values값을 array로 불러옴

s['인덱스3'] = 400 	# '인덱스3' 이라는 새로운 행을 추가하고 400 할당.
del s['인덱스3']		# '인덱스3' 행 제거
-------------------------------------------------------------------
# Series에서 Series추출하기
s[[0]]				   		# 0 번째 행의 Series 추출
s[['인덱스0']]				  # '인덱스0' 행의 Series 추출
s[[0, 2]]			   		# 0, 2 번째 행의 Series 추출
s[['인덱스0', '인덱스2']]		# '인덱스0', '인덱스2' 행의 Series 추출

s[1:3]			   	  	# 0에서 3 - 1 까지 행의 Series 슬라이싱
s['인덱스1':'인덱스2']	# '인덱스1' 에서 '인덱스2' 까지 행의 Series 슬라이싱
### 슬라이싱 시 숫자로 할 경우 마지막 번호는 제외되지만 이름으로 할 경우 마지막도 포함됨.
------------------------------------------------------------------
# Series 필터링
s[(100 < s) & (s < 300)]	# 값 100 에서 300 사이 시리즈 필터링
s[(s < 100) | (s > 200)]	# 값 100 미만 그리고 200초과 시리즈 필터링
```

> 이름 설정

```python
s.name = '인덱스 이름'			# index 이름 설정
s.index.name = '시리즈 이름'		# series 이름 설정
```

> 인덱스 기반 연산 가능

```python
s / 10	# value 값 전체에 10으로 나눔
rs = s - s2				# 인덱스가 매칭 안되는 값은 NaN
rs = (s-s2)/s * 100		# 사칙연산 다양하게 적용 가능
s.value - s2.values		# 값 결과값을 활용한 연산
```

> null 판별

```python
rs.notnull()			# null 인지 아닌지 False, True 로 Series 추출
rs[rs.notnull()]		# null 이 아닌(True) value 에 해당하는 Series 추출
```

### DataFrame

> 예시 데이터

```python
data = {
    "열0": ['aa', 'bb', 'cc'],
    "열1": [100, 200, 300],
    "열2": [400, 500, 600],
    "열3": [700, 800, 900],
    "열4": [1000, 1100, 1200],
    "열5": [0.01, 0.02, 0.03]
}
columns = ["열0", "열1", "열2", "열3", "열4", "열5"]
index = ["인덱스0", "인덱스1", "인덱스2"]
```

> 기본적인 생성법

```python
df = pd.DataFrame(data, index=index, columns=columns)			          
```

> 불러오기 규칙

df [  ] : **Series**를 불러내기 위한 명령어.
			**'열 이름'**  또는 **열 번호**만 넣을 수 있음.
			**'인덱스0' : '인덱스1' 으로 DataFrame 슬라이싱 가능**.
		    '열0', '열1' 식으로 이름 2개 **불가능**. 

df [[  ]] : **DataFrame**을 불러내기 위한 명령어
			   **'열 이름'** 또는 **열 번호**만 넣을 수 있음
			   **슬라이싱 불가능**
			   '열0', '열1' 식으로 이름 2개 **가능**. '인덱스 이름'은 어디든 넣을 순 없음.	

df.loc [  ] : '인덱스' 을 통해 **행을 Series**로 불러옴. 
			      '인덱스', '열' 을 통해 **교차되는 value** 값 불러옴.		
				  '인덱스0' : '인덱스1' 을 통해 **DataFrame 슬라이싱 가능**.

df.loc [[  ]] :  '인덱스' 을 통해 **DataFrame** 불러옴.
    				  '인덱스0', '인덱스1' **가능**
					  **슬라이싱 불가능**

> df [  ]

```python
# Series 불러오기
df[0]		 # 열 이름이 없을 경우만 가능
df['열0']	# '열0' 에 속하는 Series불러오기
df['열0'][0]
df['열0']['인덱스0']
df.열0		# .한글 가능
df.열0[0]
df.열0.인덱스0

# 값 조작
df['열4']['인덱스0'] = 200	# '열4'의 '인덱스0' 행 value 를 200으로 변경
df['열5'] *= 100  		  # '열5'의 values 갱신
df['열6'] = [1, 2, 3]      # '열6' 을 DataFrame끝에 추가
del df['열6']			  # '열6' 삭제

# 슬라이싱
df[1:3]		 	   			# 1번 인덱스부터 3-1 번 인덱스 행으로 이루어진 DataFrame 추출
df['인덱스0':'인덱스2']	 	# '인덱스0' 부터 '인덱스2' 인덱스 행으로 이루어진 DataFrame 추출
XXX df['인덱스0']

# 불가능한 명령어
XXX df['인덱스0']		         # '인덱스 이름' 입력 불가
XXX df['인덱스0', '인덱스1'] 	   # '인덱스 이름' 입력 불가, 두개 입력 불가
XXX df['인덱스0', '열0']		# '인덱스 이름' 입력 불가, 두개 입력 불가
XXX df['열0', '인덱스0']		# 두개 입력 불가, '인덱스 이름' 입력 불가
XXX df['열0', '열1']			 # 두개 입력 불가
XXX df.열6 = [1, 2, 3]		  # .한글로 추가 불가
```

> df [[  ]]

```python
# DataFrame 불러오기
df[['열0']]						# '열0' 로 이루어진 DataFrame 추출
df[['열0', '열4']]   			   # '열0', '열4' 로 이루어진 DataFrame 추출

# 불가능한 명령어
XXX df[['인덱스0']]
XXX df[['인덱스0', '열0']]
```

> df.loc [  ]

```python
# 값 추출
df.loc['인덱스0']			  	  # 인덱스0 행을 Series로 추출 
df.loc['인덱스0', '열1'] 		 # 인덱스0, 열1 에 교차되는 value 추출
df.loc['인덱스0':'인덱스2']		# 인덱스0에서 인덱스2까지 DataFrame 슬라이싱
XXX df.loc['인덱스0', '인덱스1']
XXX df.loc['열0']

df.loc[['인덱스0', '인덱스2']]	# 인덱스0, 인덱스2 행을 DataFrame 으로 추출
---------------------------------------------------------------------
# 불러오기
df.values		     # 모든 values를 array로 추출
df.index			 # 모든 인덱스 이름을 index로 추출
df.index.values		 # 모든 인덱스 이름을 array로 추출
df.columns		 	 # 모든 열 이름을 index로 추출
df.columns.values	 # 모든 열 이름을 array로 추출

df.head(2)	# 앞에서부터 2번째 행까지의 DataFrame 추출 (수치 미입력시 기본 5개)
df.tail(2)	# 뒤에서부터 2번째 행까지의 DataFrame 추출 (수치 미입력시 기본 5개)
```

> 이름 추가

```python
df.index.name = '도시'	# index 이름 추가
df.columns.name = '특성'	# columns 이름 추가
```

> 전치

```
df.T	# 전치
```

### CSV

> DataFrame을 csv 로 추출

```python
df.to_csv('sample.csv')
```

> csv 파일 출력 > 파이참에선 안됨

```python
%%writefile sample1.csv
c1, c2, c3
1, 1.11, one
2, 2.22, two
3, 3.33, three
```

> CSV 파일 불러오기

```python
pd.read_csv('sample.csv')
pd.read_csv('sample.csv', names=['c1', 'c2', 'c3'])		# 열 이름 정하기 (기존 열은 값으로 바뀜)
pd.read_csv('sample'.csv', header=None)					# 열을 넣지만 이름은 없음
```

### 고급 인덱싱

```python

```





























