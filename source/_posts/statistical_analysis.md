---
title: "통계 분석"
author: "Hanjeongin"
date: 2022-03-17 10:00:00
output : 
  html_document :
    keep_md : true
tags: R
---

## 통계
- 수량 데이터에서 다양한 방법으로 새로운 사실들을 찾아내는 학문

#### 통계를 알아야 하는 이유?
- 사실을 확인
- 새로운 내용을 알아내기 위해
- 기초적인 통계지식을 아느냐, 모르느냐로 빅데이터 분석 능략에 차이가 생김
- 통계지식이 없으면 심각한 오류를 범할 수 있음

## 통계의 종류

### 1) 기술통계
- 데이터의 특징을 알려주는 값
- 평균, 최솟값, 최댓값, 중앙값
- 기술통계는 사실 확인에 해당됨

### 2) 추론통계
- 변수 간의 관계를 파악
- 이를 토대로 변수 간의 인과관계나 새로운 사실들을 밝혀내는 것
- 데이터 분석에서 하는 추론통계 : 평균 차이 검정, 교차분석, 상관관계분석, 회귀분석 등

#### (1) 평균 차이 검정
- 집단별로 평균의 차이가 실제로 있는가를 검정하는 것

#### (2) 교차분석
- 범주형 변수로 구성된 집단들의 관련성을 검정하는 통계 분석
- 카이제곱검정, 카이스퀘어검정, 독립성 검정이라고도 부름

#### (3) 상관관계분석
- 변수 간의 상관관계를 알아보는 것
- 상관관계 : 변수간의 연관성, 한 변수가 변화하면 다른 변수도 변화
- 상관관계에서는 관계의 강도와 방향이 중요
- 강도 : 한 변수가 변화할 때 다른 변수가 변화하는 정도
- 방향 : 한 변수가 변화할 때 다른 변수가 같은 방향으로 변화하는지, 반대 방향으로 변화하는지를 의미

#### 변화의 강도와 방향을 나타나는 계수 : 상관계수(r)
- 상관계수는 -1부터 1 사이에 있음
- 수치가 클 수록 영향을 주는 강도가 큼
- '+'는 정의관계, '-'는 부의 관계 또는 역의 관계에 있는 것을 의미
- 상관계수가 +-0.7 이상이면 높은 관계
- +-0.4 ~ +-0.7 미만이면 다소 높은 관계
- +-0.2 ~ +-0.4 미만이면 낮은 관계
- +-0.2 미만이면 거의 없다고 해석
- 0이면 상관관계가 없음

#### (4) 회귀분석
- 상관관계로는 인과관계를 알 수 없음
- 인과관계 : 원인과 결과의 관계, 한 변수가 다른 변수에 영향을 주는 것
- 영향을 주는 변수 : 독립변수(independent variable)
- 영향을 받는 변수 : 종속변수(dependent variable)
- 회귀분석이란 독립변수와 종속변수 간의 인과관계를 분석하는 통계적 방법
- 독립변수가 1개면 단순회귀분석, 2개 이상이면 다중회귀분석

## 통계 검정

### 가설
- 어떤 현상을 설명하기 위해서 가정하는 명제, 증면되지 않은 추정
- 귀무가설 : 설정한 가설이 맞을 확률이 극히 적어, 처음부터 기각될 것으로 예상되는 가설
- 대립가설 : 귀무가설이 기각될 경우 받아들여지는 가설

### 유의수준
- 귀무가설이 맞는데도 대립가설을 채택할 확률, 즉 오류를 범할 확률
- p-value(p값)로 제시
- p값이 0.01이면 오류를 범할 확률은 1%라는 의미
- 오류가 없다는 것은 거의 불가능하기 때문에 '허용할 수 있는 오류 범위'를 정하게 됨
- 정확성은 유의수준의 범위와 반대되는 개념
- 유의수준의 범위가 넓으면 연구결과를 얻기 쉽지만 정확성이 떨어지게 됨
- 가설검정에서 인정하는 유의수준은 5%, 1%, 0.1%등 세 종류가 있음
- 사회과학에서는 5%까지 인정되나 정확성이 많이 요구되는 자연과학이나 의학에서는 유의수준의 허용범위가 좁아짐

### 척도
- 측정도구이며, 수치로 표시됨
- 명목척도, 서열척도, 등간척도, 비율척도 등 네 종류가 있음
- 척도에 따라 분석의 가능 유무가 갈리기 때문에 분석하기 전 척도의 종류를 파악해야 함

#### 명목척도
- 측정대상의 특성이나 범주를 구분하는 수치
- 산술연산을 할 수 없음
- 예시) 성별, 결혼유무, 종교, 인종등 구분

