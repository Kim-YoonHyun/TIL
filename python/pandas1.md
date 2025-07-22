# 	1. 개요

csv 파일의 읽고 쓰기.
시리즈와 데이터프레임 만들기.
데이터에 접근.

## 1.1. install

```bash
$ pip install pandas
```

## 1.2. import

```python
import pandas as pd
```





# 2. Series

## 2.2. 생성

### 2.2.1. 기본 생성

```python
s = pd.Series([100, 200, 300])
```

#### option - index

```python
s = pd.Series([100, 200, 300], index=['인덱스0', '인덱스1', '인덱스2'])
```

#### option - dtype

```python
import numpy as np
s = pd.Series([100, 200, 300], dtype=np.int32)
```

### 2.2.2. dict 활용

```python
s = pd.Series({'인덱스0':100, '인덱스1':200, '인덱스2':300})
```

## 2.3. 추출

### 2.3.1. 값 추출

```python
s[0]		 # 0 번째 행의 value 추출
s['인덱스0']  # '인덱스0' 행의 value 추출
s.인덱스0	   # .한글 가능
```

### 2.3.2. 전체 값 추출

```python
s.values	 # 모든 values값을 array로 불러옴
```

### 2.3.3. index 추출

```python
s.index			# 모든 index를 index로 불러옴
s.index.values	# 모든 index를 array로 불러옴
```

### 2.3.4. 슬라이싱

- 슬라이싱의 경우 숫자는 지정값 - 1 까지만 추출 됨

```python
s[[0]]				   		# 0 번째 행의 Series 추출
s[['인덱스0']]				  # '인덱스0' 행의 Series 추출
s[[0, 2]]			   		# 0, 2 번째 행의 Series 추출
s[['인덱스0', '인덱스2']]		# '인덱스0', '인덱스2' 행의 Series 추출
s[1:3]			   	  	# 0 ~ 2 
s['인덱스1':'인덱스2']	# '인덱스1' ~ '인덱스2' 

```

## 2.4. 추가 & 제거

### 2.4.1. 행 추가

```python
s['인덱스3'] = 400 	# '인덱스3' 이라는 새로운 행을 추가하고 400 할당.
```

### 2.4.2. 행 제거

```python
del s['인덱스3']		# '인덱스3' 행 제거
```

## 2.5.  재구성

### 2.5.1. 값 필터링

```python
s[(100 < s) & (s < 300)]	# 값 100 에서 300 사이 시리즈 필터링
s[(s < 100) | (s > 200)]	# 값 100 미만 그리고 200초과 시리즈 필터링
```

### 2.5.2. name 설정

```python
s.name = '인덱스 이름'			# index 이름 설정
s.index.name = '시리즈 이름'		# series 이름 설정
```

### 2.5.3. 결측치 

```python
s.notnull()			# null 인지 아닌지 False, True 로 Series 추출
s[s.notnull()]		# null 이 아닌(True) value 에 해당하는 Series 추출
```

## 2.5. 갯수 세기

```python
s.count()	# Series의 value 갯수 (NaN 제외)
```



## 2.6. 연산

```python
s / 10	# value 값 전체에 10으로 나눔
rs = s - s2				# 인덱스가 매칭 안되는 값은 NaN
rs = (s-s2)/s * 100		# 사칙연산 다양하게 적용 가능
s.value - s2.values		# 값 결과값을 활용한 연산
```

# 3. DataFrame

## 3.1. 생성

### 3.1.1. 기본

```python
df = pd.DataFrame([100, 200, 300])
```

#### option - index

```python
df = pd.DataFrame([100, 200, 300],
                  index=['인덱스0', '인덱스1', '인덱스2'])
```

#### option - columns

```python
df = pd.DataFrame([100, 200, 300],
                  columns=['열0'])
```

### 3.1.2. list 활용

``` python
data = [
    ['aa', 'bb', 'cc'],
    [100, 200, 300],
    [1000, 1100, 1200],
    [0.01, 0.02, 0.03]
]
index_name = ["인덱스0", "인덱스1", "인덱스2", "인덱스3"]
columns_name = ["열0", "열1", "열2"]
df = pd.DataFrame(data, 
                  index=index_name,
                  columns=columns_name)
```

```python
'''
        열0    열1    열2
인덱스0    aa    bb    cc
인덱스1   100   200   300
인덱스2  1000  1100  1200
인덱스3  0.01  0.02  0.03
'''
```

