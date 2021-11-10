# R basic

## 설치

[설치 사이트](https://ftp.harukasan.org/CRAN/)

사이트에 접속해서 Download R \<version> for Windows 통해 다운로드

설치경로에서 program file 의 경우 공백으로 인해 오류가 발생할 수 있으므로 C 바로 밑에 생성

## 활용 플랫폼

R Studio

## 기초 코드

### 연산자

#### 산술연산자

```R
+		# 더하기
-		# 빼기
*		# 곱하기
/		# 나누기
^		# 거듭제곱
**		# 거듭제곱
x%%y	# 나머지
x%/%y	# 몫
```

#### 논리연산자

```R
<			# 미만
<=			# 이하
>			# 초과
>=			# 이상
==			# 동일
!=			# 다름
!x			# 아님
x|y			# or
x&y			# and
isTRUE(x)	# 참 판단
```

### 객체생성법

```R
a <- 1
a = 1
a <<- 1
1 -> a
```

### 벡터

#### 벡터생성

````R
vec <- c(1, 2, 3)		# 벡터
vec <- c(1L, 2L, 3L)	# 정수형 벡터
vec <- c('a', 'b', 'c')	# 문자형 벡터
vec <- c(TRUE, FALSE, FALSE)	# 논리형 벡터
````

### 행렬

#### 행렬생성

```R
mat <- matrix(1:10, nrow=2)		# 행이 2개인 행렬 생성
mat <- matrix(1:10, ncol=2)		# 열이 2개인 행렬 생성
mat <- matrix(1:10, ncol=2, byrow=TRUE)		# 행방향 기준 원소 배열
mat <- matrix(1:10, ncol=2, byrow=FALSE)	# 열방향 기준 원소 배열(default)
```

#### 열 & 행의 갯수 확인

```R
ncol(mat)	# 열 갯수 확인
nrow(mat)	# 행 갯수 확인
```

### 배열

#### 배열생성

```R
ary <- array(1:30, dim=c(2, 3, 5))
```

### 리스트

#### 리스트생성

```R
x <- list(1:10, c('R', 'N'), c(TRUE, FALSE))
x <- list(list(1:10), c('R', 'N'), list(c(TRUE, FALSE)))	# 리스트 안에 리스트 가능
```

### 데이터프레임

#### 데이터프레임 생성

```R
df <- data.frame(col1=1:3, col2=c('R', 'N', 'S'), col3=c(TRUE, FALSE, FALSE))
```

### 요인

#### 요인 분석

```R
x <- c('male', 'female', 'female', 'male', 'bi-gender')
x_fac <- factor(x)
```

#### 요인 분석(level 표현 제외)

```R
unclass(x_fac)
```

### NA, NaN

#### 판별

```R
x <- c(NaN, NA)
is.NA(x)
is.NaN(x)
```

### 변환

#### 데이터프레임 -> 행렬

```R
mat <- as.matrix(df)	# 행렬로 강제변환. 단일속성만 허용
```

### 합계

```R
sum()
```

### 절댓값, 최댓값, 최솟값

```R
abs()
max()
min()
```

### 제곱근

```R
sqrt(x)
```

### 올림, 내림, 반올림

```R
ceiling(x)	# 올림
floor(x)	# 내림
trunc(x)	# 자름
round(x)			# 반올림
round(x, digits=n)	# n 자릿수 반올림
```

### 삼각함수

```R
cos(x)
sin(x)
tan(x)
acos(x)
cosh(x)
acosh(x)
```

### log 함수

```R
log(x)		# 자연로그
log10(x)	# 상용로그
```

### 지수함수

```R
exp(x)
```

### 함수 인수확인

```R
args(function)
```

### 자체 함수 생성

```R
function_name <- function(num){
    x <- num + 1
    return(x)
}
```

### 통계함수

#### (절사)평균

```R
mean(x, trim=0, na.rm=FALSE)
```

#### 표준편차

```R
sd(x)
```

#### 중위값

```R
median(x)
```

#### 분위값

```R
quantile(x, probs)
```



```R
dnorm(x)	# 정규밀도함수
pnorm(q)	# 누적정규확률
qnorm(p)	# 정규분위값
rnorm(n, m=0, sd=1)	# 정규분포 무작위 표본 추출
```

#### 이항분포

```R
dbinom(x, size, prob)
pbinom(q, size, prob)
qbinom(p, size, prob)
rbinom(n, size, prob)
```

#### 포아송분포

```R
dpois(x, lamda)
ppois(q, lamda)
qpois(p, lamda)
rpois(n, lamda)
```

#### 균일분포

```R
dunif(x, min=0, max=1)
punif(p, min=0, max=1)
qunif(p, min=0, max=1)
runif(n, min=0, max=1)
```

### 속성 부여

#### 이름 속성 부여

```R
x <- c(1, 2, 3, 4)
names(x) <- c('one', 'two', 'three', 'four')	# x에 이름 속성 부여
```

#### 차원 속성 부여

```R
dim(x) <- c(2, 2)		# x에 2*2 차원을 부여
```

#### 속성확인

```R
attributes(x)		# 속성확인
```

### 조건문

```R
if(논리){
    # 참 일경우
}
else{
    거짓일 경우
}
```

```R
y <- ifelse(논리, 참, 거짓)
```

```R
z <- ifelse(논리1, 참, ifelse(논리2, 참, ifelse(...)))
```

### 반복문

#### for 문

```R
for (i in data){
    code
}
```

#### while 문

```R
while(조건){
    code
}
```

#### repeat 문

```R
repeat{code}
```

#### 실행 중지

```R
break	# 반복문 종료
next	# 이하 생략(continue와 동일)
```



