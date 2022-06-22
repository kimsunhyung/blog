---
title: "day0622_visual"
output:
  html_document:
    keep_md: true
date: '2022-06-22'
---





```r
# install.packages("ggplot2")
# install.packages("readxl")
```
## ggplot2 강의
- 데이터 불러오기
- aes = 축
- 오류 시 +후 한칸 뛰고 그래프 종류 작성
- 그래프 색상 변화 다양한 색 참고 (https://color.adobe.com/ko/create/color-wheel)

```r
library(readxl)
library("ggplot2")
who_disease <- read_xlsx("who_disease.xlsx")

# 기본 시각화
ggplot(who_disease, aes(x = year, y = cases)) +
  geom_point()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
# 옵션 1 투명도 주기
ggplot(who_disease, aes(x = year, y = cases)) +
  geom_point(alpha = 0.1)
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-2-2.png)<!-- -->

```r
# 옵션 2 색상 변화
ggplot(who_disease, aes(x = year, y = cases)) +
  geom_point(alpha = 0.1, color = "#EB3BD9")
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-2-3.png)<!-- -->

```r
?geom_point
```

```
## httpd 도움말 서버를 시작합니다 ... 완료
```
- colour 입력 위치
  + geom_point(colour =red) => 색상의 숫자가 들어가야함.
  + aes(x, y, colour = 컬럼명)

```r
str(iris)
```

```
## 'data.frame':	150 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```

```r
ggplot(iris, aes(x = Sepal.Length, 
                 y = Sepal.Width,
                 colour = Petal.Length)) + geom_point()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

- 산점도 : x축 수치형 연속형 데이터, y축 수치형 연속형 데이터 
- 

```r
ggplot(who_disease, aes(x = year, y = cases)) +
  geom_point(alpha = 0.1)
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-4-1.png)<!-- -->
- 히스토그램 (분포가 어느 부분에 모여있는지 알아야 함.)
  + 질병 데이터 region = AMR, year = 1980, disease = 백일해(pertussis)
    cases > 0

```r
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
str(who_disease)
```

```
## tibble [43,262 × 6] (S3: tbl_df/tbl/data.frame)
##  $ region     : chr [1:43262] "EMR" "EUR" "AFR" "EUR" ...
##  $ countryCode: chr [1:43262] "AFG" "ALB" "DZA" "AND" ...
##  $ country    : chr [1:43262] "Afghanistan" "Albania" "Algeria" "Andorra" ...
##  $ disease    : chr [1:43262] "measles" "measles" "measles" "measles" ...
##  $ year       : num [1:43262] 2016 2016 2016 2016 2016 ...
##  $ cases      : num [1:43262] 638 17 41 0 53 0 0 2 99 27 ...
```

```r
data2 <- who_disease %>% 
  filter(region == 'AMR',
         year == 1980,
         disease == 'pertussis',
         cases > 0) -> data2

ggplot(data2, aes(x = cases)) + 
  geom_histogram(fill = "red")
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
ggplot(data2, aes(x = country, y = cases)) + 
  geom_col(fill = "blue") +
  # 가로로 그래프 나타내기
  coord_flip()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-5-2.png)<!-- -->

```r
ggplot(who_disease,aes(region)) +
  geom_bar()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-5-3.png)<!-- -->

```r
ggplot(who_disease,aes(disease)) +
  geom_bar()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-5-4.png)<!-- -->
- 교재 풀기
## 산점도 그리기

```r
ggplot(data=diamonds,aes(x=carat, y=price)) + geom_point()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
## 막대그래프 그리기

```r
ggplot(diamonds, aes(x=cut)) +geom_bar()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
ggplot(diamonds, aes(x=cut, y= price)) + geom_bar(stat = "identity")
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-7-2.png)<!-- -->

```r
cut_price <-diamonds %>%
  group_by(cut) %>% 
  summarise(mean_price=mean(price))
ggplot(data = cut_price, aes(x = cut, y = mean_price)) + geom_col()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-7-3.png)<!-- -->

```r
ggplot(data=diamonds, aes(x=carat)) + geom_histogram()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-7-4.png)<!-- -->

```r
ggplot(data=diamonds, aes(x=carat)) + geom_histogram(binwidth = 1)
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-7-5.png)<!-- -->

