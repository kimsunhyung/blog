---
title: "0701logic회귀"
date: '2022-07-01'
---

## 로지스틱 회귀
 - 선형 회귀에서 출발
 - 이진 분류 문제 해결
 - 클래스 확률 예측
 - 딥러닝에서도 사용됨

## 교재 177

- X가 사각형일 확률 30%
- X가 삼각형일 확률 50%
- X가 원일 확률 20%

## 데이터 불러오기
- SPECIES(종속변수=Y)
- 독립변수 - weight, length, diagonal, height, width


```python
import pandas as pd

fish = pd.read_csv('https://bit.ly/fish_csv_data')
fish.head()
```





  <div id="df-24bfc3ca-101f-423d-aebf-5566df7b8830">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Species</th>
      <th>Weight</th>
      <th>Length</th>
      <th>Diagonal</th>
      <th>Height</th>
      <th>Width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bream</td>
      <td>242.0</td>
      <td>25.4</td>
      <td>30.0</td>
      <td>11.5200</td>
      <td>4.0200</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bream</td>
      <td>290.0</td>
      <td>26.3</td>
      <td>31.2</td>
      <td>12.4800</td>
      <td>4.3056</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bream</td>
      <td>340.0</td>
      <td>26.5</td>
      <td>31.1</td>
      <td>12.3778</td>
      <td>4.6961</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bream</td>
      <td>363.0</td>
      <td>29.0</td>
      <td>33.5</td>
      <td>12.7300</td>
      <td>4.4555</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bream</td>
      <td>430.0</td>
      <td>29.0</td>
      <td>34.0</td>
      <td>12.4440</td>
      <td>5.1340</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-24bfc3ca-101f-423d-aebf-5566df7b8830')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-24bfc3ca-101f-423d-aebf-5566df7b8830 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-24bfc3ca-101f-423d-aebf-5566df7b8830');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>




## 데이터 탐색


```python
# 종속변수 확인
print(pd.unique(fish['Species']))
print(fish['Species'].value_counts())
```

    ['Bream' 'Roach' 'Whitefish' 'Parkki' 'Perch' 'Pike' 'Smelt']
    Perch        56
    Bream        35
    Roach        20
    Pike         17
    Smelt        14
    Parkki       11
    Whitefish     6
    Name: Species, dtype: int64
    

## 데이터 가공


```python
fish_input = fish[['Weight','Length','Diagonal','Height','Width']].to_numpy()
fish_input.shape
```




    (159, 5)




```python
print(fish_input[:5])
```

    [[242.      25.4     30.      11.52     4.02  ]
     [290.      26.3     31.2     12.48     4.3056]
     [340.      26.5     31.1     12.3778   4.6961]
     [363.      29.      33.5     12.73     4.4555]
     [430.      29.      34.      12.444    5.134 ]]
    

- 타깃 데이터, 종속변수, Y


```python
fish_target = fish['Species'].to_numpy()
print(fish_target.shape)
print(fish_target[:5])
```

    (159,)
    ['Bream' 'Bream' 'Bream' 'Bream' 'Bream']
    

훈련데이터


```python
from sklearn.model_selection import train_test_split
train_input, test_input, train_target, test_target = train_test_split(
      fish_input, fish_target, random_state=42
)

# 층화 샘플링
```

## 표준 전처리
- 여기에서도 훈련세트의 통계 값으로 테스트 세트를 변환해야 한다는 점을 잊으면 안됨.

- 데이터 가공
  + 숫자 결측치가 존재, 평균값으로 대체
  + 원본 데이터 평균대치를 하면 안됨
  + 훈련 데이터와 테스트 데이터 분리
- 데이터 누수(Data Leakage) 
  + 훈련데이터 평균값 70을 대치(0)
  + 테스트데이터 평균값 75을 대치(x)
  + 모든 데이터 평균값 72.5을 대치(x)

- 교재 p97
  - 기준을 맞춰라  -> 데이터 단위 표준화


```python
#p100
# train_scaled = (train_input - mean) / std
```


```python
from sklearn.preprocessing import StandardScaler
ss = StandardScaler()# 데이터 표준화
ss.fit(train_input)
# ss.fit(test_input)(테스트 값을 대치하면 안됨)
train_scaled = ss.transform(train_input)
test_scaled= ss.transform(test_input)
```

## 모형 만들기
- k-최근접 이웃


