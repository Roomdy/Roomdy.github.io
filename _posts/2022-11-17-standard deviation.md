```python
import pandas as pd                               #판다스 불러오기
import numpy as np                                #넘파이 불러오기
df=pd.read_csv('C:/Users/woody/data/basic1.csv')  #데이터 불러오기
```


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
    


```python
enfj_std=enfj['f1'].std()  #enfj변수의 'f1' 표준편차 저장
infp_std=infp['f1'].std()  #infp변수의 'f1' 표준편차 저장
enfj_std, infp_std
```




    (17.727097901235837, 23.586719427112648)




```python
print(abs(enfj_std-infp_std))  #표준편차 차이의 절대값 계산
```

    5.859621525876811
    