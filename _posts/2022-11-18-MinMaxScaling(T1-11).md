---
layout: single
title:  "MinMaxScaler로 데이터 표준화 시키기"
categories: 빅데이터분석기사
tags: [BigData, Certification]
---

<br/>**Data**<br/>

출처는 Kaggle의 Big Data Certification 입니다.<br/>
[출처 이동](https://www.kaggle.com/code/agileteam/py-t1-8-expected-questions/notebook)

<br/>**Question**<br/>

1. 'f5'컬럼을 min-max 스케일 변환
2. 상위 5%와 하위 5% 값의 합 구하기

<br/>**0. 처음 등장하는 함수**<br/>

    from sklearn.preprocessing import MinMaxScaler
     #sklearn 라이브러리 preprocessing(전처리) 모듈의 MinMaxScaler 함수 불러오기
     #MinMaxScaler는 데이터를 최솟값 0, 최댓값 1을 기준으로 스케일링 해줍니다.
     #주로 변수가 연속형 범주인 회귀 문제에서 사용됩니다.
     
    
<br/>**1. 라이브러리 및 데이터 불러오기**<br/>

```python
import pandas as pd                                #판다스 불러오기
import numpy as np                                 #넘파이 불러오기
from sklearn.preprocessing import MinMaxScaler
  #sklearn 라이브러리 preprocessing 모듈의 MinMaxScaler 함수 불러오기
df=pd.read_csv('.../data/basic1.csv')   #데이터 불러오기
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


<br/>**3. 객체 생성**<br/>

스케일링을 사용할 때는 항상 변형 정보를 저장하는 변형 객체를 먼저 생성합니다.<br/>

```python
scaler=MinMaxScaler()                       #scaler 객체 생성
```


<br/>**4. fit_transform 매서드 적용**<br/>

왜 fit과 transform 매서드가 사용되어야 하는지<br/>
transform()과 fit_transform()의 차이는 무엇인지<br/>
아주 잘 설명해둔 글이 있어 소개해드립니다.<br/>
[블로그 이동](https://deepinsight.tistory.com/165)<br/>

```python
df['f5']=scaler.fit_transform(df[['f5']])   #.fit_transform( ) 명령
df.head(10)                                 #데이터 확인
```


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
      <td>0.919533</td>
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
      <td>0.570252</td>
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
      <td>0.084129</td>
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
      <td>0.483685</td>
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
      <td>0.219708</td>
    </tr>
    <tr>
      <th>5</th>
      <td>id06</td>
      <td>22.0</td>
      <td>서울</td>
      <td>57.0</td>
      <td>0</td>
      <td>vip</td>
      <td>INTP</td>
      <td>0.116582</td>
    </tr>
    <tr>
      <th>6</th>
      <td>id07</td>
      <td>36.3</td>
      <td>서울</td>
      <td>60.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ISFJ</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>id08</td>
      <td>38.0</td>
      <td>서울</td>
      <td>101.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>INFJ</td>
      <td>0.833646</td>
    </tr>
    <tr>
      <th>8</th>
      <td>id09</td>
      <td>3.3</td>
      <td>서울</td>
      <td>35.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>ESFJ</td>
      <td>0.084129</td>
    </tr>
    <tr>
      <th>9</th>
      <td>id10</td>
      <td>95.0</td>
      <td>서울</td>
      <td>74.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ISFP</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>

'f5'컬럼의 스케일이 정상적으로 조정되었음을 알 수 있습니다.



<br/>**5. qunatile 함수로 상하위값 구하기**<br/>

사분위수는 자료를 오름차순(small -> big)으로 배열해 4등분한 값이므로<br/>
처음 5%의 데이터가 하위5%값이 되고<br/>
마지막 5%의 데이터가 상위5%값이 됩니다.<br/>

```python
df_lower=df['f5'].quantile(0.05)            #하위 5%값
df_upper=df['f5'].quantile(0.95)            #상위 5%값
df_lower, df_upper
```




    (0.03670782406038746, 0.9881662742993513)

<br/>**6. 합계 **<br/>


```python
print(df_lower+df_upper)                     
```

    1.0248740983597389
    
