---
title: "0629_machine learning"
date: '2022-06-29'
---

## 파이썬
 - machine learning
   + scikit-learn
   + 정형 데이터
 - deep learning
   + 비정형 데이터
   + tensorflow(구글) vs pytorch(페이스북)
   + 혼공머 : tensorflow
   + 실제 상용서비스 - tensorflow
tensorflow vs r&d - pytorch


## 생선 분류 p.45
 - 도미, 곤들매기, 농어 등등
 - 생선들을 프로그램으로 분류

 = 30cm 이상이면 도미


```python
fish_length = 31
if fish_length >= 30:
  print("도미")
```

    도미
    


```python
# 도미의 길이
bream_length = [25.4, 26.3, 26.5, 29.0, 29.0, 29.7, 29.7, 30.0, 30.0, 30.7, 31.0, 31.0, 31.5, 32.0, 32.0, 32.0, 33.0, 33.0, 33.5, 33.5, 34.0, 34.0, 34.5, 35.0, 35.0, 35.0, 35.0, 36.0, 36.0, 37.0, 38.5, 38.5, 39.5, 41.0, 41.0]
# 도미의 무게
bream_weight = [242.0, 290.0, 340.0, 363.0, 430.0, 450.0, 500.0, 390.0, 450.0, 500.0, 475.0, 500.0, 500.0, 340.0, 600.0, 600.0, 700.0, 700.0, 610.0, 650.0, 575.0, 685.0, 620.0, 680.0, 700.0, 725.0, 720.0, 714.0, 850.0, 1000.0, 920.0, 955.0, 925.0, 975.0, 950.0]
```

## 데이터 가공

## 데이터 시각화
- 여러 인사이트 확인 위해
시각화, 통계수치 계산
- 탐색적 자료 분석 
(EDA : exploratory data analysis)



```python
import matplotlib.pyplot as plt
plt.scatter(bream_length,bream_weight)
plt.xlabel('length')
plt.ylabel('weight')
plt.show
```




    <function matplotlib.pyplot.show>




    
![](/images/0629_ml/output_6_1.png)
    



```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
ax.scatter(bream_length, bream_weight)
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show()
```


    
![](/images/0629_ml/output_7_0.png)
    


- 빙어 데이터


```python
smelt_length = [9.8, 10.5, 10.6, 11.0, 11.2, 11.3, 11.8, 11.8, 12.0, 12.2, 12.4, 13.0, 14.3, 15.0]
smelt_weight = [6.7, 7.5, 7.0, 9.7, 9.8, 8.7, 10.0, 9.9, 9.8, 12.2, 13.4, 12.2, 19.7, 19.9]
```


```python
fig, ax = plt.subplots()
ax.scatter(bream_length,bream_weight)
ax.scatter(smelt_length, smelt_weight)
ax.set_xlabel('length')
ax.set_ylabel('weight')
plt.show
```




    <function matplotlib.pyplot.show>




    
![](/images/0629_ml/output_10_1.png)
    


## 두개의 리스트 합치기


```python
length = bream_length + smelt_length
weight = bream_weight + smelt_weight
```

- 2차원 리스트로 만든다


```python
fish_data = [[l,w]for l, w in zip(length, weight)]
fish_data[0:5] # 2차원 리스트
```




    [[25.4, 242.0], [26.3, 290.0], [26.5, 340.0], [29.0, 363.0], [29.0, 430.0]]



- 라벨링을 해준다 = 지도 해준다
= 지도학습


```python
fish_target = [1]*35 +[0]*14 # 리스트 연산자
print(fish_target)
```

    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
    

## 모델링


```python
from sklearn.neighbors import KNeighborsClassifier


# 클래스 인스턴스 화
kn = KNeighborsClassifier()

# 모형 학습
kn.fit(fish_data, fish_target)

# 예측 정확도
kn.score(fish_data, fish_target) # 예측 정확도가 1.0 이면 100% 정확하다는 뜻
```




    1.0



- 실제 예측
- 새로운 물고기 도착
  + 길이 :30 몸무게: 60


```python
ac_length = int(input("물고기 길이를 입력하세요."))
ac_weight = int(input("물고기 무게를 입력하세요."))

preds = kn.predict([[ac_length,ac_weight]])
print(preds)

if preds == 1:
  print("도미")
else:
  print("빙어")
kn.predict([[30,600]])# 2차원 데이터 이기때문에 2차원 리스트로 진행
```

    물고기 길이를 입력하세요.30
    물고기 무게를 입력하세요.30
    [0]
    빙어
    




    array([1])


