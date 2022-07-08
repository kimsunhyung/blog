---
title: "0707_KAGGLE입문"
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

## 라이브러리 불러오기
- 주요 라이브러리 버전을 확인한다. 


```python
import pandas as pd 
import numpy as np 
import matplotlib as mpl 
import seaborn as sns 
import sklearn
import xgboost as xgb 
import lightgbm as lgb

print("pandas version :", pd.__version__)
print("numpy version :", np.__version__)
print("matplotlib version :", mpl.__version__)
print("seaborn version :", sns.__version__)
print("scikit-learn version :", sklearn.__version__)
print("xgboost version :", xgb.__version__)
print("lightgbm version :", lgb.__version__)
```

## 데이터 불러오기
- pandas 활용


```python
DATA_PATH = '/kaggle/input/house-prices-advanced-regression-techniques/'
train = pd.read_csv(DATA_PATH + "train.csv")
test = pd.read_csv(DATA_PATH + "test.csv")

print("데이터 불러오기 완료!")
```

## 데이터 둘러보기
- 데이터를 둘러봅니다. 
- train : 행 갯수 1460 열 갯수 81 (SalePrice 존재)
- test : 행 갯수 1459, 열 갯수 80 (SalePrice 컬럼 미 존재)


```python
train.shape, test.shape
```


```python
train.info()
```


```python
test.info()
```

## 데이터 시각화
- 여기에서는 생략
- 종속변수 분포 확인

- 샤피로 검정
- 정규분포인가요? 
    + 정규분포가 아님! --> 로그변환, 박스콕스 변환 등등
    + 정규분포로 만들어 줘야 함. 
- 선형모델의 성능을 올리기 위해서는 


```python
import matplotlib.pyplot as plt 
from scipy.stats import norm 
(mu, sigma) = norm.fit(train['SalePrice'])
print("평균:", mu)
print("표준편차:", sigma)

fig, ax = plt.subplots(figsize=(10, 6))
sns.histplot(train['SalePrice'])
ax.set(title="SalePrice Distribution")
ax.axvline(mu, color = 'r', linestyle = '--')
ax.text(mu + 10000, 160, 'Mean of SalePrice', color = 'r')
plt.show()
```

- 로그변환을 해서 정규분포로 변환해준다. 


```python
# 로그변환을 함. 
train['SalePrice'] = np.log1p(train['SalePrice'])

(mu, sigma) = norm.fit(train['SalePrice'])
print("평균:", mu)
print("표준편차:", sigma)

fig, ax = plt.subplots(figsize=(10, 6))
sns.histplot(train['SalePrice'])
ax.set(title="SalePrice Distribution")
ax.axvline(mu, color = 'r', linestyle = '--')
ax.text(mu + 0.0001, 160, 'Mean of SalePrice', color = 'r')
ax.set_ylim(0, 170)
plt.show()
```

## 데이터 전처리
- 컬럼 갯수가 많다?, 어떤 컬럼을 없앨 것인가? 
- 머신러닝 연산 속도부터 높여야 함. 


### 데이터 ID값 제거



```python
train_ID = train['Id']
test_ID = test['Id']

train = train.drop(['Id'], axis = 1)
train.shape
```


```python
test = test.drop(['Id'], axis = 1)
test.shape 
```

### Y값 추출
- train데이터에 SalePrice만 따로 저장한다.




```python
y = train['SalePrice']
train = train.drop('SalePrice', axis = 1)
train.shape
```


```python
test.shape
```

### 데이터 합치기
- 강의 목적
- 원칙
    + train, 따로 정리
    + test, 따로 정리
- Data Leakage 오류를 범할 가능성이 높음. 


```python
all_df = pd.concat([train, test]).reset_index(drop=True)
all_df.shape
```

## 결측치 확인
- 결측치의 비율 확인하는 사용자 정의 함수 작성


```python
train.info()
```


```python
def check_na(data, head_num = 6):
  isnull_na = (data.isnull().sum() / len(data)) * 100
  data_na = isnull_na.drop(isnull_na[isnull_na == 0].index).sort_values(ascending=False)
  missing_data = pd.DataFrame({'Missing Ratio' :data_na, 
                               'Data Type': data.dtypes[data_na.index]})
  print("결측치 데이터 컬럼과 건수:\n", missing_data.head(head_num))

check_na(all_df, 20)
```

- 결측치 제거
- 결측치 비율이 높은 변수들을 모두 제거하기로 했다. 


