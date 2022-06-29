---
title: "0629_pandas"
date: '2022-06-29'
---

## pandas
 - index (숫자, 문자, 날짜) 모두가 올 수 있지만 중복되지 않음.
 - index, columm이 하나씩 포함되면 시리즈로 진행되지만, index, column 여러개가 포함되면 dataframe이 됨.
 - dataframe.group by() -> groupby 클래스로 바뀜 
 (dataframe에서 사용되던 명령어들이 진행되지 않음)

## 라이브러리 불러오기


```python
import pandas as pd
import numpy as np
print("pandas version: ", pd.__version__)
print("numpy version:", np.__version__)
```

    pandas version:  1.3.5
    numpy version: 1.21.6
    

## 데이터 불러오기
 - 구글드라이브 데이터 존재


```python
from google.colab import drive
drive.mount('/content/drive')
```

    Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount("/content/drive", force_remount=True).
    


```python
DATA_PATH = '/content/drive/MyDrive/Colab Notebooks/human_AI/Basic/Chapter 3. pandas/data/Lemonade2016.csv'
lemonade = pd.read_csv(DATA_PATH)
print(type(lemonade))
lemonade.info()
```

    <class 'pandas.core.frame.DataFrame'>
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 32 entries, 0 to 31
    Data columns (total 7 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   Date         31 non-null     object 
     1   Location     32 non-null     object 
     2   Lemon        32 non-null     int64  
     3   Orange       32 non-null     int64  
     4   Temperature  32 non-null     int64  
     5   Leaflets     31 non-null     float64
     6   Price        32 non-null     float64
    dtypes: float64(2), int64(3), object(2)
    memory usage: 1.9+ KB
    

- object - 문자
- int - 정수
- float - 소수

- 데이터 맛보기


```python
print(lemonade.head())# 맨 위의 데이터 보기
```

           Date Location  Lemon  Orange  Temperature  Leaflets  Price
    0  7/1/2016     Park     97      67           70      90.0   0.25
    1  7/2/2016     Park     98      67           72      90.0   0.25
    2  7/3/2016     Park    110      77           71     104.0   0.25
    3  7/4/2016    Beach    134      99           76      98.0   0.25
    4  7/5/2016    Beach    159     118           78     135.0   0.25
    


```python
print(lemonade.tail()) # 맨 밑의 데이터 보기
```

             Date Location  Lemon  Orange  Temperature  Leaflets  Price
    27  7/27/2016     Park    104      68           80      99.0   0.35
    28  7/28/2016     Park     96      63           82      90.0   0.35
    29  7/29/2016     Park    100      66           81      95.0   0.35
    30  7/30/2016    Beach     88      57           82      81.0   0.35
    31  7/31/2016    Beach     76      47           82      68.0   0.35
    

- 기술통계량 보는 함수
- describe


```python
print(lemonade.describe()) # 사분위수 확인 가능
# 표준 편차가 높으면 변동이 심함, 낮으면 변동이 크게 없음
```

                Lemon      Orange  Temperature    Leaflets      Price
    count   32.000000   32.000000    32.000000   31.000000  32.000000
    mean   116.156250   80.000000    78.968750  108.548387   0.354688
    std     25.823357   21.863211     4.067847   20.117718   0.113137
    min     71.000000   42.000000    70.000000   68.000000   0.250000
    25%     98.000000   66.750000    77.000000   90.000000   0.250000
    50%    113.500000   76.500000    80.500000  108.000000   0.350000
    75%    131.750000   95.000000    82.000000  124.000000   0.500000
    max    176.000000  129.000000    84.000000  158.000000   0.500000
    

- 범주형 데이터 빈도수 구하기


```python
lemonade['Location']
```




    0      Park
    1      Park
    2      Park
    3     Beach
    4     Beach
    5     Beach
    6     Beach
    7     Beach
    8     Beach
    9     Beach
    10    Beach
    11    Beach
    12    Beach
    13    Beach
    14    Beach
    15    Beach
    16    Beach
    17    Beach
    18     Park
    19     Park
    20     Park
    21     Park
    22     Park
    23     Park
    24     Park
    25     Park
    26     Park
    27     Park
    28     Park
    29     Park
    30    Beach
    31    Beach
    Name: Location, dtype: object




```python
lemonade['Location'].value_counts()
```




    Beach    17
    Park     15
    Name: Location, dtype: int64




```python
print(type(lemonade))
```

    <class 'pandas.core.frame.DataFrame'>
    

## 행과 열 다루기
- sold(판매량) 컬럼
(=피처=feature)을 추가



```python
# lemonade['새로운 변수']
lemonade['Sold'] = 0
print(lemonade.head(3))
```

           Date Location  Lemon  Orange  Temperature  Leaflets  Price  Sold
    0  7/1/2016     Park     97      67           70      90.0   0.25     0
    1  7/2/2016     Park     98      67           72      90.0   0.25     0
    2  7/3/2016     Park    110      77           71     104.0   0.25     0
    


```python
lemonade['Sold'] = lemonade['Lemon'] + lemonade['Orange']
print(lemonade.head(3))
```

           Date Location  Lemon  Orange  Temperature  Leaflets  Price  Sold
    0  7/1/2016     Park     97      67           70      90.0   0.25   164
    1  7/2/2016     Park     98      67           72      90.0   0.25   165
    2  7/3/2016     Park    110      77           71     104.0   0.25   187
    



 - revenue(매출) = 단가 x 판매량


```python
lemonade['Revenue'] = lemonade['Sold'] * lemonade ['Price']
print(lemonade.head())

print(lemonade[['Revenue', 'Price','Sold']].head())
```

           Date Location  Lemon  Orange  Temperature  Leaflets  Price  Sold  \
    0  7/1/2016     Park     97      67           70      90.0   0.25   164   
    1  7/2/2016     Park     98      67           72      90.0   0.25   165   
    2  7/3/2016     Park    110      77           71     104.0   0.25   187   
    3  7/4/2016    Beach    134      99           76      98.0   0.25   233   
    4  7/5/2016    Beach    159     118           78     135.0   0.25   277   
    
       Revenue  
    0    41.00  
    1    41.25  
    2    46.75  
    3    58.25  
    4    69.25  
       Revenue  Price  Sold
    0    41.00   0.25   164
    1    41.25   0.25   165
    2    46.75   0.25   187
    3    58.25   0.25   233
    4    69.25   0.25   277
    

- drop() 함수 사용해서 열 제거 또는 행 제거


```python
# 컬럼제거
col_drop = lemonade.drop('Sold', axis = 1)# axis =1 은 컬럼제거
print(col_drop.head())
```

           Date Location  Lemon  Orange  Temperature  Leaflets  Price  Revenue
    0  7/1/2016     Park     97      67           70      90.0   0.25    41.00
    1  7/2/2016     Park     98      67           72      90.0   0.25    41.25
    2  7/3/2016     Park    110      77           71     104.0   0.25    46.75
    3  7/4/2016    Beach    134      99           76      98.0   0.25    58.25
    4  7/5/2016    Beach    159     118           78     135.0   0.25    69.25
    


```python
#행 제거
row_drop = lemonade.drop(2, axis = 0)
print(row_drop.head())
```

           Date Location  Lemon  Orange  Temperature  Leaflets  Price  Sold  \
    0  7/1/2016     Park     97      67           70      90.0   0.25   164   
    1  7/2/2016     Park     98      67           72      90.0   0.25   165   
    3  7/4/2016    Beach    134      99           76      98.0   0.25   233   
    4  7/5/2016    Beach    159     118           78     135.0   0.25   277   
    5  7/6/2016    Beach    103      69           82      90.0   0.25   172   
    
       Revenue  
    0    41.00  
    1    41.25  
    3    58.25  
    4    69.25  
    5    43.00  
    

 - 데이터 언덱싱


```python
print(lemonade[0:5])
```

           Date Location  Lemon  Orange  Temperature  Leaflets  Price  Sold  \
    0  7/1/2016     Park     97      67           70      90.0   0.25   164   
    1  7/2/2016     Park     98      67           72      90.0   0.25   165   
    2  7/3/2016     Park    110      77           71     104.0   0.25   187   
    3  7/4/2016    Beach    134      99           76      98.0   0.25   233   
    4  7/5/2016    Beach    159     118           78     135.0   0.25   277   
    
       Revenue  
    0    41.00  
    1    41.25  
    2    46.75  
    3    58.25  
    4    69.25  
    

- 특정 값만 추출
- filter


```python
# lemonade[데이터 컬럼 == 특정값]
lemonade[lemonade['Location'] == 'Beach']
lemonade['Location'] == 'Beach' # T/F 인지 확인
```




    0     False
    1     False
    2     False
    3      True
    4      True
    5      True
    6      True
    7      True
    8      True
    9      True
    10     True
    11     True
    12     True
    13     True
    14     True
    15     True
    16     True
    17     True
    18    False
    19    False
    20    False
    21    False
    22    False
    23    False
    24    False
    25    False
    26    False
    27    False
    28    False
    29    False
    30     True
    31     True
    Name: Location, dtype: bool




```python
lemonade[(lemonade['Temperature']>=80) & (lemonade['Orange']>=100) & (lemonade['Location']== "Park")]
```





  <div id="df-dadf4d72-78db-4731-adf2-748d006edb21">
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
      <th>Date</th>
      <th>Location</th>
      <th>Lemon</th>
      <th>Orange</th>
      <th>Temperature</th>
      <th>Leaflets</th>
      <th>Price</th>
      <th>Sold</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>25</th>
      <td>7/25/2016</td>
      <td>Park</td>
      <td>156</td>
      <td>113</td>
      <td>84</td>
      <td>135.0</td>
      <td>0.50</td>
      <td>269</td>
      <td>134.50</td>
    </tr>
    <tr>
      <th>26</th>
      <td>7/26/2016</td>
      <td>Park</td>
      <td>176</td>
      <td>129</td>
      <td>83</td>
      <td>158.0</td>
      <td>0.35</td>
      <td>305</td>
      <td>106.75</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-dadf4d72-78db-4731-adf2-748d006edb21')"
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
          document.querySelector('#df-dadf4d72-78db-4731-adf2-748d006edb21 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-dadf4d72-78db-4731-adf2-748d006edb21');
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





```python
(lemonade['Temperature'] >= 80, lemonade['Orange'] >= 100) 
lemonade[(lemonade['Temperature'] >= 80) & (lemonade['Orange'] >= 100)]
```





  <div id="df-8ace8091-5411-421d-8e3d-31d36f773ed3">
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
      <th>Date</th>
      <th>Location</th>
      <th>Lemon</th>
      <th>Orange</th>
      <th>Temperature</th>
      <th>Leaflets</th>
      <th>Price</th>
      <th>Sold</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>7/7/2016</td>
      <td>Beach</td>
      <td>143</td>
      <td>101</td>
      <td>81</td>
      <td>135.0</td>
      <td>0.25</td>
      <td>244</td>
      <td>61.00</td>
    </tr>
    <tr>
      <th>11</th>
      <td>7/11/2016</td>
      <td>Beach</td>
      <td>162</td>
      <td>120</td>
      <td>83</td>
      <td>135.0</td>
      <td>0.25</td>
      <td>282</td>
      <td>70.50</td>
    </tr>
    <tr>
      <th>25</th>
      <td>7/25/2016</td>
      <td>Park</td>
      <td>156</td>
      <td>113</td>
      <td>84</td>
      <td>135.0</td>
      <td>0.50</td>
      <td>269</td>
      <td>134.50</td>
    </tr>
    <tr>
      <th>26</th>
      <td>7/26/2016</td>
      <td>Park</td>
      <td>176</td>
      <td>129</td>
      <td>83</td>
      <td>158.0</td>
      <td>0.35</td>
      <td>305</td>
      <td>106.75</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-8ace8091-5411-421d-8e3d-31d36f773ed3')"
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
          document.querySelector('#df-8ace8091-5411-421d-8e3d-31d36f773ed3 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-8ace8091-5411-421d-8e3d-31d36f773ed3');
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





```python
# lemonade[조건식1 & 조건식2 -> True / Flase]
# true & true -> true3
# false/ true -> true
# 데이터 컬럼 == 특정값
lemonade.loc[lemonade['Temperature'] >= 80, ['Date', 'Sold']] 
# iloc 숫자로 불러옴, loc 컬럼명, 문자로 불러옴
```





  <div id="df-99935155-abd5-40a2-b830-9e9170618be9">
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
      <th>Date</th>
      <th>Sold</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>7/6/2016</td>
      <td>172</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7/6/2016</td>
      <td>172</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7/7/2016</td>
      <td>244</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>209</td>
    </tr>
    <tr>
      <th>9</th>
      <td>7/9/2016</td>
      <td>229</td>
    </tr>
    <tr>
      <th>10</th>
      <td>7/10/2016</td>
      <td>238</td>
    </tr>
    <tr>
      <th>11</th>
      <td>7/11/2016</td>
      <td>282</td>
    </tr>
    <tr>
      <th>12</th>
      <td>7/12/2016</td>
      <td>225</td>
    </tr>
    <tr>
      <th>18</th>
      <td>7/18/2016</td>
      <td>223</td>
    </tr>
    <tr>
      <th>22</th>
      <td>7/22/2016</td>
      <td>187</td>
    </tr>
    <tr>
      <th>23</th>
      <td>7/23/2016</td>
      <td>202</td>
    </tr>
    <tr>
      <th>24</th>
      <td>7/24/2016</td>
      <td>203</td>
    </tr>
    <tr>
      <th>25</th>
      <td>7/25/2016</td>
      <td>269</td>
    </tr>
    <tr>
      <th>26</th>
      <td>7/26/2016</td>
      <td>305</td>
    </tr>
    <tr>
      <th>27</th>
      <td>7/27/2016</td>
      <td>172</td>
    </tr>
    <tr>
      <th>28</th>
      <td>7/28/2016</td>
      <td>159</td>
    </tr>
    <tr>
      <th>29</th>
      <td>7/29/2016</td>
      <td>166</td>
    </tr>
    <tr>
      <th>30</th>
      <td>7/30/2016</td>
      <td>145</td>
    </tr>
    <tr>
      <th>31</th>
      <td>7/31/2016</td>
      <td>123</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-99935155-abd5-40a2-b830-9e9170618be9')"
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
          document.querySelector('#df-99935155-abd5-40a2-b830-9e9170618be9 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-99935155-abd5-40a2-b830-9e9170618be9');
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





```python
lemonade[(lemonade['Sold']>=100) & (lemonade['Date']) & (lemonade['Location'] == 'Beach')]
```





  <div id="df-3c7763d7-5c2e-44d1-8ff8-6c458cd82499">
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
      <th>Date</th>
      <th>Location</th>
      <th>Lemon</th>
      <th>Orange</th>
      <th>Temperature</th>
      <th>Leaflets</th>
      <th>Price</th>
      <th>Sold</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>7/4/2016</td>
      <td>Beach</td>
      <td>134</td>
      <td>99</td>
      <td>76</td>
      <td>98.0</td>
      <td>0.25</td>
      <td>233</td>
      <td>58.25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7/5/2016</td>
      <td>Beach</td>
      <td>159</td>
      <td>118</td>
      <td>78</td>
      <td>135.0</td>
      <td>0.25</td>
      <td>277</td>
      <td>69.25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>7/6/2016</td>
      <td>Beach</td>
      <td>103</td>
      <td>69</td>
      <td>82</td>
      <td>90.0</td>
      <td>0.25</td>
      <td>172</td>
      <td>43.00</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7/6/2016</td>
      <td>Beach</td>
      <td>103</td>
      <td>69</td>
      <td>82</td>
      <td>90.0</td>
      <td>0.25</td>
      <td>172</td>
      <td>43.00</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7/7/2016</td>
      <td>Beach</td>
      <td>143</td>
      <td>101</td>
      <td>81</td>
      <td>135.0</td>
      <td>0.25</td>
      <td>244</td>
      <td>61.00</td>
    </tr>
    <tr>
      <th>9</th>
      <td>7/9/2016</td>
      <td>Beach</td>
      <td>134</td>
      <td>95</td>
      <td>80</td>
      <td>126.0</td>
      <td>0.25</td>
      <td>229</td>
      <td>57.25</td>
    </tr>
    <tr>
      <th>10</th>
      <td>7/10/2016</td>
      <td>Beach</td>
      <td>140</td>
      <td>98</td>
      <td>82</td>
      <td>131.0</td>
      <td>0.25</td>
      <td>238</td>
      <td>59.50</td>
    </tr>
    <tr>
      <th>11</th>
      <td>7/11/2016</td>
      <td>Beach</td>
      <td>162</td>
      <td>120</td>
      <td>83</td>
      <td>135.0</td>
      <td>0.25</td>
      <td>282</td>
      <td>70.50</td>
    </tr>
    <tr>
      <th>12</th>
      <td>7/12/2016</td>
      <td>Beach</td>
      <td>130</td>
      <td>95</td>
      <td>84</td>
      <td>99.0</td>
      <td>0.25</td>
      <td>225</td>
      <td>56.25</td>
    </tr>
    <tr>
      <th>13</th>
      <td>7/13/2016</td>
      <td>Beach</td>
      <td>109</td>
      <td>75</td>
      <td>77</td>
      <td>99.0</td>
      <td>0.25</td>
      <td>184</td>
      <td>46.00</td>
    </tr>
    <tr>
      <th>14</th>
      <td>7/14/2016</td>
      <td>Beach</td>
      <td>122</td>
      <td>85</td>
      <td>78</td>
      <td>113.0</td>
      <td>0.25</td>
      <td>207</td>
      <td>51.75</td>
    </tr>
    <tr>
      <th>15</th>
      <td>7/15/2016</td>
      <td>Beach</td>
      <td>98</td>
      <td>62</td>
      <td>75</td>
      <td>108.0</td>
      <td>0.50</td>
      <td>160</td>
      <td>80.00</td>
    </tr>
    <tr>
      <th>16</th>
      <td>7/16/2016</td>
      <td>Beach</td>
      <td>81</td>
      <td>50</td>
      <td>74</td>
      <td>90.0</td>
      <td>0.50</td>
      <td>131</td>
      <td>65.50</td>
    </tr>
    <tr>
      <th>17</th>
      <td>7/17/2016</td>
      <td>Beach</td>
      <td>115</td>
      <td>76</td>
      <td>77</td>
      <td>126.0</td>
      <td>0.50</td>
      <td>191</td>
      <td>95.50</td>
    </tr>
    <tr>
      <th>30</th>
      <td>7/30/2016</td>
      <td>Beach</td>
      <td>88</td>
      <td>57</td>
      <td>82</td>
      <td>81.0</td>
      <td>0.35</td>
      <td>145</td>
      <td>50.75</td>
    </tr>
    <tr>
      <th>31</th>
      <td>7/31/2016</td>
      <td>Beach</td>
      <td>76</td>
      <td>47</td>
      <td>82</td>
      <td>68.0</td>
      <td>0.35</td>
      <td>123</td>
      <td>43.05</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-3c7763d7-5c2e-44d1-8ff8-6c458cd82499')"
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
          document.querySelector('#df-3c7763d7-5c2e-44d1-8ff8-6c458cd82499 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-3c7763d7-5c2e-44d1-8ff8-6c458cd82499');
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





```python
lemonade.loc[lemonade['Temperature'] >= 80, ['Date', 'Sold']] 
```





  <div id="df-d92df16d-ad99-41f9-83f8-2922e6f9c463">
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
      <th>Date</th>
      <th>Sold</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>7/6/2016</td>
      <td>172</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7/6/2016</td>
      <td>172</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7/7/2016</td>
      <td>244</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>209</td>
    </tr>
    <tr>
      <th>9</th>
      <td>7/9/2016</td>
      <td>229</td>
    </tr>
    <tr>
      <th>10</th>
      <td>7/10/2016</td>
      <td>238</td>
    </tr>
    <tr>
      <th>11</th>
      <td>7/11/2016</td>
      <td>282</td>
    </tr>
    <tr>
      <th>12</th>
      <td>7/12/2016</td>
      <td>225</td>
    </tr>
    <tr>
      <th>18</th>
      <td>7/18/2016</td>
      <td>223</td>
    </tr>
    <tr>
      <th>22</th>
      <td>7/22/2016</td>
      <td>187</td>
    </tr>
    <tr>
      <th>23</th>
      <td>7/23/2016</td>
      <td>202</td>
    </tr>
    <tr>
      <th>24</th>
      <td>7/24/2016</td>
      <td>203</td>
    </tr>
    <tr>
      <th>25</th>
      <td>7/25/2016</td>
      <td>269</td>
    </tr>
    <tr>
      <th>26</th>
      <td>7/26/2016</td>
      <td>305</td>
    </tr>
    <tr>
      <th>27</th>
      <td>7/27/2016</td>
      <td>172</td>
    </tr>
    <tr>
      <th>28</th>
      <td>7/28/2016</td>
      <td>159</td>
    </tr>
    <tr>
      <th>29</th>
      <td>7/29/2016</td>
      <td>166</td>
    </tr>
    <tr>
      <th>30</th>
      <td>7/30/2016</td>
      <td>145</td>
    </tr>
    <tr>
      <th>31</th>
      <td>7/31/2016</td>
      <td>123</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-d92df16d-ad99-41f9-83f8-2922e6f9c463')"
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
          document.querySelector('#df-d92df16d-ad99-41f9-83f8-2922e6f9c463 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-d92df16d-ad99-41f9-83f8-2922e6f9c463');
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




## iloc vs loc
 - 문법 차이 확인


```python
lemonade.head()
```





  <div id="df-0f628514-9040-494b-8c1a-cd41b3ed2232">
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
      <th>Date</th>
      <th>Location</th>
      <th>Lemon</th>
      <th>Orange</th>
      <th>Temperature</th>
      <th>Leaflets</th>
      <th>Price</th>
      <th>Sold</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7/1/2016</td>
      <td>Park</td>
      <td>97</td>
      <td>67</td>
      <td>70</td>
      <td>90.0</td>
      <td>0.25</td>
      <td>164</td>
      <td>41.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7/2/2016</td>
      <td>Park</td>
      <td>98</td>
      <td>67</td>
      <td>72</td>
      <td>90.0</td>
      <td>0.25</td>
      <td>165</td>
      <td>41.25</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7/3/2016</td>
      <td>Park</td>
      <td>110</td>
      <td>77</td>
      <td>71</td>
      <td>104.0</td>
      <td>0.25</td>
      <td>187</td>
      <td>46.75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7/4/2016</td>
      <td>Beach</td>
      <td>134</td>
      <td>99</td>
      <td>76</td>
      <td>98.0</td>
      <td>0.25</td>
      <td>233</td>
      <td>58.25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7/5/2016</td>
      <td>Beach</td>
      <td>159</td>
      <td>118</td>
      <td>78</td>
      <td>135.0</td>
      <td>0.25</td>
      <td>277</td>
      <td>69.25</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-0f628514-9040-494b-8c1a-cd41b3ed2232')"
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
          document.querySelector('#df-0f628514-9040-494b-8c1a-cd41b3ed2232 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-0f628514-9040-494b-8c1a-cd41b3ed2232');
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




 - iloc -> 숫자만 들어감 (필터링이 들어간 data는 뽑아내지 못함)


```python
print(lemonade.iloc[0:3, 0:2]) # 시작 인덱스(행) : 끝 인덱스(열)
```

           Date Location
    0  7/1/2016     Park
    1  7/2/2016     Park
    2  7/3/2016     Park
    

- loc : 숫자 (x)
  +라벨 = 글자(숫자,문자 동시)


```python
print(lemonade.loc[0:2, ['Date', 'Location']]) # 0:2는 숫자가 아니라 인덱스 번호
```

           Date Location
    0  7/1/2016     Park
    1  7/2/2016     Park
    2  7/3/2016     Park
    

## 데이터 정렬
- sort_values()


```python
lemonade.head()
print(lemonade.sort_values(by=['Revenue']).head(10))
```

             Date Location  Lemon  Orange  Temperature  Leaflets  Price  Sold  \
    0    7/1/2016     Park     97      67           70      90.0   0.25   164   
    1    7/2/2016     Park     98      67           72      90.0   0.25   165   
    6    7/6/2016    Beach    103      69           82      90.0   0.25   172   
    5    7/6/2016    Beach    103      69           82      90.0   0.25   172   
    31  7/31/2016    Beach     76      47           82      68.0   0.35   123   
    13  7/13/2016    Beach    109      75           77      99.0   0.25   184   
    2    7/3/2016     Park    110      77           71     104.0   0.25   187   
    30  7/30/2016    Beach     88      57           82      81.0   0.35   145   
    14  7/14/2016    Beach    122      85           78     113.0   0.25   207   
    8         NaN    Beach    123      86           82     113.0   0.25   209   
    
        Revenue  
    0     41.00  
    1     41.25  
    6     43.00  
    5     43.00  
    31    43.05  
    13    46.00  
    2     46.75  
    30    50.75  
    14    51.75  
    8     52.25  
    


```python
print(lemonade[['Date', 'Temperature', 'Revenue']].sort_values(by=['Revenue']).head(10))
```

             Date  Temperature  Revenue
    0    7/1/2016           70    41.00
    1    7/2/2016           72    41.25
    6    7/6/2016           82    43.00
    5    7/6/2016           82    43.00
    31  7/31/2016           82    43.05
    13  7/13/2016           77    46.00
    2    7/3/2016           71    46.75
    30  7/30/2016           82    50.75
    14  7/14/2016           78    51.75
    8         NaN           82    52.25
    


```python
print(lemonade[['Date', 'Temperature', 'Revenue']].sort_values(by=['Temperature','Revenue']).head(5))
```

             Date  Temperature  Revenue
    0    7/1/2016           70    41.00
    20  7/20/2016           70    56.50
    2    7/3/2016           71    46.75
    1    7/2/2016           72    41.25
    16  7/16/2016           74    65.50
    


```python
print(lemonade[['Date', 'Temperature', 'Revenue']].sort_values(by=['Temperature','Revenue'], ascending = [True, False]).head(5))
# by =에서 요소들이 추가될경우 true false도 요소개수와 맞추어 져야함
```

             Date  Temperature  Revenue
    20  7/20/2016           70    56.50
    0    7/1/2016           70    41.00
    2    7/3/2016           71    46.75
    1    7/2/2016           72    41.25
    16  7/16/2016           74    65.50
    


```python
print(lemonade.head())
```

           Date Location  Lemon  Orange  Temperature  Leaflets  Price  Sold  \
    0  7/1/2016     Park     97      67           70      90.0   0.25   164   
    1  7/2/2016     Park     98      67           72      90.0   0.25   165   
    2  7/3/2016     Park    110      77           71     104.0   0.25   187   
    3  7/4/2016    Beach    134      99           76      98.0   0.25   233   
    4  7/5/2016    Beach    159     118           78     135.0   0.25   277   
    
       Revenue  
    0    41.00  
    1    41.25  
    2    46.75  
    3    58.25  
    4    69.25  
    


```python
df = lemonade. groupby(by='Location').count()
print(df)
print(type(df)) # 숫자가 다른 것은 결측치가 있다는 뜻
# 인덱스가 위치로 추출됨
```

              Date  Lemon  Orange  Temperature  Leaflets  Price  Sold  Revenue
    Location                                                                  
    Beach       16     17      17           17        17     17    17       17
    Park        15     15      15           15        14     15    15       15
    <class 'pandas.core.frame.DataFrame'>
    


```python
df[['Date', 'Lemon']]
```





  <div id="df-bca136a7-7941-4bf9-a179-01ca4fe7f5f6">
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
      <th>Date</th>
      <th>Lemon</th>
    </tr>
    <tr>
      <th>Location</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Beach</th>
      <td>16</td>
      <td>17</td>
    </tr>
    <tr>
      <th>Park</th>
      <td>15</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-bca136a7-7941-4bf9-a179-01ca4fe7f5f6')"
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
          document.querySelector('#df-bca136a7-7941-4bf9-a179-01ca4fe7f5f6 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-bca136a7-7941-4bf9-a179-01ca4fe7f5f6');
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





```python
print(df.loc['Beach',['Date','Lemon']])
```

    Date     16
    Lemon    17
    Name: Beach, dtype: int64
    

## 간단한 피벗 테이블 만들기


```python
lemonade.groupby('Location')['Revenue'].agg([max, min, sum, np.mean])
```





  <div id="df-fe921c93-e937-426b-bc65-d2076a2d80a8">
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
      <th>max</th>
      <th>min</th>
      <th>sum</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>Location</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Beach</th>
      <td>95.5</td>
      <td>43.0</td>
      <td>1002.8</td>
      <td>58.988235</td>
    </tr>
    <tr>
      <th>Park</th>
      <td>134.5</td>
      <td>41.0</td>
      <td>1178.2</td>
      <td>78.546667</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-fe921c93-e937-426b-bc65-d2076a2d80a8')"
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
          document.querySelector('#df-fe921c93-e937-426b-bc65-d2076a2d80a8 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-fe921c93-e937-426b-bc65-d2076a2d80a8');
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





```python
lemonade.groupby('Location')['Revenue', 'Sold', 'Temperature'].agg([max, min, sum, np.mean])
```

    /usr/local/lib/python3.7/dist-packages/ipykernel_launcher.py:1: FutureWarning: Indexing with multiple keys (implicitly converted to a tuple of keys) will be deprecated, use a list instead.
      """Entry point for launching an IPython kernel.
    





  <div id="df-be5b331e-b9bc-4794-ad9b-4cd617d18ff1">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">Revenue</th>
      <th colspan="4" halign="left">Sold</th>
      <th colspan="4" halign="left">Temperature</th>
    </tr>
    <tr>
      <th></th>
      <th>max</th>
      <th>min</th>
      <th>sum</th>
      <th>mean</th>
      <th>max</th>
      <th>min</th>
      <th>sum</th>
      <th>mean</th>
      <th>max</th>
      <th>min</th>
      <th>sum</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>Location</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Beach</th>
      <td>95.5</td>
      <td>43.0</td>
      <td>1002.8</td>
      <td>58.988235</td>
      <td>282</td>
      <td>123</td>
      <td>3422</td>
      <td>201.294118</td>
      <td>84</td>
      <td>74</td>
      <td>1355</td>
      <td>79.705882</td>
    </tr>
    <tr>
      <th>Park</th>
      <td>134.5</td>
      <td>41.0</td>
      <td>1178.2</td>
      <td>78.546667</td>
      <td>305</td>
      <td>113</td>
      <td>2855</td>
      <td>190.333333</td>
      <td>84</td>
      <td>70</td>
      <td>1172</td>
      <td>78.133333</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-be5b331e-b9bc-4794-ad9b-4cd617d18ff1')"
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
          document.querySelector('#df-be5b331e-b9bc-4794-ad9b-4cd617d18ff1 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-be5b331e-b9bc-4794-ad9b-4cd617d18ff1');
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



