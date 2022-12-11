---
layout: single
title:  "StandardScaler를 사용해 수치형 변수 표준화시키기"
categories: 빅데이터분석기사
tags: [BigData, Certification]
---

<br/>**Data**<br/>

출처는 Kaggle의 Big Data Certification 입니다.<br/>
[출처 이동](https://www.kaggle.com/code/agileteam/py-t1-8-expected-questions/notebook)

<br/>**Question**<br/>

1. 'f5'컬럼을 표준화하기
2. 중앙값 구하기

<br/>**1. 라이브러리 및 데이터 불러오기**<br/>

```python
import pandas as pd                                  #판다스 불러오기
import numpy as np                                   #넘파이 불러오기
from sklearn.preprocessing import StandardScaler
   #sklearn 라이브러리의 preprocessing 모듈 중 StandardScaler 함수 가져오기
df=pd.read_csv('.../data/basic1.csv')     #데이터 불러오기
```
StandardScaler는 데이터를 평균값은 0, 표준편차는 1로 스케일링 하며,<br/>
주로 범주형 형태를 분류할 때 사용됩니다.<br/>


<br/>**2. EDA**<br/>


```python
print(df.info())
df.head()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 100 entries, 0 to 99
    Data columns (total 8 columns):
     #   Column  Non-Null Count  Dtype  
    ---  ------  --------------  -----  
     0   id      100 non-null    object 
     1   age     100 non-null    float64
     2   city    100 non-null    object 
     3   f1      69 non-null     float64
     4   f2      100 non-null    int64  
     5   f3      5 non-null      object 
     6   f4      100 non-null    object 
     7   f5      100 non-null    float64
    dtypes: float64(3), int64(1), object(4)
    memory usage: 6.4+ KB
    None
    

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>age</th>
      <th>city</th>
      <th>f1</th>
      <th>f2</th>
      <th>f3</th>
      <th>f4</th>
      <th>f5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id01</td>
      <td>2.0</td>
      <td>서울</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>ENFJ</td>
      <td>91.297791</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id02</td>
      <td>9.0</td>
      <td>서울</td>
      <td>70.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ENFJ</td>
      <td>60.339826</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id03</td>
      <td>27.0</td>
      <td>서울</td>
      <td>61.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ISTJ</td>
      <td>17.252986</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id04</td>
      <td>75.0</td>
      <td>서울</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>INFP</td>
      <td>52.667078</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id05</td>
      <td>24.0</td>
      <td>서울</td>
      <td>85.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>ISFJ</td>
      <td>29.269869</td>
    </tr>
  </tbody>
</table>
</div>


<br/>**3. 스케일링 위한 객체 생성**<br/>

스케일링을 하기 위해선<br/>
항상 먼저 변형 정보를 저장할 변형 객체를 생성합니다.<br/>

```python
scaler = StandardScaler()         #scaler라는 객체 생성
```


<br/>**4. fit_transform 매서드 적용시키기**<br/>

[왜 fit_tranform 매서드를 사용하는 걸까](https://deepinsight.tistory.com/165)

```python
df['f5']=scaler.fit_transform(df[['f5']])  #f5 컬럼에 저장
```

<br/>**5. 중앙값 출력**<br/>

```python
print(df['f5'].median())     #중앙값 출력  
```

    64.11309937
    
