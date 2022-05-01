---
title: "Basic Statistics"
author: "Hanjeongin"
date: 2022-03-17 09:00:00
output : 
  html_document :
    keep_md : true
tags: R
---

## 모수와 통계량

- 분산값이 크다 -> 평균으로부터 멀어진 개별적인 데이터가 많다
- 분산값이 작다 -> 평균 주위에 개별적인 데이터가 많다


## 기술통계량(Descriptive Analysis)
- 최대, 최소, 평균, 중간값, 분산, 표준편차


## 앨범 판매량 데이터
- adverts : 광고집행비용
- sales : 판매량


```r
albums = read.csv("album.csv")

str(albums)
```

```
## 'data.frame':	200 obs. of  2 variables:
##  $ adverts: num  10.3 985.7 1445.6 1188.2 574.5 ...
##  $ sales  : int  330 120 360 270 220 170 70 210 200 300 ...
```


```r
albums$sales
```

```
##   [1] 330 120 360 270 220 170  70 210 200 300 290  70 150 190 240 100 250 210
##  [19] 280 230 210 230 320 210 230 250  60 330 150 150 180  80 180 130 320 280
##  [37] 200 130 190 150 230 310 340 240 180 220  40 190 290 340 250 190 120 230
##  [55] 190 210 170 310  90 140 300 340 170 100 200  80 100  70  50 240 160 290
##  [73] 140 210 300 230 280 160 200 110 110  70 100 190  70 360 360 300 120 150
##  [91] 220 280 300 140 290 180 140 210 250 250 120 290  60 140 290 160 100 160
## [109] 150 140 230 230  30  80 190  90 120 150 230 150 210 180 140 360  10 240
## [127] 270 290 220 230 220 240 260 170 130 270 140  60 210 210 240 210 200 140
## [145]  90 120 100 360 180 150 110  90 160 230  40  60 230 230 120 150 120  60
## [163] 280 120 230 230  40 140 360 210 260 250 200 150 250 100 260 210 290 220
## [181]  70 110 250 320 300 180 180 200 320 140 100 120 230 150 250 190 240 250
## [199] 230 110
```


```r
sort(albums$sales)
```

```
##   [1]  10  30  40  40  40  50  60  60  60  60  60  70  70  70  70  70  70  80
##  [19]  80  80  90  90  90  90 100 100 100 100 100 100 100 100 110 110 110 110
##  [37] 110 120 120 120 120 120 120 120 120 120 120 130 130 130 140 140 140 140
##  [55] 140 140 140 140 140 140 140 150 150 150 150 150 150 150 150 150 150 150
##  [73] 150 160 160 160 160 160 170 170 170 170 180 180 180 180 180 180 180 180
##  [91] 190 190 190 190 190 190 190 190 200 200 200 200 200 200 200 210 210 210
## [109] 210 210 210 210 210 210 210 210 210 210 220 220 220 220 220 220 230 230
## [127] 230 230 230 230 230 230 230 230 230 230 230 230 230 230 230 240 240 240
## [145] 240 240 240 240 250 250 250 250 250 250 250 250 250 250 260 260 260 270
## [163] 270 270 280 280 280 280 280 290 290 290 290 290 290 290 290 300 300 300
## [181] 300 300 300 310 310 320 320 320 320 330 330 340 340 340 360 360 360 360
## [199] 360 360
```

- 최소값

```r
# sort(albums$sales)[1]
min(albums$sales)
```

```
## [1] 10
```

- 최대값

```r
# sort(albums$sales)[200]
max(albums$sales)
```

```
## [1] 360
```

- 최빈값

```r
table(albums$sales)
```

```
## 
##  10  30  40  50  60  70  80  90 100 110 120 130 140 150 160 170 180 190 200 210 
##   1   1   3   1   5   6   3   4   8   5  10   3  11  12   5   4   8   8   7  13 
## 220 230 240 250 260 270 280 290 300 310 320 330 340 360 
##   6  17   7  10   3   3   5   8   6   2   4   2   3   6
```


```r
sort(table(albums$sales), decreasing = TRUE)[1]
```

```
## 230 
##  17
```

