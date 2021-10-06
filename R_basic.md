# R basic

## 설치

[설치 사이트](https://ftp.harukasan.org/CRAN/)

사이트에 접속해서 Download R \<version> for Windows 통해 다운로드

설치경로에서 program file 의 경우 공백으로 인해 오류가 발생할 수 있으므로 C 바로 밑에 생성

## 활용 플랫폼

R Studio

## 기초 코드

### 출력

```R
print(a)			# 객체 출력
print(pi, digits=7)	# 7자리로 출력
cat(a, b, c)		# 여러 객체 출력 (list, 행렬 불가능)
cat(format(pi, digits=7), '\n')
options(digits=7)		# 기본적으로 7자리로 출력 설정
cat("출력할 내용", 변수, "\n", file="파일이름", append=T)	# 파일에 출력하기
sink('파일이름')
...출력할 내용...
sink()
```

### 연산

#### 사칙연산

```R
+, -, *, /	# 기초 연산자
%/%			# 몫
%%			# 나머지
%*%			# 행렬의 곱
5^2			# 지수. 25
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
vec <- c(1, 2, 3, 4)			# 문자, 숫자, 논리값, 변수 모두 결합가능
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
names(vec) <- c('one', 'two', 'three', 'four')	# 벡터 원소에 이름 부여
vec$coef		# 요소, 슬롯 뽑아내기
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

### 문자

#### 문자 붙이기

```R
A <- paste('a', 'b', 'c', sep='-')		# "a-b-c"
paste(A, c('e', 'f'))					# "a-b-c e" "a-b-c f"
paste(A, 10, sep='')					# "a-b-c10"
```

#### 문자 추출

```R
substr('Bigdataanalysis', 1, 4)		# 문자열에서 1~4 위치까지 추출
```

### 파일

#### 파일 읽기

```R
list.files()		# 파일 목록 보기
list.files(recursive=T, all.files=T)	# 파일 목록보기 옵션 설정
\\ or / 	# R 에서는 경로 설정시 \\ 또는 / 로 해야한다
read.fwf('파일이름', widths=c(w1, w2, ..., wn))		# 고정자리수 데이터파일
read.table('파일이름', sep='구분자')	# 테이블로 된 데이터 파일 읽기
read.table('파일이름', sep='구분자', stringsASFactor=F)	# 주소, 이름 등의 텍스트를 요인으로 인식
read.table('파일이름', sep='구분자', na.strings='.')	# 결측치를 다른 문자열로 표현
read.table('파일이름', sep='구분자', header=T)		# 파일의 첫 행을 변수명으로 인식
read.csv('파일이름', header=T)	# csv 파일 읽기
read.csv('파일이름', header=T, as.is=T)		# 주소, 이름 등의 텍스트를 요인으로 인식
```

#### 파일 내보내기

```R
write.csv(행렬 또는 데이터프레임, '파일이름', row.names=F)	# csv 파일로 출력
write.csv(df, '파일이름', col.names=F)		# 1행이 변수명이 아닐 경우
write.csv(df, '파일이름', row.names=F)		# 레코드 번호를 생성하지 않을 경우
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

## 데이터 구조

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

### 데이터프레임

#### 데이터프레임 원소 접근방법

```R
b[1] ; b['empno']
b[[1]] ; b[['empno']]
b&empno
```