### 3.1.3. dict 활용

```python
data = {
    "열0": ['aa', 'bb', 'cc'],
    "열1": [100, 200, 300],
    "열2": [400, 500, 600],
    "열3": [700, 800, 900],
    "열4": [1000, 1100, 1200],
    "열5": [0.01, 0.02, 0.03]
}
index_name = ["인덱스0", "인덱스1", "인덱스2"]
df = pd.DataFrame(data, index=index_name)
```

```python
'''
      열0   열1   열2   열3    열4    열5
인덱스0  aa  100  400  700  1000  0.01
인덱스1  bb  200  500  800  1100  0.02
인덱스2  cc  300  600  900  1200  0.03
'''
```

## 값

### .1. 필터링

```python
a = [
     [1, 2, 3, 4, 5],
     [6, 7, 8, 9, 10],
     [11, 12, 13, 14, 15],
     [16, 17, 18, 19, 20]
 ]
df = pd.DataFrame(a, index=['a', 'b', 'c', 'd'], columns=['one', 'two', 'three', 'four', 'five'])
df['one'][df['one'] > 10] = 123
print(df)

a = (df['one'] <= 6)
b = (df['one'] >= 16)
c = a | b
df['one'][c] = 123123
print(df)
```



## 3.2. 추출

### 3.2.1. 값 추출

#### df.loc

```python
df.loc['인덱스0']			 		 # 선택한 인덱스(행)의 Series
df.loc['인덱스0', '열1']			# 선택한 인덱스(행)와 선택한 열의 교차 value
df.loc['인덱스0', ['열1', '열2']]    # 선택한 인덱스(행)에서 선택한 열의 값들의 Series
df.loc['인덱스0', '열1':]			# 선택한 인덱스(행)에서 지정한 열 범위의 Series

df.loc[['인덱스0']]			 	 # 선택한 인덱스(행)의 DataFrame	 
df.loc[['인덱스0'], '열1']			# 선택한 인덱스(행)에서 선택한 열의 Series
df.loc[['인덱스0'], ['열1', '열2']] # 선택한 인덱스(행)에서 선택한 열들의 DataFrame     
df.loc[['인덱스0'], '열1':]			# 선택한 인덱스(행)에서 지정한 열 범위의 DataFrame

df.loc[['인덱스0', '인덱스1']]					# 선택한 인덱스(행)들의 DataFrame 
df.loc[['인덱스0', '인덱스1'], '열1']			   # 선택한 인덱스(행)들에서 선택한 열의 Series
df.loc[['인덱스0', '인덱스1'], ['열1', '열2']] 	  # 선택한 인덱스(행)들에서 선택한 열들의 DataFrame   
df.loc[['인덱스0', '인덱스1'], '열1':]			   # 선택한 인덱스(행)에서 지정한 열 범위의 DataFrame

df.loc['인덱스0':]					 # 지정한 인덱스(행) 범위의 DataFrame
df.loc['인덱스0':, '열1']			# 지정한 인덱스(행) 범위에서 선택한 열의 Series
df.loc['인덱스0':, ['열1', '열2']]  # 지정한 인덱스(행) 범위에서 선택한 열들의 DataFrame
df.loc['인덱스0':, '열1':]			# 지정한 인덱스(행) 범위에서 지정한 열 범위의 DataFrame
```

#### df.iloc

```python
df.iloc[0]			  # 선택한 인덱스(행0)의 Series
df.iloc[0, 1]		  # 선택한 인덱스(행0)와 선택한 열(1) 의 교차 value
df.iloc[0, [1, 2]]    # 선택한 인덱스(행0)와 선택한 열들(1, 2)의 Series
df.iloc[0, 1:]		  # 선택한 인덱스(행0)와 지정한 열(1:) 범위의 Series

df.iloc[[0]]		  # 선택한 인덱스(행0)의 DataFrame
df.iloc[[0], 1]		  # 선택한 인덱스(행0)에서 선택한 열의 Serise
df.iloc[[0], [1, 2]]  # 선택한 인덱스(행0)에서 선택한 열들의 DataFrame
df.iloc[[0], 1:]	  # 선택한 인덱스(행0)에서 지정한 열 범위의 DataFrame

df.iloc[[0]]		  # 선택한 인덱스(행0)의 DataFrame
df.iloc[[0], 1]		  # 선택한 인덱스(행0)에서 선택한 열의 Serise
df.iloc[[0], [1, 2]]  # 선택한 인덱스(행0)에서 선택한 열들의 DataFrame
df.iloc[[0], 1:]	  # 선택한 인덱스(행0)에서 지정한 열 범위의 DataFrame

df.iloc[0:]			  # 지정한 인덱스(행0:) 범위의 DataFrame
df.iloc[0:, 1]		  # 지정한 인덱스(행0:) 범위에서 선택한 열(1)의 Series
df.iloc[0:, [1, 2]]   # 지정한 인덱스(행0:) 범위에서 선택한 열들(1, 2)의 DataFrame
df.iloc[0:, 1:]		  # 지정한 인덱스(행0:) 범위에서 지정한 열(1) 범위의 DataFrame
```

