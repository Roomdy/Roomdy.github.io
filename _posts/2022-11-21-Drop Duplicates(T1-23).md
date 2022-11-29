---
layout: single
title:  "drop_duplicates 함수로 중복값 제거하기"
---

<br/>**Data**<br/>

출처는 Kaggle의 Big Data Certification 입니다.<br/>
[출처 이동](https://www.kaggle.com/code/agileteam/py-t1-8-expected-questions/notebook)

<br/>**Question**<br/>

1. f1 컬럼의 결측치를 내림차순 10번째 값으로 채우기
2. age 컬럼의 중복값 제거 전과 후의 f1 컬럼의 중앙값 차이 구하기

<br/>**0. 처음 등장하는 함수**<br/>

    dataframe.drop_duplicates(subset=['대상 컬럼'])
    
+ 대상 컬럼을 기준으로 중복된 데이터 발생시 뒤에 나오는 행을 삭제합니다.


<br/>**1. 라이브러리 및 데이터 불러오기**<br/>

```python
import pandas as pd                                #판다스 불러오기
df=pd.read_csv('C:/Users/woody/data/basic1.csv')   #데이터 불러오기
```

<br/>**2. EDA**<br/>

```python
df.info()
df.tail()
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
      <th>95</th>
      <td>id96</td>
      <td>92.0</td>
      <td>경기</td>
      <td>53.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ENTJ</td>
      <td>52.667078</td>
    </tr>
    <tr>
      <th>96</th>
      <td>id97</td>
      <td>100.0</td>
      <td>경기</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>INFP</td>
      <td>67.886373</td>
    </tr>
    <tr>
      <th>97</th>
      <td>id98</td>
      <td>39.0</td>
      <td>경기</td>
      <td>58.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>INFP</td>
      <td>98.429899</td>
    </tr>
    <tr>
      <th>98</th>
      <td>id99</td>
      <td>1.0</td>
      <td>경기</td>
      <td>47.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>ESFJ</td>
      <td>97.381034</td>
    </tr>
    <tr>
      <th>99</th>
      <td>id100</td>
      <td>47.0</td>
      <td>경기</td>
      <td>53.0</td>
      <td>0</td>
      <td>vip</td>
      <td>ESFP</td>
      <td>33.308999</td>
    </tr>
  </tbody>
</table>
</div>


<br/>**3. f1 컬럼을 내림차순으로 정렬 후 10번째 값 저장하기**<br/>

```python
top10=df['f1'].sort_values(ascending=False).iloc[9]
  #df의 'f1'컬럼을 내림차순으로 정렬 후 10번째 값 저장(iloc는 인덱스 기반)
top10
```

    88.0

+ 참고로 sort+values 함수는<br/>데이터프레임일 경우 dataframe.sort_values(by='기준 컬럼', ascending=False) 형식<br/>시리즈일 경우 series.srot_values(ascending=False)형식을 취합니다.

<br/>**4. f1 컬럼 결측치 대치**<br/>


```python
df['f1']=df['f1'].fillna(top10) 
df.tail()
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
      <th>95</th>
      <td>id96</td>
      <td>92.0</td>
      <td>경기</td>
      <td>53.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ENTJ</td>
      <td>52.667078</td>
    </tr>
    <tr>
      <th>96</th>
      <td>id97</td>
      <td>100.0</td>
      <td>경기</td>
      <td>88.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>INFP</td>
      <td>67.886373</td>
    </tr>
    <tr>
      <th>97</th>
      <td>id98</td>
      <td>39.0</td>
      <td>경기</td>
      <td>58.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>INFP</td>
      <td>98.429899</td>
    </tr>
    <tr>
      <th>98</th>
      <td>id99</td>
      <td>1.0</td>
      <td>경기</td>
      <td>47.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>ESFJ</td>
      <td>97.381034</td>
    </tr>
    <tr>
      <th>99</th>
      <td>id100</td>
      <td>47.0</td>
      <td>경기</td>
      <td>53.0</td>
      <td>0</td>
      <td>vip</td>
      <td>ESFP</td>
      <td>33.308999</td>
    </tr>
  </tbody>
</table>
</div>

<br/>**5.중복값 제거 전 f1 컬럼의 중앙값 저장**<br/>


```python
result1=df['f1'].median()   #중복 제거 전 중앙값 저장
result1
```




    77.5

<br/>**6. drop_duplicates 함수로 중복값 제거**<br/>


```python
df2=df.drop_duplicates(subset=['age'])
df2.info()        #100행에서 71행으로 줄어들었음을 알 수 있습니다.
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 71 entries, 0 to 98
    Data columns (total 8 columns):
     #   Column  Non-Null Count  Dtype  
    ---  ------  --------------  -----  
     0   id      71 non-null     object 
     1   age     71 non-null     float64
     2   city    71 non-null     object 
     3   f1      71 non-null     float64
     4   f2      71 non-null     int64  
     5   f3      4 non-null      object 
     6   f4      71 non-null     object 
     7   f5      71 non-null     float64
    dtypes: float64(3), int64(1), object(4)
    memory usage: 5.0+ KB
    
<br/>**7. 중복값 제거 후 f1 컬럼의 중앙값 저장**<br/>

```python
result2=df2['f1'].median()  
result2
```




    77.0


<br/>**9. 중앙값 차이 출력**<br/>

```python
abs(result1-result2)   
```




    0.5


