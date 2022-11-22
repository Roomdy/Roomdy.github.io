```python
import pandas as pd
import numpy as np
df=pd.read_csv('C:/Users/woody/data/basic1.csv')
```


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




```python
top10=df['f1'].sort_values(ascending=False).iloc[9]
  #df의 'f1'컬럼을 내림차순으로 정렬 후 9번째 값 저장
  #dataframe.sort_values(by='기준 컬럼', ascending=False)
  #series.srot_values(ascending=False)
top10
```




    88.0




```python
df['f1']=df['f1'].fillna(top10)   #10번째 큰 값으로 결측치 대체
df.tail()
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




```python
result1=df['f1'].median()   #중복 제거 전 중앙값 저장
result1
```




    77.5




```python
df2=df.drop_duplicates(subset=['age'])  #drop_duplicate 함수로 중복값 제거
df2.info()                              #100행에서 71행으로 줄어들었음을 알 수 있습니다.
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
    


```python
result2=df2['f1'].median()   #중복값 제거 후 중앙값 저장
result2
```




    77.0




```python
abs(result1-result2)      #차이 출력
```




    0.5


