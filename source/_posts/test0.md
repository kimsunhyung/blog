---
title: "6/17 github 블로그"
output:
  html_document:
    keep_md:git true
date: '2022-06-21'
--- 

# 6/17 github 블로그
- test

- 티스토리 블로그/ 네이버 블로그/ medium사이트
- 깃허브 블로그

  - hugo, hexo, jekyll

  - html, css, git & github, 터미널 명령어

  - 마크 다운

 IT/기획자(제안서) → 티스토리 블로그

 개발자(코딩) → 깃허브 블로그

초콜렛

![Untitled](/images/0617/Untitled.png)

복사해서 붙임

![Untitled](/images/0617/Untitled%201.png)

POWERSHELL 검색에서 찾아서 복붙

헥소 사용을 위해 NODEJS 16.15.1로 다운로드

![Untitled](/images/0617/Untitled%202.png)

![Untitled](/images/0617/Untitled%203.png)

설치

깃허브 블로그 레퍼짓 생성

데스크에서 깃베시 

![Untitled](/images/0617/Untitled%204.png)

헥소 인 잇 블로그 설치

모든 파일 올리는 명령어

ls 파일 위치 확인

```
**echo "# blog" >> README.md
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:kimsunhyung/blog.git
git push -u origin main**
```

![Untitled](/images/0617/Untitled%205.png)

모두 입력

블로그 내용이 다 올라올 수 있도록

git add .

git commit -m”your_message”

git push -u origin main

작성

---

블로그 사이트 만들기

![Untitled](/images/0617/Untitled%206.png)

파이참으로 연결

좌측 메뉴 프로젝트로 변경

![Untitled](/images/0617/Untitled%207.png)

![Untitled](/images/0617/Untitled%208.png)

터미널 이동 후 깃 베시로 변경

![Untitled](/images/0617/Untitled%209.png)

npm install

npm install hexo-server --save

npm install hexo-deployer-git --save

입력하여 설치 후

hexo server 작성hex

숫자 1 옆에 있는 `를 세번 누르면 명령어가 생김

![Untitled](/images/0617/Untitled%2010.png)

localhost:4000을 클릭하면 사이트가 뜸

테스트 : localhost:4000

배포 : 어플(타인이 볼 수 있는 환경을 만들어 줌.)

---

배포

![Untitled](/images/0617/Untitled%2011.png)

레퍼짓 생성 시 이름과 깃허브쩜아이오작성

config.yml 더블 클릭

![Untitled](/images/0617/Untitled%2012.png)

![Untitled](/images/0617/Untitled%2013.png)

url 이름 변경

```
# Deployment
deploy:
  type: git
  repo: https://github.com/kimsunhyung/kimsunhyung.github.io.git
  branch: main
```

맨 아래에서 deploy에 작성한 후

![Untitled](/images/0617/Untitled%2014.png)

repo 이름 변경

터미널에 명령어가 작성이 되지 않으면 ctrl c 눌러야함

배포에 필요한 명령어

hexo generate

hexo deploy

작성하면 사이트완성

오타 확인 무조건 할 것

hexo server 시 나만 볼 수 있음 수정 후 타인에게 보여주고 싶을 때

hexo generate hexo deploy 작성

RTOOLS 42 설치

분석가가 되기 위해서 R 중요 → R은 ***통계***

→t.test, anova, 회귀분석, 로지스틱회귀 : 한학기

→챗봇 서비스 : 설문조사 만족도 조사 → 통계 분석

옵션에서 설정 변경

![Untitled](/images/0617/Untitled%2015.png)

CODE → SAVING → UTF -9

![Untitled](/images/0617/Untitled%2016.png)

레퍼짓으로 만들어서

바탕화면에 git clone 주소 복사를 하여 생성

![Untitled](/images/0617/Untitled%2017.png)

검색으로 r studio에 들어감

creat 클릭 후

existing directory 설치

browser로 바탕화면에 생긴 r lecture를 가져옴

![Untitled](/images/0617/Untitled%2018.png)

우측 상단에 project 중요

![Untitled](/images/0617/Untitled%2019.png)

소스, 문서, 데이터 폴더 추가

![Untitled](/images/0617/Untitled%2020.png)

후

![Untitled](/images/0617/Untitled%2021.png)

new file →

![Untitled](/images/0617/Untitled%2022.png)

저장 클릭 후 day 1저장

![Untitled](/images/0617/Untitled%2023.png)

cran →package →sorted by name →ggplot2

→referance pdf 확인

![Untitled](/images/0617/Untitled%2024.png)

ctrl enter를 쳐야함

![Untitled](/images/0617/Untitled%2025.png)

설치 후  library ()를 작성 패키지 내 함수들을 쓰겠다는 뜻.

![Untitled](/images/0617/Untitled%2026.png)

시각화

![Untitled](/images/0617/Untitled%2027.png)

help 모르는 함수 검색 →도움말

![Untitled](/images/0617/Untitled%2028.png)

시각화 진행

![Untitled](/images/0617/Untitled%2029.png)

cheat sheet로 들어가서 ggplot 2로 들어가서 마음에 드는 그래프를 선택

![Untitled](/images/0617/Untitled%2030.png)

알맞은 명령어 작성

set as 진행한 후

![Untitled](/images/0617/Untitled%2031.png)

r markdown 클릭설치후

![Untitled](/images/0617/Untitled%2032.png)

![Untitled](/images/0617/Untitled%2033.png)

제목 바꾼 후 시각화 knit 클릭

```
## ggplot2 시각화
- 다음과 같이 시각화를 작성한다.

```{r}
library(ggplot2)
ggplot(data = iris, aes(x = Sepal.Length,
                        y = Sepal.Width)) +
  geom_point()
```
작성 후
```

r 마크다운 교재 정리 및 코드실행

rmd 파일을 마크다운으로 변환

변환된 파일 → 블로그에 저장

blog폴더 source→ _post→ day1 md 올린 후 파이참을 킴

![Untitled](/images/0617/Untitled%2034.png)

파이참을 키고 난 후 기존 깃베시를 지우고 새롭게 깃베시를 킨 후 hexo server 입력

```
