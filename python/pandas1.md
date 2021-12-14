# 1. Pandas1

csv 파일의 읽고 쓰기.
시리즈와 데이터프레임 만들기.
데이터에 접근.

# 2. 명령어

## 2.1. Series

### 2.1.1. 기본적인 생성법

```python
s = pd.Series([100, 200, 300])	# values
s = pd.Series([100, 200, 300], index=['인덱스0', '인덱스1', '인덱스2'], dtype=np.int32)	# value, index
s2 = pd.Series({'인덱스0':300, '인덱스1':200, '인덱스2':100})	# dictionary (잘 쓰이진 않음)
```

### 2.1.2. index 불러오기

```python
s.index			# 모든 index를 index로 불러옴
s.index.values	# 모든 index를 array로 불러옴
```

### 2.1.3. value 불러오기

```python
s[0]		 # 0 번째 행의 value 추출
s['인덱스0']  # '인덱스0' 행의 value 추출
s.인덱스0	   # .한글 가능
s.values	 # 모든 values값을 array로 불러옴
```

### 2.1.4. 행 추가

```python
s['인덱스3'] = 400 	# '인덱스3' 이라는 새로운 행을 추가하고 400 할당.
```

### 2.1.5. 행 제거

```python
del s['인덱스3']		# '인덱스3' 행 제거
```

### 2.1.6. 하위 Series 추출

```python
s[[0]]				   		# 0 번째 행의 Series 추출
s[['인덱스0']]				  # '인덱스0' 행의 Series 추출
s[[0, 2]]			   		# 0, 2 번째 행의 Series 추출
s[['인덱스0', '인덱스2']]		# '인덱스0', '인덱스2' 행의 Series 추출

s[1:3]			   	  	# 0에서 3 - 1 까지 행의 Series 슬라이싱
s['인덱스1':'인덱스2']	# '인덱스1' 에서 '인덱스2' 까지 행의 Series 슬라이싱
### 슬라이싱 시 숫자로 할 경우 마지막 번호는 제외되지만 이름으로 할 경우 마지막도 포함됨.
```

### 2.1.7. 필터링

```python
s[(100 < s) & (s < 300)]	# 값 100 에서 300 사이 시리즈 필터링
s[(s < 100) | (s > 200)]	# 값 100 미만 그리고 200초과 시리즈 필터링
```

### 2.1.8. 행 & 열 이름 설정

```python
s.name = '인덱스 이름'			# index 이름 설정
s.index.name = '시리즈 이름'		# series 이름 설정
```

### 2.1.9. 연산

```python
s / 10	# value 값 전체에 10으로 나눔
rs = s - s2				# 인덱스가 매칭 안되는 값은 NaN
rs = (s-s2)/s * 100		# 사칙연산 다양하게 적용 가능
s.value - s2.values		# 값 결과값을 활용한 연산
```

### 2.1.10. null 판별

```python
rs.notnull()			# null 인지 아닌지 False, True 로 Series 추출
rs[rs.notnull()]		# null 이 아닌(True) value 에 해당하는 Series 추출
```

## 2.2. DataFrame

### 2.2.1. 예시 데이터

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

### 2.2.2. 기본적인 생성법

```python
df = pd.DataFrame(data, index=index, columns=columns)			       
```

### 2.2.3. Series, df 추출

```python
df.열0				# 선택한 열의 Series
df['열0']			# 선택한 열의 Series
df[['열0']]			# 선택한 열의 DataFrame
df['인덱스0':]		   # 지정한 인덱스(행) 범위의 DataFrame
```

### 2.2.4. 행, 열 이름으로 추출

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

### 2.2.5. 위치값으로 추출

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

#### 특이사항

- value 가 모이면 series 가 되고, series가 모이면 DataFrame 이 됨.
- `df['인덱스0':] == df.loc['인덱스0':] == df.iloc[0:]` 이므로 `df['열']` 로 열의 Series를 불러올 수 있는 것 외엔 df 명령어는 큰 의미가 없음.
- `df` 및 `df.loc` 에서 `['열':]` 식으로 범위지정 할 경우 에러가 나진 않지만 값이 제어가 안되서 사용안하는게 좋을듯.
- `df` 및 `df.loc` 에서 인덱스의 숫자 및 열의 숫자를 통해 불러올 수 있지만, `df.iloc`과 헷갈리므로, 이름이 지정되어있지 않은 이상 숫자로는 사용하지 않는게 좋을듯.

- `df.loc` 와 `df.iloc` 은 이름과 숫자의 차이만 있을 뿐 사실상 동일한 명령어

### 2.2.6. 값 불러오기

#### 2.2.6.1. values 불러오기

```python
# 불러오기
df.values		     # 모든 values를 array로 추출
df.index.values		 # 모든 인덱스 이름을 array로 추출
df.columns		 	 # 모든 열 이름을 index로 추출
df.columns.values	 # 모든 열 이름을 array로 추출
df.head(2)			 # 앞에서부터 2번째 행까지의 DataFrame 추출 (수치 미입력시 기본 5개)
df.tail(2)			 # 뒤에서부터 2번째 행까지의 DataFrame 추출 (수치 미입력시 기본 5개)
```

#### 갯수세기&정보표시

