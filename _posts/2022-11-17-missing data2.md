---
layout: single
title:  "map함수를 활용한 결측치 처리"
---

<br/>**Data**<br/>
data의 출처는 kaggle의 Big Data Certification KR 입니다.
[바로 이동](https://www.kaggle.com/code/agileteam/py-t1-3-map-expected-questions/notebook)

<br/>**Question**<br/>

1. 결측치가 80% 이상인 컬럼 삭제
2. 결측치가 80% 미만인 컬럼 'city'별 중앙값으로 대체
3. 'f1'컬럼의 평균값 출력

<br/>**0. 처음 등장하는 함수**<br/>

    .map({key1:value1, key2:value2, ... })   
     # 딕셔너리에 정의한 데이터를 변수에 적용할 수 있는 함수
     
map함수에 대해선 아직도 헷갈리는 부분이 많습니다.<br/>
해당 페이지에서 먼저 map함수의 기능을 살펴본 후 문제에 접근하면 이해가 쉬울듯 합니다.

[map 다루기](https://www.dacon.io/codeshare/586)

<br/>**1. 라이브러리 및 데이터 불러오기**<br/>

```python
import pandas as pd                                 #판다스 불러오기
import numpy as np                                  #넘파이 불러오기
df=pd.read_csv('C:/Users/woody/data/basic1.csv')    #데이터 불러오기
```

<br/>**2. EDA**<br/>

```python
#EDA
print(df.shape)         #행열 개수 확인
print(df.describe())    #기초 통계량 확인
df.head()               #데이터 확인
```

    (100, 8)
                  age          f1          f2          f5
    count  100.000000   69.000000  100.000000  100.000000
    mean    50.963000   66.043478    0.650000   56.734537
    std     30.442759   19.453893    0.715979   28.454244
    min    -13.500000   12.000000    0.000000    9.796378
    25%     26.875000   51.000000    0.000000   29.269869
    50%     52.500000   63.000000    1.000000   64.113099
    75%     77.000000   78.000000    1.000000   81.025055
    max    100.000000  111.000000    2.000000   98.429899
    




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


<br/>**3. 결측치 개수와 총 데이터 개수 확인**<br/>

```python
print(df.isnull().sum())    #결측치 개수 
print(df.shape[0])       #전체 행 개수 = 데이터 개수
```

    id       0
    age      0
    city     0
    f1      31
    f2       0
    f3      95
    f4       0
    f5       0
    dtype: int64
    100
    
<br/>**3. 결측치 비율 조회**<br/>
논리: 결측치 개수/ 총 데이터 개수 == 결측치 비율

```python
print(df.isnull().sum()/df.shape[0])  
```

    id      0.00
    age     0.00
    city    0.00
    f1      0.31
    f2      0.00
    f3      0.95
    f4      0.00
    f5      0.00
    dtype: float64
    
<br/>**4. 결측치가 80프로 이상인 'f3' 컬럼 삭제**<br/>

```python
del df['f3']  #결측치가 80% 이상인 'f3' column 삭제
df.head()     #데이터 확인
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
      <td>ISFJ</td>
      <td>29.269869</td>
    </tr>
  </tbody>
</table>
</div>


<br/>**4. 결측치가 80프로 미만인 'f1' 컬럼의 'city'별 중앙값 계산**<br/>

```python
print(df.groupby(['city'])['f1'].median())       #city별 중앙값 확인
           #해석: 'city' 범주별로, 'f1' 범주에 대한 중앙값
```

    city
    경기    58.0
    대구    75.0
    부산    62.0
    서울    68.0
    Name: f1, dtype: float64
    
<br/>**5. 각 변수에 중앙값 저장**<br/>

```python
k, d, b, s = df.groupby(['city'])['f1'].median()
```

<br/>**5. 결측치 대체 전 데이터 확인**<br/>

```python
df[18:21]
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
      <th>f4</th>
      <th>f5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>18</th>
      <td>id19</td>
      <td>53.0</td>
      <td>서울</td>
      <td>NaN</td>
      <td>0</td>
      <td>ISFP</td>
      <td>83.685380</td>
    </tr>
    <tr>
      <th>19</th>
      <td>id20</td>
      <td>11.0</td>
      <td>서울</td>
      <td>51.0</td>
      <td>1</td>
      <td>INTJ</td>
      <td>91.297791</td>
    </tr>
    <tr>
      <th>20</th>
      <td>id21</td>
      <td>90.0</td>
      <td>부산</td>
      <td>NaN</td>
      <td>1</td>
      <td>ISFP</td>
      <td>29.269869</td>
    </tr>
  </tbody>
</table>
</div>



<br/>**6. map함수를 활용한 결측치 대체**<br/>

```python
df['f1']=df['f1'].fillna(df['city'].map({'경기':k, '대구':d, '부산':b, '서울': s}))
df[18:21] #대치 후 데이터 확인
```
+ 코드 해석: f1 컬럼의 결측치를 채울건데, city 컬럼을 기준으로 map 함수를 통해 사전(dict)에 정의된 내용을 일대일로 대응시켜 채울래.
+ 참고로 코드 해석은 항상 왼쪽에서 오른쪽으로 읽습니다.


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
      <th>f4</th>
      <th>f5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>18</th>
      <td>id19</td>
      <td>53.0</td>
      <td>서울</td>
      <td>68.0</td>
      <td>0</td>
      <td>ISFP</td>
      <td>83.685380</td>
    </tr>
    <tr>
      <th>19</th>
      <td>id20</td>
      <td>11.0</td>
      <td>서울</td>
      <td>51.0</td>
      <td>1</td>
      <td>INTJ</td>
      <td>91.297791</td>
    </tr>
    <tr>
      <th>20</th>
      <td>id21</td>
      <td>90.0</td>
      <td>부산</td>
      <td>62.0</td>
      <td>1</td>
      <td>ISFP</td>
      <td>29.269869</td>
    </tr>
  </tbody>
</table>
</div>


<br/>**7. 'f1'컬럼의 평균값 출력**<br/>

```python
print(df['f1'].mean())
```

    65.52
    