```python
from sklearn.neighbors import KNeighborsClassifier
kn = KNeighborsClassifier(n_neighbors = 3)
kn.fit(train_scaled, train_target)

print(kn.score(train_scaled, train_target))
print(kn.score(test_scaled, test_target))
```

    0.8907563025210085
    0.85
    

- 타깃값 확인
- 알파벳으로 되어 있음



```python
print(kn.classes_)
```

    ['Bream' 'Parkki' 'Perch' 'Pike' 'Roach' 'Smelt' 'Whitefish']
    

다중 분류


```python
print(kn.predict(test_scaled[:5]))
```

    ['Perch' 'Smelt' 'Pike' 'Perch' 'Perch']
    

- 5개 샘플에 대한 예측은 어떤 확률임?


```python
import numpy as np
proba = kn.predict_proba(test_scaled[:5])
print(np.round(proba, decimals=4))
```

    [[0.     0.     1.     0.     0.     0.     0.    ]
     [0.     0.     0.     0.     0.     1.     0.    ]
     [0.     0.     0.     1.     0.     0.     0.    ]
     [0.     0.     0.6667 0.     0.3333 0.     0.    ]
     [0.     0.     0.6667 0.     0.3333 0.     0.    ]]
    

- 첫번째 클래스, perch
  - 100% 확률로 perch 예측
- 네번째 클라스 perch
  - 66.7% 확률로 perch로 예측
  - 33.3% 확률로 roach 예측

## 회귀 식
- y= ax + b
- 양변의 로그를 취함


## 로지스틱 회귀로 이진 분류 수행


```python
char_arr = np.array(['A','B','C','D','E'])
print(char_arr[[True, False, True, False, False]])
```

    ['A' 'C']
    

## 도미와 빙어의 행만 골라냄


```python
bream_smelt_indexes = (train_target == 'Bream')|(train_target == 'Smelt')
print(bream_smelt_indexes)
train_bream_smelt = train_scaled[bream_smelt_indexes]
target_bream_smelt = train_target[bream_smelt_indexes]

train_scaled.shape, train_bream_smelt.shape
```

    [ True False  True False False False False  True False False False  True
     False False False  True  True False False  True False  True False False
     False  True False False  True False False False False  True False False
      True  True False False False False False  True False False False False
     False  True False  True False False  True False False False  True False
     False False False False False  True False  True False False False False
     False False False False False  True False  True False False  True  True
     False False False  True False False False False False  True False False
     False  True False  True False False  True  True False False False False
     False False False False  True  True False False  True False False]
    




    ((119, 5), (33, 5))






```python
from sklearn.linear_model import LogisticRegression
lr = LogisticRegression()
lr.fit(train_bream_smelt, target_bream_smelt)
```




    LogisticRegression()




```python
print(lr.predict(train_bream_smelt[:5]))
```

    ['Bream' 'Smelt' 'Bream' 'Bream' 'Bream']
    

## 확률값 구하기


```python
print(lr.predict_proba(train_bream_smelt[:5]))
```

    [[0.99759855 0.00240145]
     [0.02735183 0.97264817]
     [0.99486072 0.00513928]
     [0.98584202 0.01415798]
     [0.99767269 0.00232731]]
    


```python
print(lr.classes_)
```

    ['Bream' 'Smelt']
    

- 분류 기준 : threshold 임계값 설정

도미 vs 빙어

[0.49,0.51]

[0.90,0.10]

 - 계수와 절편을 구하


```python
print(lr.coef_,lr.intercept_)
```

    [[-0.4037798  -0.57620209 -0.66280298 -1.01290277 -0.73168947]] [-2.16155132]
    


```python
decisions = lr.decision_function(train_bream_smelt[:5])
print(decisions) #z 값
```

    [-6.02927744  3.57123907 -5.26568906 -4.24321775 -6.0607117 ]
    

- z값을 확률 값으로 변환해야 함


```python
from scipy.special import expit
print(expit(decisions))
```

    [0.00240145 0.97264817 0.00513928 0.01415798 0.00232731]
    

- 다중분류 수행하기
  - 이진 분류의 확장판


```python
# 하이퍼 파라미터 세팅
# 모형을 튜닝
# 순정을 써야함
# 모형 결과의 과대적합 또는 과소 적합을 방지하기 위한 것
lr = LogisticRegression(C=20, max_iter = 1000)
lr.fit(train_scaled, train_target)
print(lr.score(train_scaled, train_target))
print(lr.score(test_scaled, test_target))
```

    0.9327731092436975
    0.925
    