#### 서열척도
- 계급, 사회계층, 자격등급 등과 같이 측정대상의 등급순위를 나타내는 척도
- 척도 간의 거리나 간격은 나타내지 않음
- 산술연산을 할 수 없음
- 예시) 계급, 사회계층, 자격등급

#### 등간척도
- 측정대상을 일정한 간격으로 구분한 척도
- 서열과 거리, 간격을 표시
- 덧뺄셈 가능
- 예시) 온도, 학력, 시험점수

#### 비율척도
- 측정대상을 비율로 나타낼 수 있는 척도
- 사칙연산 가능
- 예시) 연령, 무게


## 통계 분석 사례

### 1) 두 집단의 평균 차이 검정
- 남녀 등 두 집단의 평균차이를 분석할 때는 독립표본 t검정
- R에서는 내장된 t.test() 함수를 사용
- 독립변수는 명목척도이며, 종속변수는 등간척도 또는 비율척도이어야 함

#### t.test() 함수 사용방법
- 방법 1) t.test(data = 데이터세터, 종속변수(비교값) ~ 독립변수(비교대상))
- 방법 2) t.test(데이터세트$종속변수(비교값) ~ 데이터세트$독립변수(비교대상))

#### 예시
- 예제파일인 mpg1.csv의 trans 변수에는 기어변속방법으로 auto와 manual 두 방식이 존재
- 두 방식에 따라 cty 평균이 통계적으로 유의미한 차이가 있는지 알아봄
- cty : 도시에서 1갤런당 달리는 거리
- 독립변수 : trans / 종속변수 : cty
- 귀무가설 : auto와 manual의 cty 평균은 차이가 없다
- 대립가설 : auto와 manual의 cty 평균은 차이가 있다


```r
mpg1 = read.csv("mpg1.csv", stringsAsFactors = F)

t.test(data = mpg1, cty ~ trans)   # t.test(mpg1$cty~mpg1$trans)도 같음
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  cty by trans
## t = -4.5375, df = 132.32, p-value = 1.263e-05
## alternative hypothesis: true difference in means between group auto and group manual is not equal to 0
## 95 percent confidence interval:
##  -3.887311 -1.527033
## sample estimates:
##   mean in group auto mean in group manual 
##             15.96815             18.67532
```

- 유의 수준 : 1,263e-05 = 1.263/100000
- 신뢰 수준 : 95%
- 유의수준이 0.05보다 매우 적기 때문에 유의수준 허용조건(p<0.05) 충족
- 'alternative hypothesis: true difference in means between group auto and group manual is not equal to 0'는 '대립가설 : 평균 차이가 있다'라는 의미
- auto의 평균은 15.96815
- manual의 평균은 18.67532
- auto와 manual의 평균은 차이가 있다는 대립가설을 채택할 수 있음
- 결론 : cty 평균거리는 자동식이 15.97마일, 수동식이 18.68마일이다. 유의수준(p)이 0.05보다 작아서 통계적으로 유의미한 차이가 있기 때문에 수동식의 평균이 자동식의 평균보다 2.7마일 길다고 할 수 있다

### 2) 교차분석
- 범주형 변수들이 관계가 있다는 것을 입증하는 것
- 비율의 차이가 있는지 검정
- R에서는 chisq.test() 함수를 사용

#### 예시
- mpg1.csv를 mpg1로 불러옴
- mpg1에 있는 trans 변수의 범주에 따라 drv 범주의 비율에 차이가 있는지 알아봄
- trans : 기어 변속방식
- drv : 구동방식
- 귀무가설 : trans에 따라 drv의 차이가 없다
- 대립가설 : trans에 따라 drv의 차이가 있다

- table() 함수와 prop.table() 함수로 교차분석을 해서 trans에 따른 drv의 빈도와 비율을 구함

```r
mpg1 = read.csv("mpg1.csv", stringsAsFactors = F)

table(mpg1$trans, mpg1$drv)   # trans와 drv의 교차분석
```

```
##         
##           4  f  r
##   auto   75 65 17
##   manual 28 41  8
```

```r
prop.table(table(mpg1$trans, mpg1$drv), 1)   #auto와 manual의 drv 비율 분석
```

```
##         
##                  4         f         r
##   auto   0.4777070 0.4140127 0.1082803
##   manual 0.3636364 0.5324675 0.1038961
```

- auto는 4륜구동이 47.8%로 가장 많음
- manual에서는 전륜구동이 53.2%로 가장 많음
- trans에 따라 drv에 차이가 있어보이나 검정을 해봐야 함
- chisq.test() 함수 이외에도 summary() 함수와 table() 함수를 조합해서 구할 수 있음

