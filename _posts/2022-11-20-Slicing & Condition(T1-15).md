```python
import pandas as pd                                #판다스 불러오기
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
df=df.sort_values('age',ascending=False)       #'age' 기준 내림차순 정렬
df.head()                                      #데이터 확인
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




```python
df_top=df.head(20)        #df_top 변수에 상위 20개 값만 저장
df_top.head(5) 
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




```python
df_top['f1']=df_top['f1'].fillna(df_top['f1'].median())
  #df_top의 'f1' 변수에 결측치 중위값으로 채우기(fillna 함수 사용)
df_top.head()
```

    C:\Users\woody\AppData\Local\Temp\ipykernel_25788\437934613.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df_top['f1']=df_top['f1'].fillna(df_top['f1'].median())
    




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




```python
cond=df_top[(df_top['f4']=='ISFJ') & (df_top['f5']>=20)]   #( ) & ( ) 형식 안취하면 오류남
cond                                                       #condition(조건)의 약자
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
print(cond['f1'].mean())                       #'f1'의 평균 출력
```

    73.875
    