```python
all_df = all_df.drop(['PoolQC', 'MiscFeature', 'Alley', 'Fence', 'FireplaceQu', 'LotFrontage'], axis = 1)
print(all_df.shape)
check_na(all_df, 40)
```

## 결측치 채우기
- train 데이터와 test 데이터가 섞이면 안됨. 
- train / test 분리해서 진행해야 함. 
- 문자데이터 : 자주 등장하는 빈도 값으로 채움
- 숫자데이터 : 평균이 아니라, 중간값으로 채울 예정


```python
# all_df['SaleType'].value_counts()
all_df['SaleType'].mode()[0]
```


```python
check_na(all_df, 40)
```


```python
import numpy as np

# 문자열 데이터만 추출
cat_all_vars = train.select_dtypes(exclude=[np.number])
print("The whole number of all_vars", len(list(cat_all_vars)))

# 문자열 데이터 중에서 이미 기 삭제했던 Feature들이 있었기 때문에, 
# 한번 더 Feature를 정리하는 코드를 작성한다. 
# 따라서 38개의 Feature만 추출했다. 
final_cat_vars = []
for v in cat_all_vars:
    if v not in ['PoolQC', 'MiscFeature', 'Alley', 'Fence', 'FireplaceQu']:
        final_cat_vars.append(v)

print("The whole number of final_cat_vars", len(final_cat_vars))

# 이제 각 Feature 마다 빈도수가 가장 많이 나타나는 값을 추가하는 코드를 작성한다. 
for i in final_cat_vars:
  all_df[i] = all_df[i].fillna(all_df[i].mode()[0])

# 이제 수치형 데이터만 남은 것을 확인한다. 
check_na(all_df, 20)
```

- 수치형 데이터의 결측치를 추가할 수 있다. 
- 평균이 아닌 중간값으로 진행한다. 


```python
import numpy as np

# 방법은 기존과 동일하다. 
# 이번에는 수치형 데이터만 추출한다. 
num_all_vars = list(train.select_dtypes(include=[np.number]))
print("The whole number of all_vars", len(num_all_vars))

# 수치형 데이터 중, 결측치가 많았던 `LotFrontage`만 처리한다. 
num_all_vars.remove('LotFrontage')
print("The whole number of final_cat_vars", len(num_all_vars))

# 이번에는 수치형 데이터의 평균이 아닌 중간값을 지정했다. 
for i in num_all_vars:
  all_df[i].fillna(value=all_df[i].median(), inplace=True)

check_na(all_df, 20)
```

## 도출 변수
- 새로운 도출 변수를 작성 (기존 변수 활용)
- 기존 변수 제거 

- 각 층의 면적으로 모두 더해 전체 면적으로 계산한 새로운 변수를 작성한다. 


```python
all_df['TotalSF'] = all_df['TotalBsmtSF'] + all_df['1stFlrSF'] + all_df['2ndFlrSF']
all_df = all_df.drop(['TotalBsmtSF', '1stFlrSF', '2ndFlrSF'], axis=1)
print(all_df.shape)
```


```python
all_df['Total_Bathrooms'] = (all_df['FullBath'] + (0.5 * all_df['HalfBath']) + all_df['BsmtFullBath'] + (0.5 * all_df['BsmtHalfBath']))
all_df['Total_porch_sf'] = (all_df['OpenPorchSF'] + all_df['3SsnPorch'] + all_df['EnclosedPorch'] + all_df['ScreenPorch'])
all_df = all_df.drop(['FullBath', 'HalfBath', 'BsmtFullBath', 'BsmtHalfBath', 'OpenPorchSF', '3SsnPorch', 'EnclosedPorch', 'ScreenPorch'], axis=1)
print(all_df.shape)
```

- 연도와 관련된 변수를 추출하는 코드 작성. 


```python
num_all_vars = list(train.select_dtypes(include=[np.number]))
year_feature = []
for var in num_all_vars:
  if 'Yr' in var:
    year_feature.append(var)
  elif 'Year' in var:
    year_feature.append(var)
  else:  
    print(var, "is not related with Year")
print(year_feature)
```


```python
fig, ax = plt.subplots(3, 1, figsize=(10, 6), sharex=True, sharey=True)
for i, var in enumerate(year_feature):
  if var != 'YrSold':
    ax[i].scatter(train[var], y, alpha=0.3)
    ax[i].set_title('{}'.format(var), size=15)
    ax[i].set_ylabel('SalePrice', size=15, labelpad=12.5)
plt.tight_layout()
plt.show()
```