- chisq.test() 함수

```r
chisq.test(mpg1$trans, mpg1$drv)
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  mpg1$trans and mpg1$drv
## X-squared = 3.1368, df = 2, p-value = 0.2084
```

- chisq.test() 함수와 table() 함수의 조합

```r
chisq.test(table(mpg1$trans, mpg1$drv))
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  table(mpg1$trans, mpg1$drv)
## X-squared = 3.1368, df = 2, p-value = 0.2084
```

- summary() 함수와 table() 함수의 조합

```r
summary(table(mpg1$trans, mpg1$drv))
```

```
## Number of cases in table: 234 
## Number of factors: 2 
## Test for independence of all factors:
## 	Chisq = 3.1368, df = 2, p-value = 0.2084
```

- 유의수준이 0.2084로 p > 0.05
- 대립가설을 기각하지 못하므로 trans에 따라 drv에 차이가 있다고 할 수 없음
- 'X-squared = 3.1368'은 통계 검정 값


### 3) 상관관계분석
- R에 내장되어 있는 cor.test() 함수 사용
- cor.test(데이터세트$비교변수1, 데이터세트$비교변수2)

#### 예시
- cty가 길면 hwy도 길 것이라고 생각할 수 있다
- cty : 도시에서 1갤런당 달리는 거리
- hwy : 고속도로에서 1갤런당 달리는 거리
- 귀무가설 : cty와 hwy는 상관관계가 없다
- 대립가설 : cty와 hwy는 상관관계가 있다

```r
mpg1 = read.csv("mpg1.csv", stringsAsFactors = F)

cor.test(mpg1$cty, mpg1$hwy)   # 상관관계분석
```

```
## 
## 	Pearson's product-moment correlation
## 
## data:  mpg1$cty and mpg1$hwy
## t = 49.585, df = 232, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.9433129 0.9657663
## sample estimates:
##       cor 
## 0.9559159
```

- 유의수준이 2.2e-16이므로 0.5보다 작음
- 대립가설은 '상관관계는 0이 아니다(alternative hypothesis: true correlation is not equal to 0)'이므로 귀무가설을 기각하고 대립가설을 채택
- 상관관계는 0.9559159이므로 매우 높음
- 결과 : cty와 hwy는 유의미하게 매우 높은 상관관계


### 4) 회귀분석
#### (1) 단순회귀분석
- 독립변수가 1개, 종속변수가 1개일 때 사용
- 회귀분석의 변수는 독립변수와 종속변수가 모두 등간척도 또는 비율척도이어야 함
- R의 lm() 함수를 사용
- 방법 1) lm(data = 데이터세트, 종속변수 ~ 독립변수)
- 방법 2) lm(종속변수~독립변수, data = 데이터세트)
- 방법 3) lm(데이터세트$종속변수 ~ 데이터세트$독립변수)

#### 예시
- R에 있는 mtcars 데이터세트 분석
- 11개 변수에서 32개 자동차 정보를 담고 있음
- disp : 배기량
- mpg : 1갤런당 주행 마일
- str(mtcars)로 변수들을 보면, disp와 mpg 모두 실수형 변수이므로 회귀분석이 가능함
- 귀무가설 : disp는 mpg에 영향을 주지 않는다
- 대립가설 : disp는 mpg에 영향을 준다


```r
lm(data = mtcars, mpg ~ disp)
```

```
## 
## Call:
## lm(formula = mpg ~ disp, data = mtcars)
## 
## Coefficients:
## (Intercept)         disp  
##    29.59985     -0.04122
```

```r
# lm(mpg ~ disp, data = mtcars), lm(mtcars$mpg ~ mtcars$disp)의 결과도 같음
```

- disp의 계수는 -0.04122
- disp의 절편은 29.59985
- 위 회귀분석에서 구해진 식 : mpg = 29.59985 - 0.04122 * disp
- lm()의 결과를 summary() 함수에 넣으면 유의수준을 비롯한 상세한 회귀분석의 결과를 볼 수 있음


```r
RA = lm(data = mtcars, mpg ~ disp)   # 회귀분석 결과를 RA에 넣기기

summary(RA)   # 상세한 분석 결과 출력력
```

```
## 
## Call:
## lm(formula = mpg ~ disp, data = mtcars)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.8922 -2.2022 -0.9631  1.6272  7.2305 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 29.599855   1.229720  24.070  < 2e-16 ***
## disp        -0.041215   0.004712  -8.747 9.38e-10 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 3.251 on 30 degrees of freedom
## Multiple R-squared:  0.7183,	Adjusted R-squared:  0.709 
## F-statistic: 76.51 on 1 and 30 DF,  p-value: 9.38e-10
```

