---
title: "0729_pyspark_postgreSQL"
date: '2022-07-29'
---

# 0729 pyspark & postgreSQL

Created: 2022년 7월 29일 오전 9:10
Last Edited Time: 2022년 7월 29일 오후 3:03

![png](/images/0729pyspark_postgreSQL/1.png)

- 경로 이동 하여 데이터 복제하기!
- pyspark는 버전을 항상 맞춰주어야 함.

![png](/images/0729pyspark_postgreSQL/2.png)

- 새로운 버전을 깔기위해

![png](/images/0729pyspark_postgreSQL/3.png)

- 알맞은 버전을 설치

![png](/images/0729pyspark_postgreSQL/4.png)

- 설치 후 주피터 노트북 설정
- 주피터 노트북이 런이 돌아 가지 않을 시
- kernel에서 shutdown을 하고 비주얼 코드로 돌아와서 주피터를 켰던 터미널에서도 ctrl c를 두번 클릭하여 셧 다운을 하고 새롭게 다시 시작

---

# postgreSQL 설치

- db 세팅 시 필요함.
- ubuntu 실행 후 home에서 설치 실행

![png](/images/0729pyspark_postgreSQL/5.png)

![png](/images/0729pyspark_postgreSQL/6.png)

---

- 설치가 원활히 이루어지지 않을 시 -

![png](/images/0729pyspark_postgreSQL/7.png)

- 이 방법으로 설치 가능

![png](/images/0729pyspark_postgreSQL/8.png)

- 설치 후 버전 확인은 필수

![png](/images/0729pyspark_postgreSQL/9.png)

- 설치 후 데몬을 실행 하고, 패스워드를 설정한 후 |q를 활용하여 빠져 나온다.
- pgADmin 4 패키지가 있는 저장소를 apt에 추가한 후 설치

![png](/images/0729pyspark_postgreSQL/10.png)

![png](/images/0729pyspark_postgreSQL/11.png)

![png](/images/0729pyspark_postgreSQL/12.png)

- 이메일 비밀번호를 설정한다
    
    ![png](/images/0729pyspark_postgreSQL/13.png)
    

[https://www.pgadmin.org/download/pgadmin-4-apt/](https://www.pgadmin.org/download/pgadmin-4-apt/) 코드 관련 

---

![png](/images/0729pyspark_postgreSQL/14.png)

![png](/images/0729pyspark_postgreSQL/15.png)

![png](/images/0729pyspark_postgreSQL/16.png)

![png](/images/0729pyspark_postgreSQL/17.png)

![png](/images/0729pyspark_postgreSQL/18.png)

• vi 편집기로 아래를 수정합니다. (주석 처리가 되어 있을 겁니다.) (종료시 :wq!)

![png](/images/0729pyspark_postgreSQL/19.png)

![png](/images/0729pyspark_postgreSQL/20.png)

- 웹사이트가 활성화됨
- [http://localhost/pgadmin4/](http://localhost/pgadmin4/)로 접속하여 설치 시 설정했던 이메일, 비밀번호 설정
- 이메일 비밀번호를 치고 사이트에 들어가서

![png](/images/0729pyspark_postgreSQL/21.png)

- add new server를 클릭

![png](/images/0729pyspark_postgreSQL/22.png)

- 이름, 유저네임, 비밀번호 작성 후 genaral에서 이름을 작성한 후 서버를 구축
- ubuntu 종료 시 sudo service postgresql stop명령어를 실행한 후 종료해야 함

---

- 관련 코드

```python
sudo service postgresql stop
```

```python
sudo apt-get -y upgrade
```

```python
sudo apt-get install postgresql postgresql-contrib
```

```
sudo service postgresql stop
sudo -b unshare --pid --fork --mount-proc /lib/systemd/systemd --system-unit=basic.target
sudo -E nsenter --all -t $(pgrep -xo systemd) runuser -P -l $USER -c "exec $SHELL"
```

```python
sudo vi /etc/postgresql/12/main/postgresql.conf
```

```python
sudo /usr/pgadmin4/bin/setup-web.sh
```