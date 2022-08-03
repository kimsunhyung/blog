---
title: "0802webserver만들기"
date: "2022-08-02"
---


# 0802webserver만들기


- web_development 폴더 생성 후 vs code 접속
- 확장에서 korean(사용법)이 설치되어 있는지 확인

![](/images/0802webserver)

- beautify 자동 정렬해주는 확장팩을 설치하면 java 시 편리함.
- auto rename 확장팩 설치(태그하기 편리함)

![](/images/0802webserver/Untitled1.png)

- live server 확장팩 설치

![](/images/0802webserver/Untitled2.png)

- 확장팩 설치 후 index.html 파일 생성

![](/images/0802webserver/Untitled3.png)

- utf-8 인코딩 버전을 의미함
- 기본 세팅 코드

![](/images/0802webserver/Untitled4.png)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8"> 
        <title>human title</title>
    </head>
    <body>

    </body>
</html>
```

![](/images/0802webserver/Untitled5.png)

- GO live를 누르게 되면 human title 싸이트가 열림.

---

![](/images/0802webserver/Untitled6.png)

- h1 - h6 ****는 6단계의 구획 제목을 나타냅니다. 구획 단계는 h1이 가장 높고 h6은 가장 낮습니다.

![](/images/0802webserver/Untitled7.png)

- p 태그는 paragraph, 즉 문단의 약자로, 하나의 문단을 만들 때 쓰임

![](/images/0802webserver/Untitled8.png)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8"> 
        <title>human title</title>
    </head>
    <body>
        <p>this is a paragraph</p>

    </body>
</html>
```

![](/images/0802webserver/Untitled9.png)

![](/images/0802webserver/Untitled10.png)

- lang = “en” 번역을 옵션

![](/images/0802webserver/Untitled11.png)

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8"> 
        <title>human title</title>
    </head>
    <body>
        Macine Learning, Web Development
    </body>
</html>
```

- <br/> 띄어쓰기

![](/images/0802webserver/Untitled12.png)

![](/images/0802webserver/Untitled13.png)

- pre 태그
- 미리 정의된 형식(preformatted)의 텍스트를 정의할 때 사용합니다.
- 요소 내의 텍스트는 시스템에서 미리 지정된 고정폭 글꼴(fixed-width font)을 사용하여 표현되며, 텍스트에 사용된 여백과 줄바꿈이 모두 그대로 브라우저 화면에 나타납니다.
- 독특한 서식의 텍스트나 컴퓨터 코드 등을 HTML 문서에 그대로 표현할 수 있습니다.

---

- a href 링크를 달아서 site로 이동할 수 있도록 함.

```html
<a href="https://www.naver.com">네이버</a>
```

ctrl + , → word wrap을 친 후  editor를 off 에서 on으로 변경하면 자동 줄 바꿈이 됨

![](/images/0802webserver/Untitled14.png)

- 이미지 불러오기

```html
<image src="https://shopv.pstatic.net/web/cnsv/iu/cnsv/cnts_prod/22/0726/mpnva2lcjf.jpg", width = "500",height = "1000"></image>
```

- 리스트 만들기

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8"> 
        <title>human title</title>
    </head>
    <body>

        <hl>List를 만들자.</hl>
        <ul>
            <li>책읽기</li>
            <li>산책하기</li>
        </ul>
        <br/>
            <ol type="a">
                <li>책읽기</li>
                <li>산책하기</li>
            </ol>
            <ol type="1">
                <li>책읽기</li>
                <li>산책하기</li>
            </ol>
    </body>
</html>
```

![](/images/0802webserver/Untitled15.png)

---

- css 꾸미기
    - 보통 다이렉트로 body에서 꾸미기도 하지만, head에서 공통적으로 꾸미기도 함.
    
    ```html
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8"> 
            <title>human title</title>
            <style>
                table, th, td{
                    border :1px solid black;
                    border-spacing: 1ch;}
            </style>
        </head>
        <body>
            <table>
                <tr>
                    <th>A</th>
                    <th>B</th>
                </tr>
                <tr>
                    <th>홍길동</th>
                    <th>둘리</th>
                </tr>
            </table>
       
        </body>
    
    </html>
    ```
    
    ![](/images/0802webserver/Untitled16.png)
    
- 마음에 드는 꾸미기 코드를 복사해서 붙여 넣어도 좋음.
- css 폴더를 만들고, 그안에 style.css 파일을 넣은 후 꾸미기 코드를 넣음.
- 코드 작성 시

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8"> 
	<title>HTML Table Generator</title> 
	<link rel="stylesheet", href="css/style.css">
```

- 이렇게 작성하면 모든 바디에 내가 style파일에 넣어둔 꾸미기 코드가 적용됨.
- 부트스트랩 [https://getbootstrap.kr/](https://getbootstrap.kr/)을 활용
- 부트스트랩 스타터 템플릿을 복사하여 진행하면 페이지 시작하는데 도움이 됨.

![](/images/0802webserver/Untitled17.png)

- 부트 스트랩에서 제공하는 다양한 것들을 삽입하여 홈페이지 꾸미기를 진행할 수 있음.

---

## 8/3

- 새폴더 생성 후
- iris.csv파일 생성
- 가상환경 접속

![](/images/0802webserver/Untitled18.png)

- sklearn, django, jupyterlab, pandas 설치

![](/images/0802webserver/Untitled19.png)

- 주피터 랩 실행 후 model.ipynb파일 생성

![](/images/0802webserver/Untitled20.png)

파일을 잘 만들고 난 후 저장 후 종료

---

- django 배포 관련 내용 자세하게 나와있음.

[https://dschloe.github.io/python/django/django_iris_sklearn/](https://dschloe.github.io/python/django/django_iris_sklearn/)