```r
ggplot(data=diamonds, aes(x=carat)) + geom_histogram(binwidth = 0.1)
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-7-6.png)<!-- -->
## 커널 밀도 곡선 그래프 만들기

```r
ggplot(data=diamonds, aes(x=carat)) + geom_density()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
## 상자 그림 그래프 그리기

```r
ggplot(data=diamonds, aes(x=1, y=price)) + geom_boxplot()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
ggplot(data=diamonds, aes(x=cut, y=price)) +geom_boxplot()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-9-2.png)<!-- -->
## 선그래프 그리기

```r
str(cars)
```

```
## 'data.frame':	50 obs. of  2 variables:
##  $ speed: num  4 4 7 7 8 9 10 10 10 11 ...
##  $ dist : num  2 10 4 22 16 10 18 26 34 17 ...
```

```r
str(economics)
```

```
## spec_tbl_df [574 × 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ date    : Date[1:574], format: "1967-07-01" "1967-08-01" ...
##  $ pce     : num [1:574] 507 510 516 512 517 ...
##  $ pop     : num [1:574] 198712 198911 199113 199311 199498 ...
##  $ psavert : num [1:574] 12.6 12.6 11.9 12.9 12.8 11.8 11.7 12.3 11.7 12.3 ...
##  $ uempmed : num [1:574] 4.5 4.7 4.6 4.9 4.7 4.8 5.1 4.5 4.1 4.6 ...
##  $ unemploy: num [1:574] 2944 2945 2958 3143 3066 ...
```

```r
ggplot(data=cars,aes(x=speed, y=dist))+geom_line()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
ggplot(data=economics, aes(x=date, y=unemploy)) + geom_line()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-10-2.png)<!-- -->

```r
ggplot(data=economics, aes(x=date, y=psavert)) + geom_line()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-10-3.png)<!-- -->

```r
ggplot() + 
geom_line(data=economics,aes(x=date, y=uempmed, color="red")) +  geom_line(data=economics, aes(x=date, y=psavert))
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-10-4.png)<!-- -->

## 함수를 정교하게 그리기

```r
ggplot(data = diamonds, aes(x=carat, y=price, col = cut)) + geom_point()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

- 막대그래프 2개 범주 내용 반영하기

```r
ggplot(diamonds, aes(x=color, fill = cut)) + geom_bar()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

```r
ggplot(diamonds, aes(x=color, fill = cut)) + geom_bar(position = "dodge")
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-12-2.png)<!-- -->

```r
ggplot(diamonds, aes(x=color, fill = cut)) + geom_bar(position = "fill")
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-12-3.png)<!-- -->

- 선 그래프에 2개 범주 내용 반영

```r
leisure <- read.csv("data/leisure.csv")
str(leisure)
```

```
## 'data.frame':	200 obs. of  3 variables:
##  $ age    : int  2 2 3 3 4 4 5 5 6 6 ...
##  $ sex    : chr  "female" "male" "female" "male" ...
##  $ expense: num  25.8 21 30 16.3 25.7 ...
```

```r
ggplot(data = leisure, aes(x=age, y=expense)) + geom_line()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

```r
ggplot(data = leisure, aes(x=age, y=expense, col=sex)) + geom_line()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-14-2.png)<!-- -->
### 막대 그래프의 순서 변경
-reorder()

```r
mpg1 <- read.csv("data/mpg1.csv", stringsAsFactors = FALSE)

drv_hwy <- mpg1 %>% 
  group_by(drv) %>% 
  summarise(mean_hwy=mean(hwy))
```

```r
ggplot(data = drv_hwy, aes(x = drv, 
                           y= mean_hwy)) + geom_col()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

```r
ggplot(data = drv_hwy, aes(x = reorder(drv, mean_hwy), #오름차순
                           y= mean_hwy)) + geom_col()
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-16-2.png)<!-- -->

```r
ggplot(data = drv_hwy, aes(x = reorder(drv, -mean_hwy), #내림차순
                           y= mean_hwy)) + 
  geom_col() + 
  labs(
  title = "그래프의 제목을 입력하세요.",
  subtitle = "그래프의 소제목을 입력하세요.",
  x = "x변수명을 입력하세요.",
  y = "y변수명을 입력하세요.",
  caption= "데이터 출처를 입력하세요.")
```

![](/images/day0622_visual_files/figure-html/unnamed-chunk-16-3.png)<!-- -->