```python
s.count()	# Series의 value 갯수 (NaN 제외)
df.count()	# DataFrame의 각 인덱스(행)별 value 갯수 (NaN 제외)	
df.astype('int')	# int로 타입 변경
df_ns.columns = df_ns.columns.map(int)	# 모든 columns의 값을 int형으로 변경

df['열0'].value_counts()	    							# 선택한 열(Series)의 값 별 갯수.
df['열0'].value_counts().to_frame()	    				# 선택한 열(Series)의 값 별 갯수를 DataFrame으로.
df['열0'].value_counts().sort_values()					# 값 별 갯수를 오름차순으로 나열
df['열0'].value_counts().sort_values(ascending=False)	# 내림차순
df.index.value_counts()		# 각 인덱스가 몇개씩 있는지 카운트 
df.sort_values(by='열0')									# DataFrame을 '열0' 의 값 기준으로 오름차순 정렬

df.info()					# DataFrame의 각종 정보 표시. 열 이름, NaN 갯수, Dtype 등
df.describe()				# DataFrame의 각 열별 수치적 정보 표시. count, mean, std, min, max 등
df.duplicated()	# 중복데이터 확인
```

#### 결측치

```python
df.isnull()
df.notnull()				# DataFrame 에서 NaN 여부 (True or False)
df.fillna(11, inpace=True)	# DataFrame 에서 NaN 를 11로 대체
```

#### 시각화

```python
df['열'].plot(kind='bar')
```

#### 연산

```python
df.sum()		 # 모든 값 더하기(디폴트(axis=0)는 열 기준, axis=1: 인덱스 기준)
df['열1'].sum()  # 선택한 열 값의 합
df.mean()		 # 평균
df['열1'].mean()	# 열의 평균
df.shape		# df의 크기
```

#### 값 변경

```python
df[df['열0']=='aa']					   # 열0 에서 값이 'aa' 인 행으로만 DataFrame 재구성
df[df['열2'].notnull()]				   # 열2 기준 NaN가 존재하지 않는 인덱스(행)으로 DataFrame 재구성 
df.apply(lambda x: x.max() - x.min())	# apply 변환
df['열0'] = df['열0'].replace(df['열0'][0], 'aaa')		# 열0 의 0번째 값을 aaa 로 변경
df['열0'][0] = 'aaa'		# 변경가능한데 경고가 나옴

sb = df[df.['열'].str.contains('aa|AA', case=False)]	# 열의 str값에서 aa, AA 가 포함된 값으로 재구성
```

#### index 관련

```python
df.index						    # 모든 인덱스 이름을 index로 추출
df.index.shape						# 인덱스 사이즈값 추출
df.index.unique()					# 겹치지 않는 고유 인덱스 이름 추출
df.reset_index()					# 기존의 인덱스를 열의 값으로 바꾸고 디폴트 인덱스를 추가
df.set_index('열0', inplace=True)    # 열0 의 값을 인덱스 이름으로 변경. inplace 는 곧바로 적용을 의미함
```

#### 삭제하기

```python
df.drop(index='인덱스0', inplace=True)	# 인덱스0 의 행을 지움
df.drop(columns='열0', inplace=True)	# 열0 을 지움
df.drop(index=df.index[0])	# 0번째 행을 지움
df.dropna()
df.drop_duplicates()	# 중복데이터 삭제
```

#### 이름 관련

```python
df.index.name = '도시'	# index 이름 추가
df.columns.name = '특성'	# columns 이름 추가
df.rename(columns = {'old_nm' : 'new_nm'}, inplace = True)
df.rename(index = {'old_nm': 'new_nm'}, inplace = True)
```

#### 전치

```python
df.T	# 전치
```

#### 공백제거

```python
df['열0'].str.strip()	# 열0 의 양쪽 공백 제거
df['열0'].str.lstrip()	# 열0 의 왼쪽 공백 제거
df['열0'].str.rstrip()	# 열0 의 오른쪽 공백 제거
df.apply(lambda x: x.str.strip(), axis=1)	# 데이터프레임 전체에 양쪽 공백 제거 적용
```

#### merge

```python
pd.merge(df1, df2)					# 서로 겹치는 항목끼리 합침
pd.merge(df1, df2, how='left')		# 왼쪽(df1)을 기준으로 합침
pd.merge(df1, df2, how='right')		# 오른쪽(df2)을 기준으로 합칩
pd.merge(df1, df2, how='outer')		# 겹치는 항목에 상관없이 양쪽 데이터를 전부 합침
pd.merge(df1, df2, on='열 이름')	  # '열 이름' 에 따라 양쪽 데이터를 합침
pd.merge(df1, df2, left_on='열 이름1', right_on='열이름2')   # 열이름 1, 2 각각에 데이터를 합침
pd.merge(df1, df2, left_on=['열 이름1, 열 이름2'], right_index=True)	# 왼쪽 열이름1, 열이름2 를 기준으로 오른쪽의 값만 합친다.
df1.join(df2, how='outer')	# join을 쓰려면 key 값이 서로 같아야한다.
```

#### concat

```python
pd.concat([s1, s2])		# s1과 s2 를 연결
pd.concat([df1, df2], axis=1)	# df1과 df2를 axis=1 기준으로 연결
```

### CSV

#### DataFrame을 csv 로 추출

```python
df.to_csv('sample.csv', encoding='utf-8-sig')  
# encoding 없으면 글자 깨짐
```

#### csv 파일 출력 > 파이참에선 안됨

```python
%%writefile sample1.csv
c1, c2, c3
1, 1.11, one
2, 2.22, two
3, 3.33, three
```

#### CSV 파일 불러오기

```python
pd.read_csv('sample.csv')
pd.read_csv('sample.csv', names=['c1', 'c2', 'c3'])		# 열 이름 정하기 (기존 열은 값으로 바뀜)
pd.read_csv('sample'.csv', header=None)					# 열을 넣지만 이름은 없음
```




























