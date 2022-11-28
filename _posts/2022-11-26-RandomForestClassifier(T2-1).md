---
layout: single
title:  "(기초)파이썬 RandomForestClassifier로 예측 모델 만들기"
---

<br/>**Question**<br/>

타이타닉 생존자 예측 모형 만들기

<br/>**Data**<br/>
+ y_train: 생존여부(학습용)
+ x_train, x_test: 승객 정보 (학습 및 평가용)
[데이터 가져오기](https://www.kaggle.com/competitions/titanic/data)



# 1. 데이터 탐색
<br/>**1-1 라이브러리 및 데이터 불러오기**<br/>


```python
import pandas as pd
import numpy as np
#데이터 불러오기는 생략했습니다.
```

<br/>**1-2 EDA 수행**<br/>



```python
print(X_train.info())
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 712 entries, 90 to 116
    Data columns (total 11 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   PassengerId  712 non-null    int64  
     1   Pclass       712 non-null    int64  
     2   Name         712 non-null    object 
     3   Sex          712 non-null    object 
     4   Age          575 non-null    float64
     5   SibSp        712 non-null    int64  
     6   Parch        712 non-null    int64  
     7   Ticket       712 non-null    object 
     8   Fare         712 non-null    float64
     9   Cabin        170 non-null    object 
     10  Embarked     711 non-null    object 
    dtypes: float64(2), int64(4), object(5)
    memory usage: 66.8+ KB
    None
    

+ Age와 Cabin 컬럼에 결측치가 많은 것을 확인할 수 있습니다.


```python
X_train.head(3)
```




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
      <th>PassengerId</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>90</th>
      <td>91</td>
      <td>3</td>
      <td>Christmann, Mr. Emil</td>
      <td>male</td>
      <td>29.0</td>
      <td>0</td>
      <td>0</td>
      <td>343276</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>103</th>
      <td>104</td>
      <td>3</td>
      <td>Johansson, Mr. Gustaf Joel</td>
      <td>male</td>
      <td>33.0</td>
      <td>0</td>
      <td>0</td>
      <td>7540</td>
      <td>8.6542</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>577</th>
      <td>578</td>
      <td>1</td>
      <td>Silvey, Mrs. William Baird (Alice Munger)</td>
      <td>female</td>
      <td>39.0</td>
      <td>1</td>
      <td>0</td>
      <td>13507</td>
      <td>55.9000</td>
      <td>E44</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
y_train.head(3)
```




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
      <th>PassengerId</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>90</th>
      <td>91</td>
      <td>0</td>
    </tr>
    <tr>
      <th>103</th>
      <td>104</td>
      <td>0</td>
    </tr>
    <tr>
      <th>577</th>
      <td>578</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



# 2. 데이터 전처리

<br/>**2-1 불필요한 컬럼 삭제**<br/>
+ 이름은 탑승자 id로 확인할 수 있고, 최종 제출시에도 불필요하니 삭제해줍니다.


```python
del X_train['Name']
```


```python
del X_test['Name']
```

<br/>**2-2 범주형을 수치형으로 변환(인코딩)**<br/>


```python
from sklearn.preprocessing import LabelEncoder
```


```python
encoder=LabelEncoder()      #인코딩 위한 객체 생성
```


```python
X_train['Sex']=encoder.fit_transform(X_train['Sex'])
X_train.head(3)
```




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
      <th>PassengerId</th>
      <th>Pclass</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>90</th>
      <td>91</td>
      <td>3</td>
      <td>1</td>
      <td>29.0</td>
      <td>0</td>
      <td>0</td>
      <td>343276</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>103</th>
      <td>104</td>
      <td>3</td>
      <td>1</td>
      <td>33.0</td>
      <td>0</td>
      <td>0</td>
      <td>7540</td>
      <td>8.6542</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>577</th>
      <td>578</td>
      <td>1</td>
      <td>0</td>
      <td>39.0</td>
      <td>1</td>
      <td>0</td>
      <td>13507</td>
      <td>55.9000</td>
      <td>E44</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
X_test['Sex']=encoder.fit_transform(X_test['Sex'])
X_test.head(3)
```




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
      <th>PassengerId</th>
      <th>Pclass</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>210</th>
      <td>211</td>
      <td>3</td>
      <td>1</td>
      <td>24.0</td>
      <td>0</td>
      <td>0</td>
      <td>SOTON/O.Q. 3101311</td>
      <td>7.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>876</th>
      <td>877</td>
      <td>3</td>
      <td>1</td>
      <td>20.0</td>
      <td>0</td>
      <td>0</td>
      <td>7534</td>
      <td>9.8458</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>666</th>
      <td>667</td>
      <td>2</td>
      <td>1</td>
      <td>25.0</td>
      <td>0</td>
      <td>0</td>
      <td>234686</td>
      <td>13.0000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



+ 범주형 변수인 Pclass(티켓 class), SibSp(배우자), Parch(자녀수), Embarked(출발지) 변수도 똑같이 변환해줍니다.


```python
X_train['Pclass']=encoder.fit_transform(X_train['Pclass'])
X_train['SibSp']=encoder.fit_transform(X_train['SibSp'])
X_train['Parch']=encoder.fit_transform(X_train['Parch'])
X_train['Embarked']=encoder.fit_transform(X_train['Embarked'])
```


```python
X_test['Pclass']=encoder.fit_transform(X_test['Pclass'])
X_test['SibSp']=encoder.fit_transform(X_test['SibSp'])
X_test['Parch']=encoder.fit_transform(X_test['Parch'])
X_test['Embarked']=encoder.fit_transform(X_test['Embarked'])
```

<br/>**2-3 결측치 처리**<br/>
+ Age 컬럼의 결측치는 평균나이로 대치하겠습니다.


```python
X_train['Age']=X_train['Age'].fillna(X_train['Age'].mean())
X_test['Age']=X_test['Age'].fillna(X_test['Age'].mean())
```

+ cabin (승무원 번호)는 결측치도 많고, 데이터 분석에 필요한 정보는 아니니 삭제하겠습니다.


```python
X_train=X_train.drop(columns=['Cabin'])
X_test=X_test.drop(columns=['Cabin'])
```

<br/>**2-4 데이터 스케일링**<br/>
+ 데이터의 분포가 일의 자리에서 백의 자리로 크지 않으므로 스케일링은 수행하지 않겠습니다.


```python
X_train.describe().T
```




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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>PassengerId</th>
      <td>712.0</td>
      <td>446.910112</td>
      <td>257.882594</td>
      <td>1.00</td>
      <td>226.750</td>
      <td>444.000000</td>
      <td>672.2500</td>
      <td>891.0000</td>
    </tr>
    <tr>
      <th>Pclass</th>
      <td>712.0</td>
      <td>1.285112</td>
      <td>0.842875</td>
      <td>0.00</td>
      <td>0.000</td>
      <td>2.000000</td>
      <td>2.0000</td>
      <td>2.0000</td>
    </tr>
    <tr>
      <th>Sex</th>
      <td>712.0</td>
      <td>0.647472</td>
      <td>0.478093</td>
      <td>0.00</td>
      <td>0.000</td>
      <td>1.000000</td>
      <td>1.0000</td>
      <td>1.0000</td>
    </tr>
    <tr>
      <th>Age</th>
      <td>712.0</td>
      <td>29.414783</td>
      <td>13.108849</td>
      <td>0.42</td>
      <td>22.000</td>
      <td>29.414783</td>
      <td>35.0000</td>
      <td>74.0000</td>
    </tr>
    <tr>
      <th>SibSp</th>
      <td>712.0</td>
      <td>0.519663</td>
      <td>1.013082</td>
      <td>0.00</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>1.0000</td>
      <td>6.0000</td>
    </tr>
    <tr>
      <th>Parch</th>
      <td>712.0</td>
      <td>0.391854</td>
      <td>0.802311</td>
      <td>0.00</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.0000</td>
      <td>6.0000</td>
    </tr>
    <tr>
      <th>Fare</th>
      <td>712.0</td>
      <td>33.388155</td>
      <td>50.807818</td>
      <td>0.00</td>
      <td>7.925</td>
      <td>15.047900</td>
      <td>31.3875</td>
      <td>512.3292</td>
    </tr>
    <tr>
      <th>Embarked</th>
      <td>712.0</td>
      <td>1.526685</td>
      <td>0.805652</td>
      <td>0.00</td>
      <td>1.000</td>
      <td>2.000000</td>
      <td>2.0000</td>
      <td>3.0000</td>
    </tr>
  </tbody>
</table>
</div>



<br/>**2-5 다중공산성 확인**<br/>
+ 각 변수들의 상관관계를 확인 후 예측 결과에 부정적 영향을 미치는 상관관계가 큰 변수를 제거해주겠습니다.


```python
X_train.corr()
# 상관관계가 0.7을 넘어가는 변수가 없으니 제거하지 않겠습니다.
```




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
      <th>PassengerId</th>
      <th>Pclass</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>PassengerId</th>
      <td>1.000000</td>
      <td>-0.037308</td>
      <td>0.017995</td>
      <td>0.020696</td>
      <td>-0.061090</td>
      <td>-0.035620</td>
      <td>-0.002682</td>
      <td>0.009943</td>
    </tr>
    <tr>
      <th>Pclass</th>
      <td>-0.037308</td>
      <td>1.000000</td>
      <td>0.145065</td>
      <td>-0.330644</td>
      <td>0.089781</td>
      <td>0.011343</td>
      <td>-0.563574</td>
      <td>0.167938</td>
    </tr>
    <tr>
      <th>Sex</th>
      <td>0.017995</td>
      <td>0.145065</td>
      <td>1.000000</td>
      <td>0.082814</td>
      <td>-0.158447</td>
      <td>-0.229699</td>
      <td>-0.218511</td>
      <td>0.110268</td>
    </tr>
    <tr>
      <th>Age</th>
      <td>0.020696</td>
      <td>-0.330644</td>
      <td>0.082814</td>
      <td>1.000000</td>
      <td>-0.271659</td>
      <td>-0.211037</td>
      <td>0.086211</td>
      <td>-0.017429</td>
    </tr>
    <tr>
      <th>SibSp</th>
      <td>-0.061090</td>
      <td>0.089781</td>
      <td>-0.158447</td>
      <td>-0.271659</td>
      <td>1.000000</td>
      <td>0.439542</td>
      <td>0.164166</td>
      <td>0.077762</td>
    </tr>
    <tr>
      <th>Parch</th>
      <td>-0.035620</td>
      <td>0.011343</td>
      <td>-0.229699</td>
      <td>-0.211037</td>
      <td>0.439542</td>
      <td>1.000000</td>
      <td>0.231739</td>
      <td>0.043637</td>
    </tr>
    <tr>
      <th>Fare</th>
      <td>-0.002682</td>
      <td>-0.563574</td>
      <td>-0.218511</td>
      <td>0.086211</td>
      <td>0.164166</td>
      <td>0.231739</td>
      <td>1.000000</td>
      <td>-0.221899</td>
    </tr>
    <tr>
      <th>Embarked</th>
      <td>0.009943</td>
      <td>0.167938</td>
      <td>0.110268</td>
      <td>-0.017429</td>
      <td>0.077762</td>
      <td>0.043637</td>
      <td>-0.221899</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



# 3. 모델 학습 및 평가


```python
from sklearn.ensemble import RandomForestClassifier #랜덤포레스트 분류기 불러오기
```


```python
model=RandomForestClassifier(n_estimators=200, max_depth=7)
# 하이퍼 파라미터 튜닝은 해설에 있는 값을 따라했습니다.
```

+ 모델 학습 전 제출시에만 필요한 passengerid 컬럼은 따로 분리한 후 삭제하겠습니다.


```python
x_test_id=X_test['PassengerId']
X_train=X_train.drop(columns='PassengerId')
X_test=X_test.drop(columns='PassengerId')
y_train=y_train.drop(columns='PassengerId')
```

```python
model.fit(X_train, y_train)  #모델 학습시키기
```

    




    RandomForestClassifier(max_depth=7, n_estimators=200)




```python
y_pred=mo

.predict(X_test)   #예측값 생성
len(X_test)
```




    179



<br/>**모델 평가하기**<br/>


```python
model.score(X_train,y_train)
#score 매서드를 사용하여 훈련용 데이터를 모델에 넣고 정확도를 검증합니다.
```




    0.9157303370786517



# 4. 결과 출력하기


```python
result=pd.DataFrame(y_pred) # y 예측값을 데이터프레임으로 저장
final=pd.concat([x_test_id,result],axis=1)
#final.to_csv('data/12345.csv,index=False') 명령으로 저장
```