- 유의수준이 9.38e-10이므로 0.05보다 작기 때문에 회귀모형이 적합하고, 대립가설을 채택할 수 있음
- 회귀식 : mpg = -0.04122 * disp + 29.59985
- *** : 유의수준 0.001 충족, ** : 유의수준 0.01 충족, * : 유의수준 0.05 충족
- R-squared : 결정계수, 0에 가까울수록 회귀식과 맞지 않는 데이터가 많고, 추정된 회귀선의 예측 정확도와 변수 관계 설명력이 낮음
- Adjusted R-squared : 수정된 결정계수, 결정계수는 데이터와 독립변수가 많을수록 회귀식의 예측력과 무관하게 커지는 경향이 있어서 이를 보완한 결정계수
- 연구 결과에서는 수정된 결정계수를 밝히는 것이 일반적
- 분석 결과 : 회귀모형은 유의수준 p < 0.001에서 적합하며, 회귀식의 수정된 결정계수는 0.709이다. 배기량이 연비에 미치는 회귀계수는 유의수준 p < 0.001에서 -0.04이다.


#### (2) 다중회귀분석
- 종속변수에 영향을 주는 독립변수가 복수일 때 분석하는 방법
- 다중회귀분석에서는 단순회귀분석의 독립변수들을 '+' 기호로 연결
- 방법1 lm(data = 데이터세트, 종속변수 ~ 독립변수1 + 독립변수2 + ...)
- 방법2 lm(종속변수 ~ 독립변수1 + 독립변수2 + ..., data = 데이터세트)
- 방법3 lm(데이터세트$종속변수 ~ 데이터세트$독립변수1 + 데이터세트$독립변수2 + ...)

#### 예제
- mpg에는 disp 이외에도 hp와 wt가 영향을 미칠 수 있음
- hp : 마력
- wt : 중량


```r
lm(data = mtcars, mpg ~ disp + hp + wt)
```

```
## 
## Call:
## lm(formula = mpg ~ disp + hp + wt, data = mtcars)
## 
## Coefficients:
## (Intercept)         disp           hp           wt  
##   37.105505    -0.000937    -0.031157    -3.800891
```

```r
#lm(mtcars$mpg ~ mtcars$disp + mtcars$hp + mtcars$wt)의 결과도 같음
```

- 다중 회귀식 : mpg = 37.105505 - 0.000937 * disp - 0.031157 * hp - 3.800891 * wt
- lm()의 결과를 summary() 함수에 넣으면 유의수준을 비롯한 상세한 회귀분석의 결과를 볼 수 있음


```r
RA = lm(data = mtcars, mpg ~ disp + hp + wt)   # 회귀분석 결과를 RA에 넣기

summary(RA)
```

```
## 
## Call:
## lm(formula = mpg ~ disp + hp + wt, data = mtcars)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -3.891 -1.640 -0.172  1.061  5.861 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 37.105505   2.110815  17.579  < 2e-16 ***
## disp        -0.000937   0.010350  -0.091  0.92851    
## hp          -0.031157   0.011436  -2.724  0.01097 *  
## wt          -3.800891   1.066191  -3.565  0.00133 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.639 on 28 degrees of freedom
## Multiple R-squared:  0.8268,	Adjusted R-squared:  0.8083 
## F-statistic: 44.57 on 3 and 28 DF,  p-value: 8.65e-11
```

- p값은 8.65e-11로 유의수준 0.001보다 작아 회귀모형에 적합
- disp의 유의수준은 0.92851로 0.05 이상, hp는 0.01097로 0.05 이하, wt는 0.00133으로 0.01이하
- disp는 mpg에 영향을 주지 않고, hp와 wt만 영향을 줌
- hp가 1단위씩 증가하면 mpg는 0.031157씩 감소
- wt가 1단위씩 증가하면 mpg는 3.800891씩 감소
- 앞의 예시와 달리 disp가 mpg에 영향을 주지 않음, 같은 독립변수라도 분석 방법에 따라 영향력이 달라짐
- 수정된 결정계수는 0.8083으로 높아서 회귀모델의 설명력이 높음
- 결론 : 회귀모형은 유의수준 p < 0.001에서 적합하며, 회귀식의 수정된 결정계수는 0.81이다. 3개 독립변수가 연비에 미치는 회귀계수는 hp가 -0.03(p < 0.05), wt가 -3.80(p < 0.01)이었고, disp는 없었다. wt의 영향력이 가장 컸다.