- 평균과 중간값

```r
mean(albums$sales)   # 193.2
```

```
## [1] 193.2
```

```r
median(albums$sales)   # 200
```

```
## [1] 200
```

- 결측치 데이터 추가

```r
sales2 = c(albums$sales, NA)
tail(sales2)
```

```
## [1] 190 240 250 230 110  NA
```

```r
mean(sales2)
```

```
## [1] NA
```

```r
mean(sales2, na.rm = TRUE)   # NA값 빼고 계산
```

```
## [1] 193.2
```

## 평균의 가장 큰 단점
- 극단적인 값에 굉장히 민감하게 반응


```r
sales3 = c(albums$sales, 30000000000)
mean(sales3)
```

```
## [1] 149253924
```

## 중앙값
- 양 극단의 값에 민감하게 반응하지 않음
- 이러한 특징을 강건하다(Robust)

```r
median(sales3)
```

```
## [1] 200
```

- 분산

```r
weight = c(64, 68, 70, 72, 76)
weight_mean = mean(weight)   # 평균
weight_deviation = weight - weight_mean   # 편차
weight_deviation
```

```
## [1] -6 -2  0  2  6
```

```r
sum(weight_deviation)
```

```
## [1] 0
```

```r
weight_deviation_2 = weight_deviation ^ 2
sum(weight_deviation_2)
```

```
## [1] 80
```

```r
mean(weight_deviation_2)   # 분산
```

```
## [1] 16
```

- 표준편차

```r
sqrt(mean(weight_deviation_2))   # sqrt는 제곱근
```

```
## [1] 4
```

- R 내장 함수

```r
var(weight)   # 표본분산
```

```
## [1] 20
```

```r
sd(weight)   # 표본표준편차
```

```
## [1] 4.472136
```



## 변동계수 : 표준편차를 평균으로 나눈 값
- CV : Coefficient of Variation = 변동계수
- RSD : Relative Standard Deviation = 상대 표준 편차

- cv = sd(data) / mean(data)


## 주식 데이터

```r
# install.packages("tidyquant")
library(tidyquant)
```

```
## Warning: 패키지 'tidyquant'는 R 버전 4.1.3에서 작성되었습니다
```

```
## 필요한 패키지를 로딩중입니다: lubridate
```

```
## 
## 다음의 패키지를 부착합니다: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```
## 필요한 패키지를 로딩중입니다: PerformanceAnalytics
```

```
## Warning: 패키지 'PerformanceAnalytics'는 R 버전 4.1.3에서 작성되었습니다
```

```
## 필요한 패키지를 로딩중입니다: xts
```

```
## 필요한 패키지를 로딩중입니다: zoo
```

```
## 
## 다음의 패키지를 부착합니다: 'zoo'
```

```
## The following objects are masked from 'package:base':
## 
##     as.Date, as.Date.numeric
```

```
## 
## 다음의 패키지를 부착합니다: 'PerformanceAnalytics'
```

```
## The following object is masked from 'package:graphics':
## 
##     legend
```

```
## 필요한 패키지를 로딩중입니다: quantmod
```

```
## Warning: 패키지 'quantmod'는 R 버전 4.1.3에서 작성되었습니다
```

```
## 필요한 패키지를 로딩중입니다: TTR
```

```
## Warning: 패키지 'TTR'는 R 버전 4.1.3에서 작성되었습니다
```

```
## Registered S3 method overwritten by 'quantmod':
##   method            from
##   as.zoo.data.frame zoo
```

```
## == Need to Learn tidyquant? ====================================================
## Business Science offers a 1-hour course - Learning Lab #9: Performance Analysis & Portfolio Optimization with tidyquant!
## </> Learn more at: https://university.business-science.io/p/learning-labs-pro </>
```

```r
library(quantmod)
library(purrr)
library(ggplot2)
library(tibble)

tickers = c("AAPL", "TSLA")
getSymbols(tickers,
           from = "2022-01-02",
           to = "2022-01-30")
