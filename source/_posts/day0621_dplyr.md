---
title: "day0621_dplyr에 대하여"
output:
  html_document:
    keep_md: true
date: '2022-06-21'
---



- p.98
- 분석 프로세스

- 데이터 전처리를 위한 도구 dplyr
- 데이터 전처리를 위한 도구 data. table

- 처리 속도 차이
  - dplyr : 10gb 이내
  - data.table : 50gb 이상
  
- 배움의 측면 
  - dplyr 매우 쉬움
  - data. table 어려움
  
- 라이브러리 불러오기
- r 마크다운 사용 시 install.packages 사용 x 만약 사용 시 # 주석을 넣어주어야 함.

```r
# install.packages("dplyr")
library(dplyr)
```

```
## 
## 다음의 패키지를 부착합니다: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
mpg1<- read.csv("mpg1.csv", stringsAsFactors = F)
```

- 여러개의 변수 쉽게 변경

```r
str(mpg1)
```

```
## 'data.frame':	234 obs. of  5 variables:
##  $ manufacturer: chr  "audi" "audi" "audi" "audi" ...
##  $ trans       : chr  "auto" "manual" "manual" "auto" ...
##  $ drv         : chr  "f" "f" "f" "f" ...
##  $ cty         : int  18 21 20 21 16 18 18 18 16 20 ...
##  $ hwy         : int  29 29 31 30 26 26 27 26 25 28 ...
```

```r
data2 <- mpg1 %>% 
  select(drv, cty, hwy) %>% 
  filter(drv == "f")
```

- 여러개의 어렵게 변수 변경

```r
data3 <- select(mpg1, drv, cty, hwy)
data3 <- filter(data3, drv == "f")
```

- ctrl+shift+m : %>% 
- select : 컬럼명 추출
- filter : 행추출 (조건식)
- mutate : 
- group by :
- summarize () :

- filter, select, rename, mutate 등등
iris

-데이터 미리보기
glimpse(iris),
 #Species, setosa versicolor

```r
glimpse(iris)
```

```
## Rows: 150
## Columns: 5
## $ Sepal.Length <dbl> 5.1, 4.9, 4.7, 4.6, 5.0, 5.4, 4.6, 5.0, 4.4, 4.9, 5.4, 4.…
## $ Sepal.Width  <dbl> 3.5, 3.0, 3.2, 3.1, 3.6, 3.9, 3.4, 3.4, 2.9, 3.1, 3.7, 3.…
## $ Petal.Length <dbl> 1.4, 1.4, 1.3, 1.5, 1.4, 1.7, 1.4, 1.5, 1.4, 1.5, 1.5, 1.…
## $ Petal.Width  <dbl> 0.2, 0.2, 0.2, 0.2, 0.2, 0.4, 0.3, 0.2, 0.2, 0.1, 0.2, 0.…
## $ Species      <fct> setosa, setosa, setosa, setosa, setosa, setosa, setosa, s…
```

```r
iris %>% 
 filter(Species != "virginica") %>% 
 select(Sepal.Length, Sepal.Width) %>%
 filter(Sepal.Length >5.0) %>% 
 mutate(total = Sepal.Length + Sepal.Width) ->data2
```
-> data2 : 마무리되었다는 뜻

- 집단별 통계량 (평균, 총합, 중간값)

```r
  mpg1 %>% 
  group_by(trans) %>% 
  summarise(avg   = mean(cty)
           ,total = sum(cty)
           ,med   = median(cty))   
```

```
## # A tibble: 2 × 4
##   trans    avg total   med
##   <chr>  <dbl> <int> <int>
## 1 auto    16.0  2507    16
## 2 manual  18.7  1438    18
```





- p99 - p120
변수 이름 바꾸기

```r
mpg1_newname1 <- mpg1 %>% 
  rename(transmission=trans, drive_method=drv, city=cty, highway=hwy)

mpg1_newname2 <- rename(mpg1, transmission=trans, drivemethod=drv, city=cty, highway=hwy)
```

빈도 분석

```r
count(mpg1,trans)
```

```
##    trans   n
## 1   auto 157
## 2 manual  77
```

```r
class(count(mpg1,trans))
```

```
## [1] "data.frame"
```

```r
table(mpg1$trans)
```

```
## 
##   auto manual 
##    157     77
```

- 필요한 열 추출하기

```r
mpg1_1 <- mpg1 %>% select(manufacturer, trans, cty)
mpg1_2 <- select(mpg1, manufacturer, trans, cty)
```
- 불필요한 열을 빼고 필요한 열만 남기기

