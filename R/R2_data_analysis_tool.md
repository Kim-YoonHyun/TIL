# Data analysis tools

## R reshape

데이터 마트 개발

### 설치

```R
install.packages('reshape')
library(reshape)
```

### 예제 데이터: airquality data

```R
df <- data.frame(고객id=c('a', 'a', 'b', 'c', 'a'),
코너=c('식료품', '식료품', '가전', '완구', '생필품'),
가격=c(5000, 7000, 40000, 98000, 50000))
```

### melt()

```R
# id를 기준으로 나머지를 재구성
melt_df <- melt(df, id=c('고객id', '코너'), na.rm=T)	# 고객id, 코너 기준. variable: 가격
melt_df <- melt(df, id=c('가격'), na.rm=T)	# 가격 기준. variable: 고객id, 코너
```

### cast()

```R
cast(melt_df, 고객id~코너~가격) # 고객 id: 행, 코너: 열, 가격: 추가 열
cast(melt_df, 고객id~코너+가격) # 고객 id: 행, 코너+가격: 열
cast(melt_df, 고객id+코너~가격) # 고객 id+코너: 행, 가격: 열
```

## sqldf()  

R에서 sql 명령어 사용

### 설치

```R
install.packages('sqldf')
library(sqldf)
```

### 예시 코드

```R
sqldf('select * from [df]')
sqldf('select * from [df] limit 6')		# =head([df])
sqldf('select * from [df] limit 10')
sqldf('select * from [df] where [col] like "char%" ')
sqldf('select * from [df] where [col] like "qn%" ')		# =subset([df], grep1('qn%', [col]))
sqldf('select * from [df] where [col] in ("BF", "HF")')	# =subset([df], [col] %in% c('BF', 'HF'))
sqldf("select * from [df1] union all select * from [df2]")	# =rbind("[df1], [df2]")
sqldf("select * from [df1], [df2]")		# =merge([df1], [df2])
sqldf("select * from [df] order by [col] desc")	# =df[order([df]$[col], decreasing=T),]
```

## plyr

### 설치

## data.table

### 설치

```R
install.packages('data.table')
library(data.table)
```

### 예시 코드

```R
DF <- data.frame(x=runif(2.6e+07), y=rep(LETTERS, each=10000))
df <- data.frame(x=runif(2.6e+07), y=rep(letters, each=10000))
system.time(x <- DF[DF$y=="c"])
DT <- as.data.table(DF)
setkey(DT, y)
system.time(x <- DT[J("C"), ])
```