```

```
## 'getSymbols' currently uses auto.assign=TRUE by default, but will
## use auto.assign=FALSE in 0.5-0. You will still be able to use
## 'loadSymbols' to automatically load data. getOption("getSymbols.env")
## and getOption("getSymbols.auto.assign") will still be checked for
## alternate defaults.
## 
## This message is shown once per session and may be disabled by setting 
## options("getSymbols.warning4.0"=FALSE). See ?getSymbols for details.
```

```
## [1] "AAPL" "TSLA"
```

```r
stock = map(tickers, function(x) Ad(get(x)))
stock = reduce(stock, merge)
colnames(stock) = tickers

head(stock)
```

```
##                AAPL    TSLA
## 2022-01-03 181.7784 1199.78
## 2022-01-04 179.4713 1149.59
## 2022-01-05 174.6974 1088.12
## 2022-01-06 171.7811 1064.70
## 2022-01-07 171.9509 1026.96
## 2022-01-10 171.9709 1058.12
```

```r
stock_df = stock %>%
  data.frame(date = index(stock))
stock_df
```

```
##                AAPL    TSLA       date
## 2022-01-03 181.7784 1199.78 2022-01-03
## 2022-01-04 179.4713 1149.59 2022-01-04
## 2022-01-05 174.6974 1088.12 2022-01-05
## 2022-01-06 171.7811 1064.70 2022-01-06
## 2022-01-07 171.9509 1026.96 2022-01-07
## 2022-01-10 171.9709 1058.12 2022-01-10
## 2022-01-11 174.8572 1064.40 2022-01-11
## 2022-01-12 175.3066 1106.22 2022-01-12
## 2022-01-13 171.9709 1031.56 2022-01-13
## 2022-01-14 172.8498 1049.61 2022-01-14
## 2022-01-18 169.5839 1030.51 2022-01-18
## 2022-01-19 166.0185  995.65 2022-01-19
## 2022-01-20 164.3007  996.27 2022-01-20
## 2022-01-21 162.2034  943.90 2022-01-21
## 2022-01-24 161.4143  930.00 2022-01-24
## 2022-01-25 159.5767  918.40 2022-01-25
## 2022-01-26 159.4868  937.41 2022-01-26
## 2022-01-27 159.0174  829.10 2022-01-27
## 2022-01-28 170.1133  846.35 2022-01-28
```

## 사용자 정의 함수

```r
cv_fun = function(data){
   result = sd(data) / mean(data) * 100
   return(result)
}

cv_fun(stock_df$AAPL)   # 4.034862
```

```
## [1] 4.034862
```

```r
cv_fun(stock_df$TSLA)   # 9.46726
```

```
## [1] 9.46726
```

- 그래프

```r
ggplot(stock_df, aes(x = date)) +
  geom_line(aes(y = AAPL, colour = "Apple")) +
  geom_line(aes(y = TSLA, colour = "Tesla")) +
  scale_colour_manual(name = "Company", values = 
  c("Apple" = "red", "Tesla" = "darkblue")) +
  theme_bw()
```

![](/images/basic_statistics_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

## 4분위수

```r
qs_df = quantile(albums$sales)
qs_df[4] - qs_df[2]
```

```
##   75% 
## 112.5
```

```r
IQR(albums$sales)   # 3사분위에서 1사분위를 뺀 값
```

```
## [1] 112.5
```

- boxplot

```r
boxplot(albums$sales)
```

![](/images/basic_statistics_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

## 이상치 판별

```r
sales4 = c(albums$sales, 450, 460, -100, -1000)

q = quantile(sales4)
boxplot(sales4)
```

![](/images/basic_statistics_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

- 이상치 하한 극단 경계값 찾기

```r
bottom_outlier = q[2] - 1.5 * (q[4] - q[2])
bottom_outlier
```

```
## 25% 
## -50
```

- 이상치 상한 극단 경계값 찾기

```r
top_outlier = q[4] + 1.5 * (q[4] - q[2])
top_outlier
```

```
## 75% 
## 430
```

- 실제로 극단값이 존재하는지 출력

```r
sales4[sales4 < bottom_outlier]
```

```
## [1]  -100 -1000
```

```r
sales4[sales4 > top_outlier]
```

```
## [1] 450 460
```