- value 가 모이면 series 가 되고, series가 모이면 DataFrame 이 됨.
- `df['인덱스0':] == df.loc['인덱스0':] == df.iloc[0:]` 이므로 `df['열']` 로 열의 Series를 불러올 수 있는 것 외엔 df 명령어는 큰 의미가 없음.
- `df` 및 `df.loc` 에서 `['열':]` 식으로 범위지정 할 경우 에러가 나진 않지만 값이 제어가 안되서 사용안하는게 좋을듯.
- `df` 및 `df.loc` 에서 인덱스의 숫자 및 열의 숫자를 통해 불러올 수 있지만, `df.iloc`과 헷갈리므로, 이름이 지정되어있지 않은 이상 숫자로는 사용하지 않는게 좋을듯.
- `df.loc` 와 `df.iloc` 은 이름과 숫자의 차이만 있을 뿐 사실상 동일한 명령어

#### 모든 값 추출

```python
df.values	# 모든 values를 array로 추출
```

#### 랜덤 샘플링

```python
df_shuffled = df.sample(n=5)	# 5개 행 랜덤추출
df_shuffled = df.sample(frac=0.5)	# 전체 행의 50 % 랜덤추출
```



### 3.2.2. index 추출

```python
df.index			 # 모든 인덱스 이름을 index로 추출
df.index.values		 # 모든 인덱스 이름을 array로 추출
df.index.unique()	 # 겹치지 않는 고유 인덱스 이름 추출
df.index.shape		 # 인덱스 사이즈값 추출
```

### 3.2.3. columns 추출

```python
df.columns		 	 # 모든 열 이름을 index로 추출
df.columns.values	 # 모든 열 이름을 array로 추출
```

### 3.2.4. 슬라이싱

#### df 기반

```python
df.열0				# 선택한 열의 Series
df['열0']			# 선택한 열의 Series
df[['열0']]			# 선택한 열의 DataFrame
df['인덱스0':]		   # 지정한 인덱스(행) 범위의 DataFrame
```

#### 앞 5개

- 5는 기본값이며 설정해서 변경 가능

```python
df.head(2)			 # 앞에서부터 2번째 행까지의 값 (default = 5)
```

#### 뒤 5개

- 5는 기본값이며 설정해서 변경 가능

```python
df.tail(2)			 # 뒤에서부터 2번째 행까지의 값 (default = 5)
```

### 3.2.5. 조건부

#### 논리기반 재구성

```python
df[df['열0']=='aa']	   # 열0 == 'aa' 인 행으로 df 재구성
```

```python
sb = df[df.['열'].str.contains('aa|AA', case=False)]	
# 열의 str값에서 aa, AA 가 포함된 값으로 재구성
```

#### 결측치 기반 재구성

```python
df[df['열0'].notnull()]     # 열0 기준 NaN가 존재하지 않는 행으로 df 재구성 
df.fillna(11, inpace=True)	# DataFrame 에서 NaN 를 11로 대체
```

#### 값 변경

```python
df['열0'][0] = 'aaa'		# 변경가능한데 경고가 나옴
df['열0'] = df['열0'].replace(df['열0'][0], 'aaa')	   # 열0 의 0번째 값을 aaa 로 변경
```

#### lambda 적용

```python
def temp(x):
    return x

df.apply(lambda x: x.max() - x.min())	# apply 변환
df['column'] = df['column'].apply(lambda x: temp(x))	# apply 변환
```

### 3.2.6. 행 순서대로 추출 

```python
for idx, row in df.iterrows():
    print(row)
    
# tqdm 적용
for idx, row in tdqm(df.iterrows(), total=len(df)):
    pass
```



## 3.3. 결측치

### 3.3.1. 결측치 여부 파악