```r
mpg1_type1 <- mpg1 %>% select(-cty,-hwy)
mpg1_type2 <- mpg1 %>% select(-c(cty,-hwy))
mpg1_type3 <- select(mpg1, -cty, -hwy)
mpg1_type4 <- select(mpg1,-c(cty,hwy))
```
- 행의 일부를 추출하기

```r
mpg1 %>% slice(1,4,5)
```

```
##   manufacturer trans drv cty hwy
## 1         audi  auto   f  18  29
## 2         audi  auto   f  21  30
## 3         audi  auto   f  16  26
```

```r
slice(mpg1,1,4,5)
```

```
##   manufacturer trans drv cty hwy
## 1         audi  auto   f  18  29
## 2         audi  auto   f  21  30
## 3         audi  auto   f  16  26
```
-비교값이 같은 데이터 추출 및 최대값 구하기

```r
mpg1_hd1 <- mpg1 %>% filter(manufacturer == "hyundai") 
mpg1_hd2 <- filter(mpg1, manufacturer =="hyundai")
max(mpg1$cty)
```

```
## [1] 35
```
- 비교값이 다른 데이터 추출

```r
mpg1_no_hd1 <- mpg1 %>% filter(manufacturer!="hyundai")
mpg1_no_hd2 <- filter(mpg1, manufacturer!="hyundai")
```

- 평균값 구하기 및 비교값이 크거나 작은 데이터를 추출

```r
mean(mpg1$cty)
```

```
## [1] 16.85897
```

```r
mpg1_low1 <-mpg1 %>% filter(cty<16.85897)
mpg1_low2 <-filter(mpg1,cty<16.85897)
mpg1_low3 <- mpg1 %>% filter(cty < mean(cty))
```
- 1사 분위수 알아보기 및 비교값이 작거나 같은 (<=) 데이터 추출

```r
quantile(mpg1$cty)
```

```
##   0%  25%  50%  75% 100% 
##    9   14   17   19   35
```

```r
mpg1_low4 <- mpg1 %>% filter(cty<=14)
mpg1_low5 <- filter(mpg1,cty<=14)
```
- 중앙값 구하기 및 비교값이 큰 데이터 추출

```r
median(mpg1$cty)
```

```
## [1] 17
```

```r
mpg1_high1 <- mpg1 %>% filter(cty>17)
mpg1_high2 <- filter(mpg1,cty>17)
mpg1_high3 <- mpg1 %>% filter(cty>median(cty))
```
- 삼분위 값 알아보기 및 비교값이 크거나 같은 데이터 추출

```r
quantile(mpg1$cty)
```

```
##   0%  25%  50%  75% 100% 
##    9   14   17   19   35
```

```r
mpg1_high4 <- mpg1 %>% filter(cty>=19)
mpg1_high5 <-filter(mpg1,cty>=19)
```
- 복수의 조건을 충족하는 데이터 추출

```r
mpg1_hd3 <- mpg1 %>% filter(manufacturer=="hyundai"&cty>=16.85897)
mpg1_hd4 <- filter(mpg1, manufacturer=="hyundai"&cty>16.85897)
mpg1_hd5 <- mpg1 %>% filter(manufacturer=="hyundai"&cty>=mean(cty))
```
- 조건이 3개일 때 

```r
mpg1_hd6 <- mpg1 %>% filter(manufacturer=="hyundai"&trans=="auto"&cty>=17)
mpg1_hd7 <- filter(mpg1,manufacturer=="hyundai"&trans=="auto"&cty>=17)
mpg1_hd8 <- mpg1 %>%filter(manufacturer=="hyundai"&trans=="auto"&cty>=median(mpg1$cty))
```
- 복수 조건중 하나라도 충족하는 데이터 추출

```r
mpg1_j1<-mpg1 %>% filter(manufacturer=="honda"|manufacturer=="nissan"|
                         manufacturer=="subaru"|manufacturer=="toyota")
mpg1_j2<-filter(mpg1,manufacturer=="honda"|manufacturer=="nissan"|
                manufacturer=="subaru"|manufacturer=="toyota")
```
- 1개의 파생변수 만들기

```r
mpg2 <- mpg1 %>% mutate(total=cty+hwy)
```
- 복수의 파생변수 만들기

```r
mpg3 <- mpg1 %>% mutate(total=cty+hwy,mean=cty+hwy/2)
```

