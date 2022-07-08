---
title: "0708_KAGGLE입문"
date: '2022-07-08'
---

```python
# This Python 3 environment comes with many helpful analytics libraries installed
# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python
# For example, here's several helpful packages to load

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)

# Input data files are available in the read-only "../input/" directory
# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

# You can write up to 20GB to the current directory (/kaggle/working/) that gets preserved as output when you create a version using "Save & Run All" 
# You can also write temporary files to /kaggle/temp/, but they won't be saved outside of the current session
```

## step 01. 필수 라이브러리 불러오기
- 주석


```python
import pandas as pd # 데이터 가공
import numpy as np # 수치 연산
import matplotlib as mpl # 시각화 
import matplotlib.pyplot as plt 
import seaborn as sns # 시각화 
import sklearn # 머신러닝

# 버전 확인
print("pandas version :", pd.__version__)
print("numpy version :", np.__version__)
# matplotlib
# sklearn
# seaborn
```

## step 02. 데이터 불러오기
- 데이터를 불러온다. 


```python
DATA_PATH = '/kaggle/input/bike-sharing-demand/'

train = pd.read_csv(DATA_PATH + 'train.csv') # 훈련 데이터
test = pd.read_csv(DATA_PATH + 'test.csv')   # 테스트 데이터
submission = pd.read_csv(DATA_PATH + 'sampleSubmission.csv') # 제출 샘플 데이터

train.shape, test.shape, submission.shape
```

## step 03. 데이터 확인하기


```python
train.info()
```


```python
test.info()
```

- 현재 판단으로는 결측치가 없어 보임. 

## step 03. 탐색적 자료 분석
- 시각화
- 날짜 기반
- train 다이렉트로 변화를 주면 전처리 시 헷갈림
- 복제본 뜬다. (탐색적 자료 분석)
- 데이터 셋 매우 작음
    + 전체를 다 써도 큰 상관 없음
    + 전체 데이터에서 일부 샘플링 함


```python
temp_df = train.copy()
temp_df.info()
```

- 시각화를 위한 날짜 데이터 처리
- 연도, 월, 일자, 시간, 분, 초



```python
temp_df['datetime']
```


```python
print(temp_df['datetime'][0].split()[1])
print(temp_df['datetime'][100].split()[0].split('-')[2])
```


```python
year = temp_df['datetime'][100].split()[0].split('-')[0]
month = temp_df['datetime'][100].split()[0].split('-')[1]
day = temp_df['datetime'][100].split()[0].split('-')[2]

year, month, day
```


```python
hour = temp_df['datetime'][2].split()[1].split(':')[0]
minutes = temp_df['datetime'][2].split()[1].split(':')[1]
seconds = temp_df['datetime'][2].split()[1].split(':')[2]

hour, minutes, seconds
```


```python
temp_df['datetime'][0].split()[0]
```

- 시간 데이터 전처리 방법론 1. 


```python
import time 
import datetime 

# 시간 테스트 
start_time = time.time()

temp_df['date'] = temp_df['datetime'].apply(lambda x : x.split()[0])
temp_df['year'] = temp_df['datetime'].apply(lambda x : x.split()[0].split('-')[0])
temp_df['month'] = temp_df['datetime'].apply(lambda x : x.split()[0].split('-')[1])
temp_df['day'] = temp_df['datetime'].apply(lambda x : x.split()[0].split('-')[2])
temp_df['hour'] = temp_df['datetime'].apply(lambda x : x.split()[1].split(':')[0])

end_time = time.time() 
lambda_ctime = end_time - start_time

print("실행시간 (second) -> ", np.round(lambda_ctime, 3))
temp_df[['datetime', 'year', 'month', 'day', 'hour']]
```

- 시간 데이터 방법론 전처리 2


```python
temp_df['date'] = pd.to_datetime(temp_df['datetime'])
temp_df['year'] = temp_df['date'].dt.year

```