```python
df.isnull()
df.notnull()				# DataFrame 에서 NaN 여부 (True or False)
df[df['열0'].notnull()]		# 열 0 이 NaN 이 없는 경우만 추출
df[(df['열0'].isnull()) | (df['열1'].isnull())] # 열 0 또는 열 1 이 NaN 이 아닌 경우만 추출
```

### 3.3.2. 갯수 파악

```python
df.isnull().sum()
```

### 3.3.3. 삭제하기

```python
df = df.dropna()
```

#### option - axis

```python
df = df.dropna(axis=0)	# 행 기준, default
df = df.dropna(axis=1)	# 열 기준
```

#### option - subset

```python
df = df.dropna(subset=['열0'])	# 열0 기준 결측치 제거
df = df.dropna(subset=['열0', '열2'])	# 열0, 2 기준 결측치 제거
```

### 3.3.4. 대체하기

```python
df.fillna(0)		# 결측치를 0 으로 대체
```

#### option - method

```python
fillna(method='ffill' or 'pad')		# 윗값 복사
fillna(method='bfill' or 'backfill')# 아랫값 복사
```

## 3.4. index 설정

### 3.4.1. 이름 추가

```python
df.index.name = '도시'	# index 이름 추가
```

### 3.4.2. 이름 변경

```python
df.rename(index={'<기존 이름>': '<변경할 이름>'}, inplace = True)
```

### 3.4.3. 리셋

- 기존의 인덱스는 열로 바뀜

```python
df.reset_index()
```

#### option - drop

- 기존의 인덱스를 열에 추가하지 않고 없애는 여부. default = False

```python
df = df.reset_index(drop=True)
```

### 3.4.4. columns -> index

```python
df.set_index('열0', inplace=True)   # 열0 의 값들을 인덱스로 변경.
```

### 3.4.5. 셔플

- 본질적으로는 샘플링이지만 셔플과 같은 효과

```python
df_shuffled = df.sample(frac=1, random_state=42).reset_index(drop=True)
# reset_index(drop=True): index 초기화
```

## 3.5. column 설정

### 3.5.1. 열 추가

```python
df['새로운 열'] = list
df.insert(3, '새로운 열', list, True)
```

### 3.5.2. 이름 추가

```python
df.columns.name = '특성'	# columns 이름 추가
```

### 3.5.3. 이름 변경

```python
df.rename(columns = {'old_nm' : 'new_nm'}, inplace = True)
```

## 3.5. 삭제

### 3.5.1. index 삭제

```python
df.drop(index='인덱스0', inplace=True)	# 인덱스0 의 행을 지움
df.drop(index=df.index[0])	# 0번째 행을 지움
```

### 3.5.2. column 삭제

```python
df.drop(columns='열0', inplace=True)	# 열0 을 지움
```

### 3.5.3. 결측치 삭제

```python
df.dropna(subset=['열 이름'])
```

### 3.5.4. 중복 삭제

```python
df.drop_duplicates()	# 모든 열 기준으로 제거
df.drop_duplicates(['열 이름'])	# 열 이름 기준으로 제거
```

#### option - keep

```python
df.drop_duplicates(['열 이름'], keep='first')	# 첫 번째만 남기기
df.drop_duplicates(['열 이름'], keep='last')	# 마지막만 남기기
df.drop_duplicates(['열 이름'], keep=False)	# 모두 제거
```

#### option - ignore_index

```python
df.drop_duplicates(['열 이름'], ignore_index=True)	# 인덱스 재설정
```

### 3.5.5. 공백 제거

```python
df['열0'].str.strip()	# 열0 의 양쪽 공백 제거
df['열0'].str.lstrip()	# 열0 의 왼쪽 공백 제거
df['열0'].str.rstrip()	# 열0 의 오른쪽 공백 제거
df.apply(lambda x: x.str.strip(), axis=1)	# 데이터프레임 전체에 양쪽 공백 제거 적용
```

### 3.5.6. 특정 단어 포함 행 제거

```python
drop_index = df[df['열 이름'].str.contains('단어')].index
df.drop(drop_index, inplace=True)
```



## 3.6. 갯수 세기

### 3.6.1. 전체 기준

```ㅔ
df.count()	# DataFrame의 각 인덱스(행)별 value 갯수 (NaN 제외)
```

### 3.6.2. index 기준

```python
df.index.value_counts()		# 각 인덱스가 몇개씩 있는지 카운트 
```

### 3.6.3. column 기준

