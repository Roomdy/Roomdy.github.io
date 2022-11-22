---
layout: single
title:  ""
---

<br/>**Data**<br/>

출처는 Kaggle의 Big Data Certification 입니다.<br/>
[출처 이동](https://www.kaggle.com/code/agileteam/py-t1-8-expected-questions/notebook)

<br/>**Question**<br/>
1. age 컬럼 상위 20개 데이터 구하기
2. f1 컬럼의 결측치를 중앙값으로 채우기
3. f4가 ISFJ이고, F5가 20 이상인 데이터의 f1 평균값 계산

<br/>**1. 라이브러리 및 데이터 불러오기**<br/>

```python
import pandas as pd                                #판다스 불러오기
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


<br/>**3. 'age' 컬럼 기준으로 내림차순 정렬**<br/>

+ ascending=False 옵션을 추가해야 내림차순으로 데이터가 정렬됩니다.

```python
df=df.sort_values('age',ascending=False)       
df.head()                                      #데이터 확인
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
      <th>36</th>
      <td>id37</td>
      <td>100.0</td>
      <td>부산</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>ESTP</td>
      <td>33.308999</td>
    </tr>
    <tr>
      <th>44</th>
      <td>id45</td>
      <td>97.0</td>
      <td>대구</td>
      <td>88.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>ENFJ</td>
      <td>13.049921</td>
    </tr>
    <tr>
      <th>91</th>
      <td>id92</td>
      <td>97.0</td>
      <td>경기</td>
      <td>78.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>INFP</td>
      <td>97.381034</td>
    </tr>
    <tr>
      <th>51</th>
      <td>id52</td>
      <td>97.0</td>
      <td>대구</td>
      <td>82.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ISFJ</td>
      <td>90.496999</td>
    </tr>
  </tbody>
</table>
</div>



<br/>**4. 상위 20개 데이터 저장**<br/>

```python
df_top=df.head(20)        #df_top 변수에 상위 20개 값만 저장
```

<br/>**5. fillna()함수를 사용하여 'f1'의 결측치 중앙값 대치**<br/>

```python
df_top['f1']=df_top['f1'].fillna(df_top['f1'].median())
  #df_top의 'f1' 변수에 결측치 중위값으로 채우기
df_top.head()
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
      <th>96</th>
      <td>id97</td>
      <td>100.0</td>
      <td>경기</td>
      <td>77.5</td>
      <td>0</td>
      <td>NaN</td>
      <td>INFP</td>
      <td>67.886373</td>
    </tr>
    <tr>
      <th>36</th>
      <td>id37</td>
      <td>100.0</td>
      <td>부산</td>
      <td>77.5</td>
      <td>0</td>
      <td>NaN</td>
      <td>ESTP</td>
      <td>33.308999</td>
    </tr>
    <tr>
      <th>44</th>
      <td>id45</td>
      <td>97.0</td>
      <td>대구</td>
      <td>88.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>ENFJ</td>
      <td>13.049921</td>
    </tr>
    <tr>
      <th>91</th>
      <td>id92</td>
      <td>97.0</td>
      <td>경기</td>
      <td>78.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>INFP</td>
      <td>97.381034</td>
    </tr>
    <tr>
      <th>51</th>
      <td>id52</td>
      <td>97.0</td>
      <td>대구</td>
      <td>82.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ISFJ</td>
      <td>90.496999</td>
    </tr>
  </tbody>
</table>
</div>


<br/>**6. cond 변수에 조건 저장**<br/>

+ 조건을 cond(condition의 약자) 변수에 저장해서 조회하면 인덱싱에 편리합니다.

```python
cond=(df_top['f4']=='ISFJ') & (df_top['f5']>=20)]  
  #( ) & ( ) 형식을 안취하면 오류가 발생합니다.
df_top[cond]                                                   
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
      <th>51</th>
      <td>id52</td>
      <td>97.0</td>
      <td>대구</td>
      <td>82.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ISFJ</td>
      <td>90.496999</td>
    </tr>
    <tr>
      <th>72</th>
      <td>id73</td>
      <td>90.0</td>
      <td>경기</td>
      <td>77.5</td>
      <td>1</td>
      <td>NaN</td>
      <td>ISFJ</td>
      <td>73.586397</td>
    </tr>
    <tr>
      <th>62</th>
      <td>id63</td>
      <td>88.0</td>
      <td>경기</td>
      <td>86.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ISFJ</td>
      <td>73.586397</td>
    </tr>
    <tr>
      <th>80</th>
      <td>id81</td>
      <td>86.0</td>
      <td>경기</td>
      <td>50.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>ISFJ</td>
      <td>37.113739</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(df_top[cond]['f1'].mean())                       #'f1'의 평균 출력
```

    73.875
    
