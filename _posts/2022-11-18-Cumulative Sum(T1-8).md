---
layout: single
title:  "cumsum 함수로 누적합 계산하기"
categories: 빅데이터분석기사
tags: [BigData, Certification]
---

<br/>**Data**<br/>

출처는 Kaggle의 Big Data Certification 입니다.<br/>
[출처 이동](https://www.kaggle.com/code/agileteam/py-t1-8-expected-questions/notebook)

<br/>**Question**<br/>

1. 'f2'컬럼이 1인 데이터의 'f1'컬럼 누적합 계산
2. 누적합 결측치는 바로 뒤의 값으로 채우기<br/>
   (단, 결측치 바로 뒤의 값이 없으면 다음에 나오는 값으로)
3. 누적합의 평균값 출력

<br/>**0. 처음 등장하는 함수**<br/>

    Series.cumsum()                   #데이터의 누적합 계산
    Dataframe.fillna(method='bfill')  #결측치를 바로 다음값으로 대체

<br/>**1. 라이브러리 및 데이터 불러오기**<br/>

```python
import pandas as pd                                #판다스 불러오기
import numpy as np                                 #넘파이 불러오기
df=pd.read_csv('.../data/basic1.csv')   
```

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


<br/>**3. cumsum함수로 누적합 계산**<br/>

```python
df2=df[df['f2']==1]['f1'].cumsum()   #df2에 조건을 만족하는 누적합 데이터 저장
df2                                  #데이터 확인   
```

결측치가 존재함을 알 수 있습니다.(인덱스 16, 20,...)<br/>


    1       70.0
    2      131.0
    6      191.0
    7      292.0
    9      366.0
    13     416.0
    14     483.0
    16       NaN
    19     534.0
    20       NaN
    21     606.0
    22     681.0
    25     738.0
    27     772.0
    33       NaN
    35     849.0
    37       NaN
    46     924.0
    49    1002.0
    51    1084.0
    53       NaN
    55       NaN
    58       NaN
    62    1170.0
    65       NaN
    66    1222.0
    69    1318.0
    72       NaN
    77    1414.0
    80    1464.0
    82    1514.0
    86       NaN
    88    1580.0
    91    1658.0
    93       NaN
    94    1701.0
    95    1754.0
    Name: f1, dtype: float64


<br/>**4. fillna함수로 결측치 대체**<br/>

```python
df2=df2.fillna(method='bfill')            #결측치 대체
df2
```

결측치가 다음값으로 채워졌음을 알 수 있습니다.<br/>
만약, 바로 직전값으로 결측치를 대체하고 싶다면 'method='ffill'을 사용합니다.

    1       70.0
    2      131.0
    6      191.0
    7      292.0
    9      366.0
    13     416.0
    14     483.0
    16     534.0
    19     534.0
    20     606.0
    21     606.0
    22     681.0
    25     738.0
    27     772.0
    33     849.0
    35     849.0
    37     924.0
    46     924.0
    49    1002.0
    51    1084.0
    53    1170.0
    55    1170.0
    58    1170.0
    62    1170.0
    65    1222.0
    66    1222.0
    69    1318.0
    72    1414.0
    77    1414.0
    80    1464.0
    82    1514.0
    86    1580.0
    88    1580.0
    91    1658.0
    93    1701.0
    94    1701.0
    95    1754.0
    Name: f1, dtype: float64


<br/>**4. 대체 후 평균값 출력**<br/>



```python
df2.mean()
```




    980.3783783783783


