```python
import pandas as pd                                  #판다스 불러오기
import numpy as np                                   #넘파이 불러오기
from sklearn.preprocessing import StandardScaler
   #sklearn 라이브러리의 preprocessing 모듈 중 StandardScaler 함수 가져오기
df=pd.read_csv('C:/Users/woody/data/basic1.csv')     #데이터 불러오기
```


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




```python
scaler = StandardScaler()         #scaler라는 객체 생성
```




    StandardScaler()




```python
scaler.fit_transform(df[['f5']])[0:9]     #객체에 fit_transform() 명령
```




    array([[ 1.22081535],
           [ 0.1273431 ],
           [-1.39453546],
           [-0.14366749],
           [-0.9700848 ],
           [-1.29293552],
           [-1.65791175],
           [ 0.95193593],
           [-1.39453546]])




```python
df['f5']=scaler.fit_transform(df[['f5']])  #df에 저장
```


```python
print(df['f5'].median())     #중앙값 출력  
```

    64.11309937
    
