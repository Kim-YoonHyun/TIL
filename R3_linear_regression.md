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
```

#### 결과 해석

| 회귀계수 의미        | 기호 | 영어 명칭             | 값    |
| -------------------- | ---- | --------------------- | ----- |
| 선형 회귀식의 절편   | β0   | intercept coefficient | 6.409 |
| 선형 회귀식의 기울기 | β1   | slope coefficient     |       |



선형 회귀식의 절편: intercept coeffieient



Residual Standard Error(RSE): 잔차표준오차
