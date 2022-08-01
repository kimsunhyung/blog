---
title: "0801heroku배포"
date: '2022-08-01'
---

# 0801heroku배포

Created: 2022년 8월 1일 오전 9:12
Last Edited Time: 2022년 8월 1일 오후 2:05

[https://www.heroku.com/](https://www.heroku.com/) 

- 회원가입 후 local에 설치
- gmail 확인 후 accept를 누르고, 새로운 앱을 설치

[https://devcenter.heroku.com/articles/heroku-cli](https://devcenter.heroku.com/articles/heroku-cli)

- heroku cli 설치 64비트

---

- 설치 후 git rep를 새로 하나 만듬 heroku에서는 ‘_’ 불가능
- repo 생성 시 이름을 똑같이 생성하면 접속 시 오류가 생길 수 있음
- git-repo → 다운로드(local)→heroku 에 배포-
    - 이름이 동일하면 생성 불가
    
    ---
    
- heroku 앱 이름 생성 후 → 다운로드→github에 올림

![png](/images/0801heroku배포/Untitled.png)

repo 생성 시 git tempelte을  python으로 변경하여  add a readme를 클릭!

- 생성 후 http 코드를 복사하여 local 에 git clone 함

![png](/images/0801heroku배포/Untitled1.png)

- vs코드 생성 후 가상 환경을 설정한 후  Flask gunicorn을 설치

![png](/images/0801heroku배포/Untitled2.png)

![png](/images/0801heroku배포/Untitled3.png)

---

```python
pip install Flask gunicorn
```

```python
python.exe -m pip install --upgrade pip
```

---

```python
pip freeze > requirements.txt  
#라이브러리 생성 작성하는 공간을 생성
```

![png](/images/0801heroku배포/Untitled4.png)

[app.py](http://app.py) 파일 생성 후 저장하는 것 있지 말 것!!!! 제발!!!

![png](/images/0801heroku배포/Untitled5.png)

```python
# -*- coding:utf-8 -*-
from flask import Flask

app = Flask(__name__)

@app.route("/")
def index():
    return "Hello World!"
```

![png](/images/0801heroku배포/Untitled6.png)

![png](/images/0801heroku배포/Untitled7.png)

---

```python
export FLASK_APP=app
```

```python

export FLASK_ENV=development
```

```python
flask run
```

---

- 앱이 활성화 될 수 있게  만들어주는 명령어
- Procfile 생성 후

![png](/images/0801heroku배포/Untitled8.png)

```python
web: gunicorn wsgi:app 
```

- 명령어 작성
- Procfile 작성 완료 후 [wsgi.py](http://wsgi.py) 파일 생성

```python
from app import app

if __name__ == "__main__":
    app.run()
# 명령어 작성
```

![png](/images/0801heroku배포/Untitled9.png)

![png](/images/0801heroku배포/Untitled10.png)

- runtime 파일 생성 후   python 버전을 작성
- git hub repo 에 add, commit, push 명령어를 활용하여 올림.

![png](/images/0801heroku배포/Untitled11.png)

![png](/images/0801heroku배포/Untitled12.png)

- app.py에서 heroku login 명령어 작성 후 실행
- 아무 키를 누르고 난 후 로그인 진행

![png](/images/0801heroku배포/Untitled13.png)

- 로그인된 것을 확인하고 난 후

![png](/images/0801heroku배포/Untitled14.png)

heroku create 홈페이지 이름을 작성하여 진행

![png](/images/0801heroku배포/Untitled15.png)

gitpush heroku main을 작성하여 진행한 후 페이지에

---

app.py를 수정

```python
# -*- coding:utf-8 -*-
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index():
    return render_template('index.html')

if __name__ == "__main__":
    app.run(port=5000)
```

- templates 파일 생성 후 그 안에 index 파일 생성

![png](/images/0801heroku배포/Untitled16.png)

- 명령어 작성 후

![png](/images/0801heroku배포/Untitled17.png)

```python
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>FlaskBlog</title>
</head>
<body>
<h1>aaaaa</h1>
</body>
</html>
```

- git 명령어 작성하여 진행

```python

git add .
```

```python
git commit -m “updated”
```

```python
git push origin main
```

```python
git push heroku main
```

---

- temp폴더 완성 후
- static 폴더 생성 후 안에 css폴더를 생성한 후 style.css 파일 넣음

![png](/images/0801heroku배포/Untitled18.png)

- style.css 에 아래 명령어를 작성하고

![png](/images/0801heroku배포/Untitled19.png)

```css
h1 {
    border: 2px #eee solid;
    color: brown;
    text-align: center;
    padding: 10px;
}
```

- index.html파일에 meta와 title 사이에 아래 명령어를 작성

![png](/images/0801heroku배포/Untitled20.png)

```python
<link rel="stylesheet" href="{{ url_for('static', filename= 'css/style.css') }}">
```

- flask run으로 변경되었는지 확인
- 이렇게 뜨면 완성

![png](/images/0801heroku배포/Untitled21.png)

---

python 웹개발

- Django
    - 세팅할 것이 많음
- Flask
    - 초기에 개발하기 수월함
    - 세팅할 것이 많지 않음
    - REST API가 있어야만,  카카오톡 챗봇이 연동이 됨
    - REST API = @app.route 와 같음