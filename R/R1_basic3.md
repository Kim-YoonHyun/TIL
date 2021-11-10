# R basic

## 설치

[설치 사이트](https://ftp.harukasan.org/CRAN/)

사이트에 접속해서 Download R \<version> for Windows 통해 다운로드

설치경로에서 program file 의 경우 공백으로 인해 오류가 발생할 수 있으므로 C 바로 밑에 생성

## 활용 플랫폼

R Studio

## 기초 코드

### 변수

#### 변수에 값 할당

```R
a <- 1
a <<- 1
1 -> a
1 ->> a
a = 1
```

#### 변수 목록 확인

```R
ls()		# 변수명 확인
ls.str()	# 변수명, 할당값, 형식 확인
```

#### 변수 삭제

```R
rm(a)			# 변수 a 삭제
rm(list=ls())	# 모든 변수 삭제
```

### 벡터

#### 벡터 생성

```R
vec <- c(1, 2, 3)				# 벡터
vec <- c(1L, 2L, 3L)			# 정수형 벡터
vec <- c('a', 'b', 'c')			# 문자형 벡터
vec <- c(TRUE, FALSE, FALSE)	# 논리형 벡터
vec <- (1, 2, '3')	# 전부 문자형으로 통일됨. 벡터 원소들은 항상 같은 형식
```

#### 벡터 원소 선택

```R
vec[3]			# 3번째 원소
vec[c(1, 3)]	# 1, 3번째 원소
vec[-3]			# 3번째 원소를 제외하고 선택
vec[-c(1, 3)]	# 1, 3번째 원소를 제외하고 선택
```

#### 벡터 원소 추가

```R
vec <- c(vec, 4)
```

#### 벡터 원소 삽입

```R
vec <- append(vec, 25, after=2)
```

#### 벡터 요인생성

```R
f <- factor(vec)
```

#### 벡터 함수 적용

```R
sapply(vec, log)	# 함수 적용
```

#### 벡터 합치기

```R
stack(list(v1=vec1, v2=vec2, v3=vec3))
```





### 출력

#### 기본 출력

```R
print(a)
print(pi, digits=4)
```

#### 동시 출력

```R
cat(a, b, c) 	# 동시 출력, print(a, b, c)는 불가능
```

#### 출력 결과 외부 저장

```R
sink('example.txt')	# 기록 시작
x <- 1
y <- 2
x + y	# 결과를 기록
sink()	# 기록 종료(저장)
```





```R
cat("출력할 내용", 변수, "\n", file="파일이름", append=T)	# 파일에 출력하기
```

### 연산

#### 사칙연산

```R
+, -, *, /
```

#### 거듭제곱

```R
**, ^
```

#### 몫 & 나머지

```R
%/%
%%
```

#### 행렬의 곱

```R
%*%
```

#### 원소여부

```R
%in%	# 왼쪽의 원소가 오른쪽에 존재하는지 여부
vec <- c(1, 2, 3)
2 %in% vec
```

#### 함수적용

```R
%>%		# 파이프 연산자. 오른쪽 함수를 왼쪽에 적용
vec <- c(1, 2, 3, 4, 5)
vec %>% mean
```

#### 논리연산

```R
==		# 같다
!=		# 같지 않다
<, <=	# 작다, 보다 작다
>, >=	# 크다, 보다 크다
!		# 논리 부정
&		# and
|		# or
~		# formula
?		# 도움말. ?lm ?ls 등
TRUE	# T 로 표현 가능. 단 True 는 불가능
FALSE	# F 로 표현 가능. 단 False 는 불가능
```

#### 통계연산

```R
mean()		# 평균
sum()		# 합계
median()	# 중앙값
log()		# log
sd()		# 표준편차
var()		# 분산
cov(변수1, 변수2)	# 변수간 공분산
cor(변수1, 변수2)	# 변수간 상관계수
length(변수)		 # 변수간 길이 값
```

### 변수

#### 변수에 값 할당

```R
a = 1		# 변수 a 에 1을 할당
a <- 1		# 위와 동일
1 -> a		# 위와 동일
a <<- 1		# 전역변수 지정
a ->> 1		# 위와 동일
```

#### 변수 목록보기

```R
ls()		# 변수명 보기
ls.str()	# 변수명 및 할당된 값, 형식 보기
```

#### 변수 삭제하기

```R
rm(a)			# 변수 a 삭제
rm(list=ls())	# 모든 변수 삭제
```

### 벡터

#### 벡터 생성

```R
vec <- c(1, 2, 3, 4)	# 문자, 숫자, 논리값, 변수 모두 결합가능
c(1.1, 2.2, 3.3)
c('1', '2', '3')
c(x, y, z)
c(1, 2, '3')		# 하나라도 문자일 경우 모든 원소가 문자형태가 됨.
```

#### 벡터 원소 선택

```R
vec[3]			# 벡터에서 3번째 원소 불러옴
vec[-3]			# 벡터에서 3번째 원소 제외
vec[c(1, 3)]	# 벡터의 1, 3 번째 원소로 구성된 하위 벡터
vec[-c(1, 3)]	# 벡터의 1, 3 번째 원소를 제외하고 구성된 하위 벡터
names(vec) <- c('one', 'two', 'three', 'four')	# 벡터 원소에 이름 부여
vec$coef		# 요소, 슬롯 뽑아내기
```

#### 벡터에 데이터 추가

```R
vec <- c(vec, newdata)
```

#### 벡터에 데이터 삽입

```R
append(vec, newdata, after=n)
```

#### 벡터 요인 생성

```R
f <- factor(v)
f <- factor(v, level)
```

#### 벡터연산

```R
+, -, *, /, ^	# 기본 사칙연산
sapply(vec, log)	# 함수 적용
```

#### 벡터 저장

```R
write.csv(vec, 'text.csv')		# csv 로 저장
save(vec, file='text.Rdata')	# R 파일로 저장
```

#### 벡터 합치기

```R
comb <- stack(list(v1=vec1, v2=vec2, v3=vec3))
```

#### 함수 정의

```R
function(변수1, 변수2 ....변수n) {expr1, expr2 ....expr n}
# expr 의 특징
# 변수지정, 논리 실행 등 가능
```

#### 수열

```R
1:5		# 연속적인 숫자 생성. 1 2 3 4 5
seq(from=0, to=11, by=3)		# 0에서 11까지 3의 간격으로 숫자생성
seq(from=0, to=11, length.out=5)	# 0에서 11까지 5등분
```

#### 반복

```R
rep(1, time=5)		# 1을 5번 반복
rep(1:4, each=2)	# 1에서 4까지의 숫자를 각각 2번씩 반복
rep('aaa', each=2)	# 문자를 각각 2번씩 반복
```

### 리스트

#### 리스트 생성

```R
ls <- list(1, 'A', vec)	# 숫자, 문자, 함수를 포함 가능. 혼합 가능
```

#### 리스트 원소 선택

```R
ls[[1]]			# 리스트에서 1번째 원소 선택
ls[c(1, 3)]		# 리스트에서 1, 3 번째 원소로 구성된 하위 리스트
names(ls) <- c('one', 'A', 'vector')	# 이름 지정 가능
ls[['one']]		# 이름으로 원소 선택
ls&one			# 위와 동일
```

#### 리스트 원소 제거

```R
ls[['one']] <- NULL		# 이름으로 원소 제거
ls[sapply(ls, is.null)] <- NULL		# 리스트 내 NULL 원소 제거
ls[ls==o] <- NULL		# 위와 동일
ls[is.na(ls)] <- NULL	# 위와 동일
```

### 행렬

#### 행렬 생성

```R
mtx <- matrix(data, row, col)	# data로 이루어진 row X col 행렬 생성
ex_mtx1 <- matrix(1, 4, 4)		# 모든 원소가 1인 4 X 4 행렬
ex_mtx2 <- matrix(1:20, 5, 4)	# 1 ~ 20 까지의 숫자로 이루어진 5 x 4 행렬
```

#### 행렬 차원 확인

```R
dim(mtx)
```

#### 행렬 원소 선택

```R
diag(mtx)		# 대각선 원소 추출
vec <- mtx[1, ]	# 행 1 선택
vec <- mtx[, 3]	# 열 3 선택
rownames(mtx) <- c('rowname1', 'rowname2', 'rowname3')	# 행 이름 할당
colnames(mtx) <- c('colname1', 'colname2', 'colname3')	# 열 이름 할당
```

#### 행렬 전치

```R
t(mtx)
```

#### 역

```R
solve(mtx)		# 단 정사각행렬인 경우만 가능
```

#### 행렬 연산

```R
+, -, *, /		# 행렬 상수간 연산
%*% 			# 행렬간 곱
```

### 데이터프레임

#### 데이터프레임 생성

```R
df <- data.frame(vec1, vec2, vec3)		# 벡터로 데이터프레임 생성
df <- data.frame(1, 2, 3, 'a')			# 개별 값으로 데이터프레임 생성	
df <- data.frame(vec1, vec2, 1, 2)		# 벡터 및 개별 값
df <- as.data.frame(list(vec1, vec2))	# 열 변수로 데이터프레임 생성
df <- data.frame(열1이름=numeric(n), 열2이름=character(n))		# 데이터프레임 할당
```

#### 데이터프레임 결합

```R
rbind(df1, df2)		# 데이터프레임간 행결합. 행의 개수가 동일해야함
cbind(df1, df2)		# 데이터프레임간 열결합. 열의 개수와 이름이 동일해야함
merge(df1, d2, by='공통 열 이름')	# 공통변수로 병합
```

#### 단순 값 선택

```R
df[1]
df[[1]]
df[1,]
df[,1]
df[['name']]
df$name
df[c('name')]
colnames(변수)	# 변수 속성 조회
```

#### 조건 값 선택

```R
df[df$gender='m']	# 데이터프레임 내 성별이 남성
df[df$변수1>4 & df$변수2>5, c(변수3, 변수4)]	# 변수1과 변수2의 조건에 만족하는 레코드의 변수3, 4 조회
df[grep('문자', df$변수1, ignore.case=T), c('변수2, 변수3')]	# 변수1 내 '문자'가 들어있는 변수2, 3 값 조회
subset(df, select=변수, subset=변수>조건)		# 특정변수의 값이 조건에 맞는 변수셋 조회.
```

### 문자

#### 문자 길이

```R
nchar('string')
```

#### 문자 연결

```R
A <- paste('a', 'b', 'c', sep='-')		# "a-b-c"
paste(A, c('e', 'f'))					# "a-b-c e" "a-b-c f"
paste(A, 10, sep='')					# "a-b-c10"
```

#### 문자 추출

```R
substr('Bigdataanalysis', 1, 4)		# 문자열에서 1~4 위치까지 추출
strsplit('str,ing', ',')
```

### 파일

#### 파일 읽기

```R
list.files()		# 파일 목록 보기
list.files(recursive=T, all.files=T)	# 파일 목록보기 옵션 설정
\\ or / 	# R 에서는 경로 설정시 \\ 또는 / 로 해야한다
```

#### 파일 내보내기

```R
write.csv(행렬 또는 데이터프레임, '파일이름', row.names=F)	# csv 파일로 출력
write.csv(df, '파일이름', col.names=F)		# 1행이 변수명이 아닐 경우
write.csv(df, '파일이름', row.names=F)		# 레코드 번호를 생성하지 않을 경우
save(vec, file='test.Rdata')
```

#### 파일 불러오기

```R
load('test.R')
read.fwf('파일이름', widths=c(w1, w2, ..., wn))		# 고정자리수 데이터파일
read.table('파일이름', sep='구분자')	# 테이블로 된 데이터 파일 읽기
read.table('파일이름', sep='구분자', stringsASFactor=F)	# 주소, 이름 등의 텍스트를 요인으로 인식
read.table('파일이름', sep='구분자', na.strings='.')	# 결측치를 다른 문자열로 표현
read.table('파일이름', sep='구분자', header=T)		# 파일의 첫 행을 변수명으로 인식
read.csv('파일이름', header=T)	# csv 파일 읽기
read.csv('파일이름', header=T, as.is=T)		# 주소, 이름 등의 텍스트를 요인으로 인식
```

#### 웹에서 읽어올때

```R
read.csv('url/data.csv')
read.table('url/data.txt')
```

#### html에서 테이블 읽어 올 때

```R
library(XML)
url <- 'url'
t <- readHTMLTable(url)
```

### 데이터 구조

| 객체         | 예시                                         | 모드              |
| ------------ | -------------------------------------------- | ----------------- |
| 숫자         | 3.1415                                       | 수치형(numeric)   |
| 숫자벡터     | c(1, 3, 4, 6)                                | 수치형(numeric)   |
| 문자열       | 'Tom'                                        | 문자형(character) |
| 문자열 벡터  | c('Tom', 'Yoon', 'Kim')                      | 문자형(character) |
| 요인         | factor(c('A', 'B', 'C'))                     | 수치형(numeric)   |
| 리스트       | list('Tom', 'Yoon', 'Kim')                   | 리스트(list)      |
| 데이터프레임 | data.frame(x=1:3, y=c('Tom', 'Yoon', 'Kim')) | 리스트(list)      |
| 함수         | print                                        | 함수(function)    |

| 객체            | 예시                   | 설명                                        |
| --------------- | ---------------------- | ------------------------------------------- |
| 단일값(Scalars) | pi                     | R에서는 원소가 하나인 벡터로 인식           |
| 행렬(Matrix)    | `dim(a <- c(3, 3))`    | R에서는 차원을 가진 벡터로 인식             |
| 배열(Arrays)    | `dim(b) <- c(2, 3, 2)` | 행렬에 n 차원까지 확장된 형태               |
| 요인(Factors)   |                        | 벡터에 있는 고유값들을 요인의 수준이라고 함 |

### 구조 변경

기본적으로 앞에 `as.`를 붙인다

#### 벡터 ->

```R
as.list(vec) 	# 리스트로 변경
cbind(vec)			# 1열짜리 행렬
matrix(vec)			# 위와 동일
rbind(vec)			# 1행짜리 행렬
matrix(vec, n, m) 	# row X col 행렬
as.data.frame(vec)			# 1열짜리 데이터프레임
as.data.frame(rbind(vec))	# 1행짜리 데이터프레임
```

#### 리스트 ->

```R
unlist(lst)		# 벡터로 변경
as.matrix(lst)			# 1열짜리 행렬
as.matrix(rbind(lst))	# 1행짜리 행렬
as.matrix(lst, n, m)	# n X m 행렬
as.data.frame(lst)			# 데이터의 열로 변경
rbind(obs[[1]], obs[[2]])	# 데이터의 행으로 변경
```

#### 행렬 ->

```R
as.vector(mtx)	# 벡터로 변경
as.list(mtx)	# 리스트로 변경
as.data.frame()	# 데이터프레임으로 변경
```

#### 데이터프레임 ->

```R
df[[1]] 	# 데이터프레임 열1을 벡터로 변경
df[, 1]		# 위와 동일
df[1,]		# 데이터프레임 행1을 벡터로 변경
as.list(df)	# 리스트로 변경
as.matrix(df)	# 행렬로 변경
```

### 통계

#### 평균

```R
mean(data&column)	# 특정 column의 평균
```

#### 중앙값

```R
median(data$column)	# 특정 column의 중앙값
```

#### 표준편차

```R
sd(data$column)	# 특정 column의 표준편차
```

#### 분산

```R
var(data$column) # 특정 column의 분산
```

#### 공분산

```R
cov(x, y=NULL, use='everything', method=c('pearson', 'kendall', 'spearman'))
```

#### 분위수

```R
quantile(data$column) 	# 특정 column의 분위수
```

#### 상관관계

```R
cor(x, y=NULL, use='everything', method=c('pearson', 'kendall', 'spearman'))
```

### 기타

#### 워킹 디렉토리 지정

```R
setwd(path)		# 워킹 디렉토리 지정
```

#### 요약 관련

```R
data()		# R에 내장된 데이터셋 리스트 보여줌
summary(dataset)	# 데이터셋의 변수내용 요약
head(dataset)		# 처음 6개 레코드 조회
head(dataset, n)	# 처음 n개 레코드 조회
tail(dataset)		# 마지막 6개 레코드 조회
```

#### 패키지 관련

```R
install.packages('패키지이름')	# 패키지 설치
library('패키지')		# 패키지 불러오기
```

#### 종료

```R
q()
```





