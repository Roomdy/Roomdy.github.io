---
layout: single
title:  "조건에 맞는 데이터의 표준편차 계산"
---

<br/>**Data**<br/>
data 출처는 kaggle의 Big Data Certification KR입니다.
[바로 이동](https://www.kaggle.com/code/agileteam/py-t1-5-expected-questions/notebook)

<br/>**Question**<br/>

1. 'f4'컬럼의 값이 'ENFJ'와 'INFP'인 행의 'f1'값 표준편차 구하기
2. 표준편차 차이를 절대값으로 구하기

<br/>**1. 라이브러리 및 데이터 불러오기**<br/>

```python
import pandas as pd                               #판다스 불러오기
import numpy as np                                #넘파이 불러오기
df=pd.read_csv('C:/Users/woody/data/basic1.csv')  #데이터 불러오기
```

<br/>**2. 'f4'가 'ENFJ'와 'INFP'인 데이터 저장**<br/>

```python
enfj=df[df['f4']=='ENFJ']   #f4가 ENFJ인 데이터 저장
infp=df[df['f4']=='INFP']   #f4가 INFP인 데이터 저장
print(enfj)
print(infp)
```

          id   age city    f1  f2   f3    f4         f5
    0   id01   2.0   서울   NaN   0  NaN  ENFJ  91.297791
    1   id02   9.0   서울  70.0   1  NaN  ENFJ  60.339826
    32  id33  47.0   부산  94.0   0  NaN  ENFJ  17.252986
    40  id41  81.0   대구  55.0   0  NaN  ENFJ  37.113739
    44  id45  97.0   대구  88.0   0  NaN  ENFJ  13.049921
    53  id54  53.0   대구   NaN   1  NaN  ENFJ  69.730313
          id    age city    f1  f2   f3    f4         f5
    3   id04   75.0   서울   NaN   2  NaN  INFP  52.667078
    33  id34   65.0   부산   NaN   1  NaN  INFP  48.431184
    76  id77   77.0   경기  31.0   0  NaN  INFP  98.429899
    91  id92   97.0   경기  78.0   1  NaN  INFP  97.381034
    96  id97  100.0   경기   NaN   0  NaN  INFP  67.886373
    97  id98   39.0   경기  58.0   2  NaN  INFP  98.429899
    
<br/>**3. 각 데이터의 'f1' 표준편차 저장**<br/>

```python
enfj_std=enfj['f1'].std()  #enfj변수의 'f1' 표준편차 저장
infp_std=infp['f1'].std()  #infp변수의 'f1' 표준편차 저장
enfj_std, infp_std
```




    (17.727097901235837, 23.586719427112648)

<br/>**3. 표준편차 차이의 절대값 계산**<br/>


```python
print(abs(enfj_std-infp_std))  
```

    5.859621525876811
    
