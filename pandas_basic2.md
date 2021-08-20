# Pandas basic 2

## DataFrame

> 예시 데이터1

```python
age = [0, 2, 10, 21, 23, 37, 31, 61, 20, 41, 32, 101]
bins = [1, 20, 30, 50, 70, 100]
labels = ['미성년자', '청년', '중년', '장년', '노년']
```

> 카테고리 변환

```python
catego = pd.cut(ages, bins, labels=labels)	# 실수값 -> 카테고리
```

> 예시 데이터2

```python
np.random.seed(2021)
data = np.random.randn(1000)
```

> data 등분

```python
cats = pd.qcut(data, 4, labels=['q1', 'q2', 'q3', 'q4']) 
```

--------

> 예시 데이터3

```python
data = {
    "도시": ["서울", "서울", "서울", "부산", "부산", "부산", "인천", "인천"],
    "연도": ["2015", "2010", "2005", "2015", "2010", "2005", "2015", "2010"],
    "인구": [9904312, 9631482, 9762546, 3448737, 3393191, 3512547, 2890451, 263203],
    "지역": ["수도권", "수도권", "수도권", "경상권", "경상권", "경상권", "수도권", "수도권"]
}
columns = ["도시", "연도", "인구", "지역"]
df1 = pd.DataFrame(data, columns=columns)
```

> pivot

```python
df1.pivot('도시', '연도', '인구')				# (인덱스, 열, value) 순서
df1.pivot(['지역', '도시'], '연도', '인구')
```

> pivot_table

```python
df1.pivot_table('인구', '도시', '연도')		# (value, 인덱스, 열) 순서
df1.pivot_table('인구', '도시', '연도', margins=True, margins_name='합계')	 # 마지막에 '합계' 열 추가
df1.pivot_table('인구', ['지역', '도시'], '연도', margins=True, margins_name='합계')
```

> groupby

```python
df.groupby(df['열 이름']).<적용할 연산>		# '열이름' 을 기준으로 나머지 열에 <적용할 연산> 으로 값을 구성
										 # mean(), std(), sum(), count() 등 
df.groupby(df['열 이름1', '열 이름2']).<적용할 연산>

df.groupby(df['열 이름1'])['열 이름2'].sum()	# 열 이름2 에만 연산 적용
df.groupby(df['열 이름']).agg(['mean', 'std'])		# agg를 통해 복수 연산 적용가능
df.groupby(df['열 이름']).agg(<함수>)			   # 함수 (lambda 등) 적용 가능
```