```python
print(lr.predict(test_scaled[:5]))
```

    ['Perch' 'Smelt' 'Pike' 'Roach' 'Perch']
    


```python
proba = lr.predict_proba(test_scaled[:5])
print(np.round(proba, decimals = 3))
print(lr.classes_)
```

    [[0.    0.014 0.841 0.    0.136 0.007 0.003]
     [0.    0.003 0.044 0.    0.007 0.946 0.   ]
     [0.    0.    0.034 0.935 0.015 0.016 0.   ]
     [0.011 0.034 0.306 0.007 0.567 0.    0.076]
     [0.    0.    0.904 0.002 0.089 0.002 0.001]]
    ['Bream' 'Parkki' 'Perch' 'Pike' 'Roach' 'Smelt' 'Whitefish']
    

- 다중 분륭ㄹ 경우 선형 방정식은 어떤 모형?
- 분류 7개,컬럼 5개에 대한 선형 방식


```python
print(lr.coef_.shape, lr.intercept_.shape)
```

    (7, 5) (7,)
    

## 평가 지표
- 회귀 평가지표
- 결정계수
  - 1- (타깃 - 예측)^2 의 합 / (타깃-평균)^2합
- mae,mse,rmse
  - (실제 - 예측) = 오차
  - mae : mean absolute error오차의 절대 값의 평균
  - mse(mean squared error) : 오차 제곱의 평균
  - rmse(route mean squared error) : mse의 제곱근을 취한 값

- 좋은 모델이란
  - 결정계수 : 1에 수렴하면 좋은 모델
  - mae,mse, rmse : 0에 수렴하면 좋은 모델


```python
import numpy as np
from sklearn.metrics import mean_absolute_error,mean_squared_error,r2_score

true = np.array([1,2,3,2,3,5,4,6,5,6,7,8,8,]) # 실제값
preds = np.array([1,1,2,2,3,4,4,5,5,7,7,6,8]) # 예측값

mae =mean_absolute_error(true,preds)
print(mae) # 절대값 오차의 평균

mse = mean_squared_error(true,preds)
print(mse) # 제곱 오차의 평균

rmse = np.sqrt(mse)# 루트를 씌워줌
print(rmse) # mse 제곱근

# 결정계수
r2 = r2_score(true,preds)
print(r2)
```

    0.5384615384615384
    0.6923076923076923
    0.8320502943378437
    0.8617021276595744
    

### 분류 평가지표
- 오차행렬
  - https://velog.io/@skyepodium/%EB%B6%84%EB%A5%98-%EB%AA%A8%EB%8D%B8-%ED%8F%89%EA%B0%80-%EB%B0%A9%EB%B2%95
- 실제 값
  - [빙어, 도미, 도미, 빙어, 도미]
  - [빙어, 빙어, 도미, 빙어, 빙어] 

TP(빙어를 빙어로 예측) : 2

TN(도미를 도미로 예측) : 1

FN(실제 도미, 예측 빙어) : 2

FP(실제 빙어, 예측 도미) : 0

- TP,TN,FP,FN
  - TP=5, TN=5, FP=3, FN=7
  - 정확도 : (TP +TN)/ (TP +TN+FP+FN)
  - 정밀도(PRECISION) : 양성이락 예측한 값 (TP + FP) 중에서 실제 양성인 값의 비율
  - 재현율 : 실제로 양성인것 중에서 양성으로 예측한 비율 (TP)의 비율
  - 로그 손실
  - ROC CURVE (=AUC)
- 모형의 정확도 3/5 = 60%

- 코로나 검사
  - 양성 (1): 음성(99)
  - 머신러닝 모형 : 98% / 정밀도 99 (좋지 못한 모델)
  - 인간 음성진단 모형 : 99% / 정밀도 95
    - 검사자가 실제로 양성, 진단은 음성으로 내림 
- 정밀도가 높은 모형을 사용해야함
  - 양성 비율 1%
  - 비율 1:99



```python
from sklearn.metrics import confusion_matrix

true = [0,1,1,0,0]
preds = [1,0,0,0,0]

confusion_matrix(true,preds)
```




    array([[2, 1],
           [2, 0]])




