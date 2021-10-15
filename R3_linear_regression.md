# R Linear regression

## 코드

### 단순선형회귀분석

#### 예시 데이터

```R
x <- c(19, 23, 26, 29, 30, 38, 39, 46, 49)
y <- c(33, 51, 40, 49, 50, 69, 70, 64, 89)
df <- data.frame(x, y)
```

#### 함수

```R
lm(formula=y~x, data=df)	# y에 대한 x의 회귀분석
```

#### 결과

```R
> lm(formula=y~x, data=df)
Call:
lm(formula = y ~ x, data = df)

Coefficients:
(Intercept)            x  
      6.409        1.529  
```

```R
> summary(lm(formula=y~x, data=df))
Call:
lm(formula = y ~ x, data = df)

Residuals:
    Min      1Q  Median      3Q     Max 
-12.766  -2.470  -1.764   4.470   9.412 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)   6.4095     8.9272   0.718 0.496033    
x             1.5295     0.2578   5.932 0.000581 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 7.542 on 7 degrees of freedom
Multiple R-squared:  0.8341,	Adjusted R-squared:  0.8104 
F-statistic: 35.19 on 1 and 7 DF,  p-value: 0.0005805

결정계수 = 0.8341, 수정된 결정계수 = 0.8104

```

#### 결과 해석

| 회귀계수 의미        | 기호 | 영어 명칭             | 값    |
| -------------------- | ---- | --------------------- | ----- |
| 선형 회귀식의 절편   | β0   | intercept coefficient | 6.409 |
| 선형 회귀식의 기울기 | β1   | slope coefficient     | 1.529 |



```R
x1 <- c(7, 1, 11, 11, 7, 11, 3, 1, 2, 21, 1, 11, 10)
x2 <- c(26, 29, 56, 31, 52, 55, 71, 31, 54, 47, 40, 66, 68)
x3 <- c(6, 15, 8, 8, 6, 9, 17, 22, 18, 4, 23 ,9, 8)
x4 <- c(60, 52, 20, 47, 33, 22, 6, 44, 22, 26 ,34, 12 ,12)
y <- c(78.5, 74.3, 104.33, 87.6, 95.9, 109.2, 102.7, 72.5, 93.1, 115.9, 83.8, 113.3, 109.4)
df <- data.frame(x1, x2, x3, x4, y)
a <- lm(y~x1+x2+x3+x4, data=df)
```

```R
> summary(a)
Call:
lm(formula = y ~ x1 + x2 + x3 + x4, data = df)

Residuals:
    Min      1Q  Median      3Q     Max 
-3.1709 -1.6582  0.2505  1.3704  3.9201 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)  
(Intercept) 62.98401   69.99781   0.900   0.3945  
x1           1.54530    0.74399   2.077   0.0714 .
x2           0.50426    0.72303   0.697   0.5053  
x3           0.09574    0.75392   0.127   0.9021  
x4          -0.14993    0.70831  -0.212   0.8377  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 2.443 on 8 degrees of freedom
Multiple R-squared:  0.9824,	Adjusted R-squared:  0.9736 
F-statistic: 111.7 on 4 and 8 DF,  p-value: 4.713e-07
```