```python
import time 
import datetime 

# 시간 테스트 
start_time = time.time()

temp_df['date'] = pd.to_datetime(temp_df['datetime'])
temp_df['year'] = temp_df['date'].dt.year
temp_df['month'] = temp_df['date'].dt.month
temp_df['day'] = temp_df['date'].dt.day
temp_df['hour'] = temp_df['date'].dt.hour

end_time = time.time() 
dt_ctime = end_time - start_time

print("실행시간 (second) -> ", np.round(dt_ctime, 3))

temp_df[['datetime', 'year', 'month', 'day', 'hour']]
```


```python
temp_df['date']
```

- 요일 추출하기


```python
temp_df['date']
```


```python
temp_df['weekday'] = temp_df['date'].dt.day_name()
temp_df['weekday']
```


```python
temp_df['season'] = temp_df['season'].map({
    1: 'Spring', 
    2: 'Summer', 
    3: 'Fall', 
    4: 'Winter'
})

temp_df['season']
```


```python
temp_df['weather'] = temp_df['weather'].map({
    1 : 'Clear', 
    2 : 'Few clouds', 
    3 : 'Light Snow, Rain', 
    4 : 'Heavy Snow, Rain'
})

temp_df['weather']
```


```python
temp_df.head()
```

## step 04. 데이터 시각화
- 수치를 예측하는 대회
- 종속 변수를 시각화 해야 함. 
- 분포 확인 후, 로그변환을 줄지 안줄지 결정 해야 함. 


```python
fig, ax = plt.subplots(nrows = 1, ncols = 2)

sns.histplot(train['count'], ax = ax[0]) 
sns.histplot(np.log(train['count']), ax = ax[1])

# 옵션 제목
ax[0].set_title('Normal Graph')
ax[1].set_title("Log Transformed Graph")

plt.show()
```

- 막대 그래프
    + year, count 
    + month, count
    + day, count 
    + hour, count
- 각 그래프의 타이틀 추가 


```python
fig, ax = plt.subplots(nrows = 2, ncols = 2)

## 1단계 : 전체 그래프 기본 설정
# 그래프 사이 간격
fig.tight_layout()

# 전체 그래프 사이즈 관리
fig.set_size_inches(10, 9)

## 2단계 :  각 개별 그래프 입력
sns.barplot(x = 'year', y = 'count', data = temp_df, ax=ax[0,0])
sns.barplot(x = 'month',y = 'count', data = temp_df, ax=ax[0,1])
sns.barplot(x = 'day', y = 'count', data = temp_df, ax=ax[1,0])
sns.barplot(x = 'hour', y = 'count', data = temp_df, ax=ax[1,1])

## 3단계 : 디테일 옵션
ax[0, 0].set_title("Rental Amounts by Year")
ax[0, 1].set_title("Rental Amounts by month")
ax[1, 0].set_title("Rental Amounts by day")
ax[1, 1].set_title("Rental Amounts by hour")

ax[0, 0].tick_params(axis = 'x', labelrotation=90)

plt.show()
```

- boxplot
    + season, count
    + weather, count
    + holiday, count
    + workingday, count


```python
fig, ax = plt.subplots(nrows = 2, ncols = 2)

## 1단계 : 전체 그래프 기본 설정
# 그래프 사이 간격
fig.tight_layout()

# 전체 그래프 사이즈 관리
fig.set_size_inches(10, 9)

## 2단계 :  각 개별 그래프 입력
sns.boxplot(x = 'season', y = 'count', data = temp_df, ax=ax[0,0])
sns.boxplot(x = 'weather',y = 'count', data = temp_df, ax=ax[0,1])
sns.boxplot(x = 'holiday', y = 'count', data = temp_df, ax=ax[1,0])
sns.boxplot(x = 'workingday', y = 'count', data = temp_df, ax=ax[1,1])

## 3단계 : 디테일 옵션
ax[0, 0].set_title("Box Plot On Count Across Season")
ax[0, 1].set_title("Rental Amounts by month")
ax[1, 0].set_title("Rental Amounts by day")
ax[1, 1].set_title("Rental Amounts by hour")

ax[0, 1].tick_params(axis = 'x', labelrotation=20)

plt.show()
```