```python
df['열0'].value_counts()	    			# 선택한 열(Series)의 값 별 갯수.
df['열0'].value_counts().to_frame()	 	# 선택한 열(Series)의 값 별 갯수를 DataFrame으로.
df['열0'].value_counts().sort_values()					# 오름차순
df['열0'].value_counts().sort_values(ascending=False)	# 내림차순
```

### 3.6.4. 중복 확인

```python
df.duplicated()	# 중복데이터 확인
```

## 3.7. 정보 표시

### 3.7.1. 기본 정보

```python
df.info()		# DataFrame의 각종 정보 표시. 열 이름, NaN 갯수, Dtype 등
```

### 3.7.2. 수치적 정보

```python
df.describe()	# DataFrame의 각 열별 수치적 정보 표시. count, mean, std, min, max 등
```

## 3.8. 전환

### 3.8.1. 전치

```python
df.T
```

### 3.8.2. 형변환

```python
df.astype('int')	# int로 타입 변경
df_ns.columns = df_ns.columns.map(int)	# 모든 columns의 값을 int형으로 변경
```

### 3.8.3. s -> df

```python
pd.DataFrame(s, column=['열 이름'])
```

### 3.8.4. df -> numpy

```python
df.to_numpy()
```



## 3.9 정렬

### 3.9.1. 열 기준

```python
df.sort_values(by='열0')     # DataFrame을 '열0' 의 값 기준으로 오름차순 정렬
```

#### option - ascending

```python
df.sort_values(by='열0', ascending=False) # 내림차순 (default=True, 오름차순)
```

## 3.9. 합치기

### 3.9.1. merge

```python
pd.merge(df1, df2)					# 서로 겹치는 항목끼리 합침
```

#### option - how

```python
pd.merge(df1, df2, how='left')		# 왼쪽(df1)을 기준으로 합침
pd.merge(df1, df2, how='right')		# 오른쪽(df2)을 기준으로 합칩
pd.merge(df1, df2, how='outer')		# 겹치는 항목에 상관없이 양쪽 데이터를 전부 합침
```

#### option - on

```python
pd.merge(df1, df2, on='열 이름')	  # '열 이름' 에 따라 양쪽 데이터를 합침
```

#### option - left_on

```python
pd.merge(df1, df2, left_on='열 이름1', right_on='열이름2')   # 열이름 1, 2 각각에 데이터를 합침
pd.merge(df1, df2, left_on=['열 이름1, 열 이름2'], right_index=True)	
# 왼쪽 열이름1, 열이름2 를 기준으로 오른쪽의 값만 합친다. 
```

### 3.9.2. join

- join을 쓰려면 key 값이 서로 같아야한다.

```python
df1.join(df2)
```

#### option - how

```python
df1.join(df2, how='outer')
```

### 3.9.3. concat

```python
pd.concat([s1, s2])		# s1과 s2 를 연결
pd.concat([df1, df2])	# df1과 df2를 연결
```

#### option - axis

```python
pd.concat([df1, df2], axis=1)	# df1과 df2를 axis=1 기준으로 연결
```

## 3.10. CSV

### 3.10.1. 저장

- 한글 포함시 encoding 필요

```python
df.to_csv('<path>/<filename>.csv', encoding='utf-8-sig')  
# encoding 없으면 한글 깨짐
```

#### option - index

```python
df.to_csv('<path>/<filename>.csv', encoding='utf-8-sig', index=False)  
```

### 3.10.2. 불러오기

```python
pd.read_csv('<path>/<filename>.csv')
df.read_csv('<path>/<filename>.csv', encoding='utf-8-sig')
```

#### option - name

```python
pd.read_csv('sample.csv', names=['c1', 'c2', 'c3'])		
# 열 이름 정하기 (기존 열은 값으로 바뀜)
```

#### option - header

```python
pd.read_csv('sample.csv', header=None)
# 열을 넣지만 이름은 없음
```

#### option - index_col

```python
pd.read_csv('sample.csv', index_col=0)
# Unnamed:0 컬럼 없애기
```



## 3.11. 시각화

```python
df['열'].plot(kind='bar')
```

## 3.12. 연산

```python
df.sum()		 # 모든 값 더하기(디폴트(axis=0)는 열 기준, axis=1: 인덱스 기준)
df['열1'].sum()  # 선택한 열 값의 합
df.mean()		 # 평균
df['열1'].mean()	# 열의 평균
df.shape		# df의 크기
```































