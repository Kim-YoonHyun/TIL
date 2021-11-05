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

### 단순객체생성

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
mat <- matrix(1:10, nrow=2)		# 행의 갯수가 2개인 1~10까지의 행렬 생성
mat <- matrix(1:10, ncol=2)		# 열의 갯수가 2개인 1~10까지의 행렬 생성
mat <- matrix(1:10, ncol=2, byrow=TRUE)		# 행방향 기준 원소 배열
mat <- matrix(1:10, ncol=2, byrow=FALSE)	# 열방향 기준 원소 배열(default)
```

#### 열 & 행의 갯수 확인

```R
ncol(mat)	# 열 갯수 확인
nrow(mat)	# 행 갯수 확인
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