- 포인트 플롯
- 5개의 행 그래프 작성
    + workingday, holiday, weekday, season, weather
 - 5개의 그래프를 한 이미지로 그리세요. 


```python
fig, ax = plt.subplots(nrows = 5)
fig.set_size_inches(12, 18)

sns.pointplot(x = 'hour', y = 'count', hue = 'workingday', data = temp_df, ax = ax[0])
sns.pointplot(x = 'hour', y = 'count', hue = 'holiday', data = temp_df, ax = ax[1])
sns.pointplot(x = 'hour', y = 'count', hue = 'weekday', data = temp_df, ax = ax[2])
sns.pointplot(x = 'hour', y = 'count', hue = 'season', data = temp_df, ax = ax[3])
sns.pointplot(x = 'hour', y = 'count', hue = 'weather', data = temp_df, ax = ax[4])

plt.show()
```

- 회귀선을 포함한 산점도 그래프
    + x, y 모두 수치형 그래프여야 함. 
    + 연속형 수치형 데이터야 함. 
- 총 4개의 그래프가 나와야 함. 


```python
temp_df.info()
```


```python
temp_df['windspeed'].value_counts()
```


```python
fig, ax = plt.subplots(nrows=2, ncols =2)

fig.tight_layout()

sns.regplot(x ='temp', y ='count', data = temp_df, scatter_kws = {'alpha' : 0.2}, line_kws = {'color' : 'blue'}, ax = ax[0, 0])
sns.regplot(x ='atemp', y ='count', data = temp_df, scatter_kws = {'alpha' : 0.2}, line_kws = {'color' : 'blue'}, ax = ax[0, 1])
sns.regplot(x ='humidity', y ='count', data = temp_df, scatter_kws = {'alpha' : 0.2}, line_kws = {'color' : 'blue'}, ax = ax[1, 0])
sns.regplot(x ='windspeed', y ='count', data = temp_df, scatter_kws = {'alpha' : 0.2}, line_kws = {'color' : 'blue'}, ax = ax[1, 1])
plt.show()
```

- 히트맵 그래프 그리기
- 상관계수 해석
    + 수치가 양수다 (양의 관계)
    + 수치가 음수다 (음의 관계)
    + 0~0.2 : 두 변수 사이의 상관관계는 없다. 
    + 0.2 ~ 1: 값이 커지면 커질수록 두 변수 간 상관관계는 크다. 
    


```python
corrMat = temp_df[['temp', 'atemp', 'humidity', 'windspeed', 'count']].corr()
corrMat
```


```python
sns.heatmap(corrMat, annot=True)
```

## 데이터 전처리
- 첫번째 : train 데이터의 causal, registered 컬럼 제거할 것임
- 두번째 : 날짜 데이터 처리, dt.month, 이렇게 할 것
- 세번째 : season 컬럼 처리 필요 (숫자를 문자로 바꾸자)
    + 인코딩 변환 (라벨인코딩, 원핫 인코딩) 
- 네번째 : weather 컬럼 처리 필요 (숫자를 문자로 바꾸자)
    + 인코딩 변환 (라벨인코딩, 원핫 인코딩)
- 다섯번째 : month, day 컬럼 삭제 예정
- 여섯번째 : weather 4인 데이터는 삭제 (이상치)
- 일곱번째 : windspeed 컬럼 삭제 
- 여덟번째 : temp, atemp중 하나 삭제 (옵션)
- 마지막 : 모든 문자를 숫자로 인코딩 (원-핫 인코딩)

### 첫번째
- train 데이터의 causal, registered 컬럼 제거


```python
train.info()
```


```python
train = train.drop(['casual', 'registered'], axis = 1)
train.shape, test.shape
```