```python
all_df = all_df.drop(['YearBuilt', 'GarageYrBlt'], axis=1)
print(all_df.shape)
```


```python
YearsSinceRemodel = train['YrSold'].astype(int) - train['YearRemodAdd'].astype(int)

fig, ax = plt.subplots(figsize=(10, 6))
ax.scatter(YearsSinceRemodel, y, alpha=0.3)
plt.show()
```


```python
all_df['YearsSinceRemodel'] = all_df['YrSold'].astype(int) - all_df['YearRemodAdd'].astype(int)
all_df = all_df.drop(['YrSold', 'YearRemodAdd'], axis=1)
print(all_df.shape)
```

## 더미변수
- 더미변수란 원 데이터 독립변수를 0과 1로 변환하는 변수를 말함. 


```python
all_df['PoolArea'].value_counts() 
```

- 사용자 정의 함수 만들기


```python
def count_dummy(x):
    if x > 0:
        return 1
    else:
        return 0
```


```python
all_df['PoolArea'] = all_df['PoolArea'].apply(count_dummy) 
all_df['PoolArea'].value_counts()
```


```python
all_df['GarageArea'] = all_df['GarageArea'].apply(count_dummy)
all_df['GarageArea'].value_counts()
```


```python
all_df['Fireplaces'] = all_df['Fireplaces'].apply(count_dummy)
all_df['Fireplaces'].value_counts()
```

## 인코딩
- 문자를 숫자로 변환해주는 코드를 인코딩 변환



```python
all_df.shape
```

## Label Encoding, Ordinal Encoding, One-Hot Encoding
- 인코딩은 문자 데이터를 수치로 변환하는 방법론 중의 하나이다. 


```python
# 분류모형
# 종속변수 (양성, 음성)
# Label Encoder는 종속변수에만 적용
from sklearn.preprocessing import LabelEncoder
import pandas as pd

temp = pd.DataFrame({'Food_Name': ['Apple', 'Chicken', 'Broccoli'], 
                     'Calories': [95, 231, 50]})

encoder = LabelEncoder()
encoder.fit(temp['Food_Name'])
labels = encoder.transform(temp['Food_Name'])
print(list(temp['Food_Name']), "==>", labels)
```

- OrdinalEncoder는 독립변수에만 쓴다. 


```python
from sklearn.preprocessing import OrdinalEncoder
import pandas as pd

temp = pd.DataFrame({'Food_Name': ['Apple', 'Chicken', 'Broccoli'], 
                     'Calories': [95, 231, 50]})

encoder = OrdinalEncoder()
labels = encoder.fit_transform(temp[['Food_Name']])
print(list(temp['Food_Name']), "==>", labels.tolist())
```

- pandas 메서드 통해서 직접 숫자로 변환


```python
temp = pd.DataFrame({'Food_Name': ['Apple', 'Chicken', 'Broccoli'], 
                     'Calories': [95, 231, 50]})

temp['Food_No'] = temp['Food_Name'].replace(to_replace = ['Apple', 'Chicken', 'Broccoli'], value = [1, 2, 3])
print(temp[['Food_Name', 'Food_No']])
```

- 원핫 인코딩
    + scikit-learn 방식이 조금 복잡함. 
    + 그래서 보통은 pandas get_dummies() 함수를 활


```python
import pandas as pd
from sklearn.preprocessing import LabelBinarizer

temp = pd.DataFrame({'Food_Name': ['Apple', 'Chicken', 'Broccoli'], 
                     'Calories': [95, 231, 50]})

encoder = LabelBinarizer()
encoder.fit(temp['Food_Name'])
transformed = encoder.transform(temp['Food_Name'])
ohe_df = pd.DataFrame(transformed)
temp = pd.concat([temp, ohe_df], axis=1).drop(['Food_Name'], axis=1)
temp.columns = ['Calories', 'Food_Name_Apple', 'Food_Name_Broccoli', 'Food_Name_Chicken']
print(temp)
print(temp.shape)
```


```python
import pandas as pd

temp = pd.DataFrame({'Food_Name': ['Apple', 'Chicken', 'Broccoli'], 
                     'Calories': [95, 231, 50]})

temp = pd.get_dummies(temp)
print(temp)
print(temp.shape)
```

- 본 데이터 적용
    + 여기서는 Ordinal Encoding 적용 안함. (단, 실전에서는 꼭 찾아서 해야함). 
- 원핫 인코딩 적용


```python
all_df = pd.get_dummies(all_df).reset_index(drop=True)
all_df.shape
```


