---
layout: single
title:  "결측치 처리 후 그룹 합계 구하기"
---

<br/>**Data**<br/>
data 출처는 kaggle의 Big Data Certification KR입니다.<br/>
[바로 이동](https://www.kaggle.com/code/agileteam/py-t1-6-expected-questions/notebook)

<br/>**Question**<br/>

1. 'f1'컬럼의 결측 데이터 제거
2. 'city'와 'f2'칼럼을 기준으로 그룹 합계 구하기
3. ('city'== '경기' & 'f2' == 0)인 조건에 만족하는 'f1'값 구하기

<br/>**0. 새롭게 등장하는 함수**<br/>

    data.dropna(subset = ['컬럼명1', '컬럼명2', ...])
     #특정 컬럼의 결측치가 있는 행을 삭제할 때 사용합니다.
    data.groupby(['기준컬럼1','기준컬럼2',...])
     #기준컬럼을 기준으로 정렬
    data.groupby(['기준컬럼1','기준컬럼2',...]).sum()
     #기준컬럼을 기준으로 그룹합계 

<br/>**1. 라이브러리 및 데이터 불러오기**<br/>

```python
import pandas as pd                               #판다스 불러오기
import numpy as np                                #넘파이 불러오기
df=pd.read_csv('...data/basic1.csv')  #데이터 불러오기
```

<br/>**2. EDA**<br/>

```python
print(df.shape)          #행, 열 개수 확인
print(df.describe())     #기초통계량 확인
df.head()                #데이터 확인
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

<br/>**3. 'f1'컬럼의 결측치 제거**<br/>

```python
df=df.dropna(subset='f1')    #dropna함수로 'f1' 칼럼의 결측치 제거
df.head()                    #데이터 확인
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
    <tr>
      <th>5</th>
      <td>id06</td>
      <td>22.0</td>
      <td>서울</td>
      <td>57.0</td>
      <td>0</td>
      <td>vip</td>
      <td>INTP</td>
      <td>20.129444</td>
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
      <td>9.796378</td>
    </tr>
  </tbody>
</table>
</div>



<br/>**4. 'city'와 'f2' 기준 그룹합계 구하기**<br/>

```python
df.groupby(['city','f2']).sum()  #groupby 함수로 조건별 합계 계산
```





</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>age</th>
      <th>f1</th>
      <th>f5</th>
    </tr>
    <tr>
      <th>city</th>
      <th>f2</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">경기</th>
      <th>0</th>
      <td>720.4</td>
      <td>833.0</td>
      <td>943.944823</td>
    </tr>
    <tr>
      <th>1</th>
      <td>696.0</td>
      <td>670.0</td>
      <td>657.241212</td>
    </tr>
    <tr>
      <th>2</th>
      <td>239.0</td>
      <td>311.0</td>
      <td>362.300060</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">대구</th>
      <th>0</th>
      <td>387.0</td>
      <td>527.0</td>
      <td>183.199568</td>
    </tr>
    <tr>
      <th>1</th>
      <td>217.6</td>
      <td>235.0</td>
      <td>241.333824</td>
    </tr>
    <tr>
      <th>2</th>
      <td>140.0</td>
      <td>211.0</td>
      <td>79.667919</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">부산</th>
      <th>0</th>
      <td>331.0</td>
      <td>389.0</td>
      <td>284.371097</td>
    </tr>
    <tr>
      <th>1</th>
      <td>188.7</td>
      <td>315.0</td>
      <td>299.270973</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-13.5</td>
      <td>47.0</td>
      <td>67.886373</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">서울</th>
      <th>0</th>
      <td>145.0</td>
      <td>278.0</td>
      <td>218.528577</td>
    </tr>
    <tr>
      <th>1</th>
      <td>315.3</td>
      <td>534.0</td>
      <td>438.485010</td>
    </tr>
    <tr>
      <th>2</th>
      <td>68.3</td>
      <td>207.0</td>
      <td>126.661135</td>
    </tr>
  </tbody>
</table>
</div>


<br/>**5. 'city'== '경기' & 'f2' == 0인 조건에 만족하는 'f1'값 구하기**<br/>

```python
df_f1=df[(df['city']=='경기') & (df['f2']==0)]['f1']
 #데이터프레임[행 인덱스][열 인덱스]
```





    68    85.0
    71    97.0
    73    98.0
    74    47.0
    75    12.0
    76    31.0
    79    60.0
    83    44.0
    84    55.0
    87    75.0
    90    72.0
    92    57.0
    98    47.0
    99    53.0
    Name: f1, dtype: float64



