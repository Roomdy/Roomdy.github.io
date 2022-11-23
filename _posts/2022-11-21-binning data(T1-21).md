---
layout: single
title:  "var 함수를 통해 데이터의 분산 구하기"
---

<br/>**Data**<br/>

출처는 Kaggle의 Big Data Certification 입니다.<br/>
[출처 이동](https://www.kaggle.com/code/agileteam/py-t1-8-expected-questions/notebook)

<br/>**Question**<br/>

1. basic1 데이터 중 age 컬럼의 이상치 제거
2. 나이순으로 크기가 동일한 3그룹 나누기
3. 각 그룹의 중앙값 더하기

<br/>**0. 처음 등장하는 함수**<br/>

    pd.qcut(dateframe['기준 열'], 그룹 수, labels=[그룹1, 그룹2, 그룹3,...])
    
 + qcut 함수는 동일한 데이터 개수로 구간을 나눕니다.
 + 기준 열을 설정하면 크기순으로 그룹을 나눕니다.
 + labels 옵션으로 그룹명을 지정할 수 있습니다.

<br/>**1. 라이브러리 및 데이터 불러오기**<br/>

```python
import pandas as pd                                #판다스 불러오기
df=pd.read_csv('C:/Users/woody/data/basic1.csv')   #데이터 불러오기
```

<br/>**2. EDA**<br/>

```python
print(df.describe())  
df.head()
```

+ age에 음수값, 소수점이 있는 값이 있는 것을 알 수 있습니다.

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

<br/>**3. age 컬럼의 이상치 제거**<br/>


```python
df=df[df['age']>0]                         #age가 음수인 이상치 제거
df=df[(df['age']-np.ceil(df['age'])==0)]   #소수점이 있는 이상치 제거
df
```

+ (age 값 - 올림한 age 값 == 0)인 소수점이 없는 값만 df 변수에 저장합니다.


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
      <th>group</th>
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
      <td>group1</td>
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
      <td>group1</td>
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
      <td>group1</td>
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
      <td>group3</td>
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
      <td>group1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
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
      <td>group3</td>
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
      <td>group3</td>
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
      <td>group2</td>
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
      <td>group1</td>
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
      <td>group2</td>
    </tr>
  </tbody>
</table>
<p>90 rows × 9 columns</p>
</div>


<br/>**4. quct 함수로 그룹 나누기**<br/>

```python
pd.qcut(df['age'], 3)      #pd.qcut()함수로 그룹 나누기
```

+ 라벨명을 붙이기 전이므로 각 구간의 범위가 라벨명이 되었습니다.


    0      (0.999, 38.667]
    1      (0.999, 38.667]
    2      (0.999, 38.667]
    3      (73.333, 100.0]
    4      (0.999, 38.667]
                ...       
    95     (73.333, 100.0]
    96     (73.333, 100.0]
    97    (38.667, 73.333]
    98     (0.999, 38.667]
    99    (38.667, 73.333]
    Name: age, Length: 90, dtype: category
    Categories (3, interval[float64, right]): [(0.999, 38.667] < (38.667, 73.333] < (73.333, 100.0]]


<br/>**5. 라벨명 붙이기**<br/>

```python
df['group']=pd.qcut(df['age'], 3, labels=['group1', 'group2', 'group3'])
df.head()
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
      <th>group</th>
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
      <td>group1</td>
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
      <td>group1</td>
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
      <td>group1</td>
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
      <td>group3</td>
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
      <td>group1</td>
    </tr>
  </tbody>
</table>
</div>


+ .value_count()함수를 통해 각 그룹에 속한 데이터 개수를 확인할 수 있습니다.

```python
df['group'].value_counts()    #각 그룹의 value 카운트
```




    group1    30
    group2    30
    group3    30
    Name: group, dtype: int64

<br/>**6. 그룹별 중앙값 구하기**<br/>


```python
group1=df[df['group']=='group1']['age'].median()
group2=df[df['group']=='group2']['age'].median()
group3=df[df['group']=='group3']['age'].median()
group1, group2, group3
```




    (22.5, 55.5, 87.0)


<br/>**7. 합계 출력**<br/>

```python
print(group1 + group2 + group3)
```

    165.0
    


```python

```
