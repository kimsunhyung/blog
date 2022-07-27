---
title: "0727 airflow설치"
date: '2022-07-27'
---


# 아파치 에어플로 설치 및 설정

Created: 2022년 7월 27일 오후 2:06
Last Edited Time: 2022년 7월 27일 오후 3:21

- 설치하는 이유 (스케줄 관리 등)

![png](/images/0727_airflow설치/Untitled.png)

- 가상환경 설정 후 apache-airflow 설치

pip3 install 'apache-airflow[postgres, slack, celery]’ (pip버전에 따라 코드 다름)

- 설치 후

 airflow db init 코드 입력 

![png](/images/0727_airflow설치/Untitled1.png)

- 입력 후 활성화 됨
- 웹에서 [localhost:8081](http://localhost:8081) 입력하면 sign in이 뜸
- 종료 후 아이디 생성
- `airflow users create --username airflow --password airflow --firstname evan --lastname airflow --role Admin --email your_email@some.com` 코드 작성
- 아이디 패스워드 입력하면 접속 완료

---

- 환경이 설정 되지 않을 경우

![png](/images/0727_airflow설치/Untitled2.png/)

- 가상환경을 제거하고
- vi ~/.bashrc 설정 후
- d를 두번 클릭하여 export절을 삭제한 후  :wq!로 저장한 후

![png](/images/0727_airflow설치/Untitled3.png)

- 모든 프로그램을 종료 하고 처음부터 시작

![png](/images/0727_airflow설치/Untitled4.png)

![png](/images/0727_airflow설치/Untitled5.png)

- 가상환경 설정이 안될경우
- sudo apt install python3.8-venv
- python -m venv venv 를 작성

---