## weather 컬럼 지우기
- 4인 데이터는 삭제


```python
train = train[train['weather'] != 4].reset_index(drop=True)
train.shape
```

### 데이터 합치기
- train, test 데이터 합치기


```python
all_data = pd.concat([train, test], ignore_index=True)
all_data.info()
```

### 두번째 & 다섯번째 
- 날짜 데이터 처리, dt.month, 이렇게 할 것
- month, day 컬럼 삭제 예정


```python
all_data['date'] = pd.to_datetime(all_data['datetime'])
all_data['year'] = all_data['date'].dt.year
all_data['hour'] = all_data['date'].dt.hour
all_data['weekday'] = all_data['date'].dt.day_name()

all_data.shape
```


```python
all_data.info()
```

### 세번째 
- season 컬럼 처리 필요 (숫자를 문자로 바꾸자)
    + 인코딩 변환 (라벨인코딩, 원핫 인코딩) 


```python

```


```python
all_data['season'] = all_data['season'].map({
    1: 'Spring', 
    2: 'Summer', 
    3: 'Fall', 
    4: 'Winter'
})

all_data.shape
```


```python
all_data['season']
```

### 네번째
- weather 컬럼 처리 필요 (숫자를 문자로 바꾸자)
    + 인코딩 변환 (라벨인코딩, 원핫 인코딩)



```python
all_data['weather'] = all_data['weather'].map({
    1 : 'Clear', 
    2 : 'Few clouds', 
    3 : 'Light Snow, Rain', 
    4 : 'Heavy Snow, Rain'
})

all_data.shape
```


```python
all_data['weather']
```

### 다섯번째
- weather 4인 데이터는 삭제 (이상치)
    + 4 대신, 'Heavy Snow, Rain'


```python
all_data.shape
```


```python
# all_data['weather'].value_counts()
```

- all_data에서 지우는 것이 아니라, train에서만 지워야 함.


```python
# all_data = all_data[all_data['weather'] != 'Heavy Snow, Rain']
# all_data.shape
```

## 일곱번째 
- windspeed 컬럼 삭제
- train만 제거


```python
all_data = all_data.drop('windspeed', axis = 1)
all_data.shape
```


```python
all_data.info()
```

### date 컬럼 제거



```python
all_data.info()
```


```python
all_data = all_data.drop(['datetime', 'date'], axis = 1)
all_data.shape
```

### 원-핫 인코딩
- 


```python
all_data = pd.get_dummies(all_data).reset_index(drop=True)
all_data.shape
```


```python
all_data.info()
```

## 데이터셋 나누기
- 훈련데이터와 테스트 데이터로 재 나누기
- count 타깃 데이터(=종속 데이터)
    + 타깃 데이터가 있으면 훈련 데이터
    + 타깃 데이터가 없으면 테스트 데이터


```python
train = all_data[~pd.isnull(all_data['count'])]
test = all_data[pd.isnull(all_data['count'])]

train.shape, test.shape
```

- count 컬럼을 제거해야 함
- 타깃 데이터만 y로 추출
    + train, test count 컬럼 제거


```python
y = train['count'] # 타깃값 

train = train.drop(['count'], axis = 1)
test = test.drop(['count'], axis = 1)

train.shape, test.shape
```

## 모델 훈련
- LinearRegression 모형만 학습


```python
from sklearn.linear_model import LinearRegression
# train_test_split

lr_model = LinearRegression()

# 모형 학습 전
# 로그변환을 해준다. 
log_y = np.log(y)
lr_model.fit(train, log_y)

# 모형 예측
lr_preds = lr_model.predict(test)
lr_preds[:10]
```

## 모형 예측


```python
# 지수변환
final_preds = np.exp(lr_preds)
final_preds
```

## 제출


```python
submission['count'] = final_preds 
submission.to_csv('submission.csv', index=False)
```


```python
submission.head()
```


```python

```


```python

```


```python

```


```python
# all_data.info()
```
