```python
import pandas as pd                                #판다스 불러오기
import numpy as np                                 #넘파이 불러오기
from sklearn.preprocessing import MinMaxScaler
  #sklearn 라이브러리 preprocessing 모듈의 MinMaxScaler 함수 불러오기
df=pd.read_csv('C:/Users/woody/data/basic1.csv')   #데이터 불러오기
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
scaler=MinMaxScaler()                       #scaler 객체 생성
```


```python
df['f5']=scaler.fit_transform(df[['f5']])   #.fit_transform( ) 명령
df.head(10)                                 #데이터 확인
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




```python
df_lower=df['f5'].quantile(0.05)            #하위 5%값
df_upper=df['f5'].quantile(0.95)            #상위 5%값
df_lower, df_upper
```




    (0.03670782406038746, 0.9881662742993513)




```python
print(df_lower+df_upper)                     #합
```

    1.0248740983597389
    