```python
all_df.head()
```

- train, test 데이터 합쳐서 진행. 
- train, test 데이터 재분리


```python
X = all_df.iloc[:len(y), :]
test = all_df.iloc[len(y):, :]

X.shape, y.shape, test.shape
```

- 머신러닝을 위한 데이터 전처리가 끝이 남. 

## 과제
- 남은 시간동안 교제를 보고 머신러닝 학습 및 RMSE 구하세요. 
- 데이터셋 분리
    + X 데이터를 X_train, X_test, y_train, y_test로 분리
   


```python
from sklearn.model_selection import train_test_split 
X_train, X_test, y_train, y_test = train_test_split(
    # 독립변수, 종속변수 
    X,         y, test_size = 0.3, random_state=0
)

X_train.shape, X_test.shape, y_train.shape, y_test.shape
```

## 평가지표
- MAE, MSE, RMSE 


### MAE 
- 실젯값과 예측값의 차이, 오차, 오차들의 절댓값 평균을 말함. 



```python
import numpy as np 

def mean_absolute_error(y_true, y_pred):
    error = 0 
    for yt, yp in zip(y_true, y_pred):
        # yt : 실젯값
        # yp : 예측값
        error = error + np.abs(yt - yp)
        # 절댓값 오차의 평균
    mae = error / len(y_true)
    return mae

def mean_squared_error(y_true, y_pred):
    error = 0 
    for yt, yp in zip(y_true, y_pred):
        # yt : 실젯값
        # yp : 예측값
        error = error + (yt - yp) ** 2
        # 제곱값 오차의 평균
    mse = error / len(y_true)
    return mse

def root_mean_squared_error(y_true, y_pred):
    error = 0 
    for yt, yp in zip(y_true, y_pred):
        # yt : 실젯값
        # yp : 예측값
        error = error + (yt - yp) ** 2
        # 제곱값 오차의 평균
    mse = error / len(y_true)
    
    # 제곱근 추가
    rmse = np.round(np.sqrt(mse), 3)
    return rmse

y_true = [400, 300, 800]
y_pred = [380, 320, 777]

print("MAE:", mean_absolute_error(y_true, y_pred))
print("MSE:", mean_squared_error(y_true, y_pred))
print("RMSE:", root_mean_squared_error(y_true, y_pred))
```

sklearn 참고 자료
- URL : https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html


```python
from sklearn.metrics import mean_squared_error
def rmse(y_true, y_pred):
    return np.sqrt(mean_squared_error(y_true, y_pred))
```

## 머신러닝 모형 정의, 검증 평가
- 교차 검증 함수 만들기



```python
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import KFold, cross_val_score
from sklearn.linear_model import LinearRegression
from sklearn.ensemble import RandomForestRegressor 
from lightgbm import LGBMRegressor
from xgboost import XGBRegressor

def cv_rmse(model, n_folds=5):
    cv = KFold(n_splits=n_folds, random_state=42, shuffle=True)
    rmse_list = np.sqrt(-cross_val_score(model, X, y, scoring='neg_mean_squared_error', cv=cv))
    print('CV RMSE value list:', np.round(rmse_list, 4))
    print('CV RMSE mean value:', np.round(np.mean(rmse_list), 4))
    return (rmse_list)

n_folds = 5
rmse_scores = {}
# lr_model = LinearRegression()
lgb_model = LGBMRegressor()
```


```python
score = cv_rmse(lgb_model, n_folds)
print("linear regression - mean: {:.4f} (std: {:.4f})".format(score.mean(), score.std()))
rmse_scores['linear regression'] = (score.mean(), score.std())
```

## 제출 방법
- 


```python
from sklearn.model_selection import cross_val_predict

# X = all_df.iloc[:len(y), :]
# X_test = all_df.iloc[len(y):, :]
# X.shape, y.shape, X_test.shape

lr_model_fit = lgb_model.fit(X_train, y_train)
final_preds = np.floor(np.expm1(lr_model_fit.predict(test)))
print(final_preds)
```


```python
submission = pd.read_csv(DATA_PATH + "sample_submission.csv")
submission.iloc[:,1] = final_preds
print(submission.head())
submission.to_csv("submission.csv", index=False)
```

## 모형 만들기



```python
# from sklearn.linear_model import LinearRegression
# lr_model = LinearRegression()
# lr_model.fit(X_train, y_train) 

# print(lr_model.score(X_train, y_train))
# print(lr_model.score(X_test, y_test))
```
