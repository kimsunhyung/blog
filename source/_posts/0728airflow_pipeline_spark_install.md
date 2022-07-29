---
title: "0728_airflow pipeline 구축 및 spark 설치"
date: '2022-07-28'
---

# 0728 airflow 파이프라인 구축 및 스파크 설치


![png](/images/0728airflow_pipeline_spark_install/1.png)

- 경로 이동 방법
- cat 자신이 작성한 코드를 보여줌

![png](/images/0728airflow_pipeline_spark_install/2.png)

![png](/images/0728airflow_pipeline_spark_install/3.png)

- 1000개의가짜데이터 형성
- 라이브러리를 불러올 때 파일 명과 동일하게 생성하면 라이브러리가 제대로 불러오지 못함.

# airflow  파이프라인 구축

- airflow.cfg 로 들어가서 load_example을 True에서 False로 변경

![png](/images/0728airflow_pipeline_spark_install/4.png)

- 라이브러리를 불러온다.

---

```jsx
import datetime as dt
from datetime import timedelta
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator

import pandas as pd
```

---

- csv 파일을 읽어서 name을 추출하는 함수를 작성
- csv파일에서 데이터를 읽어 들이는 방법과 데이터를 json파일로 기록하는 방법을 조합

---

```jsx
def csvToJson():
df=pd.read_csv('dags/data.csv')
for i,r in df.iterrows():
print(r['name'])
df.to_json('fromAirflow.json',orient='records')
```

---

- 사전 객체로 dag 호출 후 객체 생성
- default_args 매개변수 앞에서는 설정한 사전 객체 지정 후
- 실행 사이 간격을 지정한다
- timedelta, crontab으로 사용가능

---

```python
default_args = {
    'owner': 'evan',
    'start_date': dt.datetime(2022, 7, 27),
    'retries': 1,
    'retry_delay': dt.timedelta(minutes=1),
}
```

---

- airflow webserver -p 포트번호
- airflow scheduler

---

---

# 스파크 설치

- 스파크를 사용하는 이유

![png](/images/0728airflow_pipeline_spark_install/5.png)

- 
- ubuntu 열기 후

![png](/images/0728airflow_pipeline_spark_install/6.png)

- 경로 설정 후 파일 생성 후

```jsx
sudo apt-get install openjdk-8-jdk
```

- 코드 입력 후

```jsx
sudo wget [https://archive.apache.org/dist/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz](https://archive.apache.org/dist/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz)
```

- 코드 입력

```jsx
# 압축풀기
sudo tar -xvzf spark-3.2.0-bin-hadoop3.2.tgz
```

- 압축을 풀고 난 후 파일 생성

![png](/images/0728airflow_pipeline_spark_install/7.png)

![png](/images/0728airflow_pipeline_spark_install/8.png)

![png](/images/0728airflow_pipeline_spark_install/9.png)

- 파일을 지우는 명령어

![png](/images/0728airflow_pipeline_spark_install/10.png)

![png](/images/0728airflow_pipeline_spark_install/11.png)

- 복사코드

![png](/images/0728airflow_pipeline_spark_install/12.png)

![png](/images/0728airflow_pipeline_spark_install/13.png)

![png](/images/0728airflow_pipeline_spark_install/14.png)

![png](/images/0728airflow_pipeline_spark_install/15.png)

![png](/images/0728airflow_pipeline_spark_install/16.png)

한 후 localhost:8080을 웹에 작성하면 

![png](/images/0728airflow_pipeline_spark_install/17.png)

이 화면이 나옴.

환경변수 = 해당 파일이 있는지 없는지 확인하는 경로

![png](/images/0728airflow_pipeline_spark_install/18.png)

![png](/images/0728airflow_pipeline_spark_install/19.png)

![png](/images/0728airflow_pipeline_spark_install/20.png)

배포 후

HOME으로 이동한 후

![png](/images/0728airflow_pipeline_spark_install/21.png)

```python
# 환경변수 설정
export AIRFLOW_HOME=/home/human/airflow
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export SPARK_HOME=/home/human/spark3
export PATH=$JAVA_HOME/bin:$PATH
export PATH=$SPARK_HOME/bin:$PATH
export PYSPARK_PYTHON=/usr/bin/python3
```

- 환경 변수 설정 후 배포 한 후
- ubuntu 를 종료 하고 다시 실행하여
- pyspark 코드를 입력 하면 활성화됨

---

![png](/images/0728airflow_pipeline_spark_install/22.png)

가상환경 설정 후 3개의 파인스파크 파이스파크를 설치한 후 파이 스파크 활성화

![png](/images/0728airflow_pipeline_spark_install/23.png)

환경변수 추가