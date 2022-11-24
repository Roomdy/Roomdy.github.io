---
layout: single
title:  "(기초)퍼이썬으로 예측 모형 만들기"
---

<br/>**Question**<br/>

1. 고객의 상품 구매 속성으로 성별 예측 모형 만들기
2. 고객의 성별 예측값(남자일 확률) 데이터 생성

<br/>**Data**<br/>

+ y_train.csv: 고객 성별 데이터(학습용)
+ x_train.csv, x_test.csv: 고객의 상품 구매 속성(학습용 및 검증용)

<br/>#1. 데이터 탐색<br/>

<br/>**1-1 라이브러리 및 데이터 불러오기**<br/>

```python
import pandas as pd
x_train = pd.read_csv('C:/Users/woody/data2/x_train.csv',encoding='CP949')
x_test = pd.read_csv('C:/Users/woody/data2/x_test.csv',encoding='CP949')
y_train = pd.read_csv('C:/Users/woody/data2/y_train.csv',encoding='CP949')
```

<br/>**1-2 데이터 탐색 수행(EDA)**<br/>

```python
print(x_train.info(), x_test.info(), y_train.info())
 #x의 환불금액에 결측치가 많음을 알 수 있습니다.
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3500 entries, 0 to 3499
    Data columns (total 10 columns):
     #   Column   Non-Null Count  Dtype  
    ---  ------   --------------  -----  
     0   cust_id  3500 non-null   int64  
     1   총구매액     3500 non-null   int64  
     2   최대구매액    3500 non-null   int64  
     3   환불금액     1205 non-null   float64
     4   주구매상품    3500 non-null   object 
     5   주구매지점    3500 non-null   object 
     6   내점일수     3500 non-null   int64  
     7   내점당구매건수  3500 non-null   float64
     8   주말방문비율   3500 non-null   float64
     9   구매주기     3500 non-null   int64  
    dtypes: float64(3), int64(5), object(2)
    memory usage: 273.6+ KB
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2482 entries, 0 to 2481
    Data columns (total 10 columns):
     #   Column   Non-Null Count  Dtype  
    ---  ------   --------------  -----  
     0   cust_id  2482 non-null   int64  
     1   총구매액     2482 non-null   int64  
     2   최대구매액    2482 non-null   int64  
     3   환불금액     871 non-null    float64
     4   주구매상품    2482 non-null   object 
     5   주구매지점    2482 non-null   object 
     6   내점일수     2482 non-null   int64  
     7   내점당구매건수  2482 non-null   float64
     8   주말방문비율   2482 non-null   float64
     9   구매주기     2482 non-null   int64  
    dtypes: float64(3), int64(5), object(2)
    memory usage: 194.0+ KB
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3500 entries, 0 to 3499
    Data columns (total 2 columns):
     #   Column   Non-Null Count  Dtype
    ---  ------   --------------  -----
     0   cust_id  3500 non-null   int64
     1   gender   3500 non-null   int64
    dtypes: int64(2)
    memory usage: 54.8 KB
    None None None
    


```python
x_train.head(3)  #고객의 상품 구매 속성(훈련용)
```


</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cust_id</th>
      <th>총구매액</th>
      <th>최대구매액</th>
      <th>환불금액</th>
      <th>주구매상품</th>
      <th>주구매지점</th>
      <th>내점일수</th>
      <th>내점당구매건수</th>
      <th>주말방문비율</th>
      <th>구매주기</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>68282840</td>
      <td>11264000</td>
      <td>6860000.0</td>
      <td>기타</td>
      <td>강남점</td>
      <td>19</td>
      <td>3.894737</td>
      <td>0.527027</td>
      <td>17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2136000</td>
      <td>2136000</td>
      <td>300000.0</td>
      <td>스포츠</td>
      <td>잠실점</td>
      <td>2</td>
      <td>1.500000</td>
      <td>0.000000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>3197000</td>
      <td>1639000</td>
      <td>NaN</td>
      <td>남성 캐주얼</td>
      <td>관악점</td>
      <td>2</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
x_test.head(3)   #고객의 상품 구매 속성(검증용)
```


</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cust_id</th>
      <th>총구매액</th>
      <th>최대구매액</th>
      <th>환불금액</th>
      <th>주구매상품</th>
      <th>주구매지점</th>
      <th>내점일수</th>
      <th>내점당구매건수</th>
      <th>주말방문비율</th>
      <th>구매주기</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3500</td>
      <td>70900400</td>
      <td>22000000</td>
      <td>4050000.0</td>
      <td>골프</td>
      <td>부산본점</td>
      <td>13</td>
      <td>1.461538</td>
      <td>0.789474</td>
      <td>26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3501</td>
      <td>310533100</td>
      <td>38558000</td>
      <td>48034700.0</td>
      <td>농산물</td>
      <td>잠실점</td>
      <td>90</td>
      <td>2.433333</td>
      <td>0.369863</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3502</td>
      <td>305264140</td>
      <td>14825000</td>
      <td>30521000.0</td>
      <td>가공식품</td>
      <td>본  점</td>
      <td>101</td>
      <td>14.623762</td>
      <td>0.083277</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
y_train.head(3)  #고객의 성별 데이터
```



</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cust_id</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>

<br/>#2. 데이터 전처리<br/>

<br/>**2-1 불필요한 컬럼 삭제**<br/>

+ 목적에 맞는 데이터들로 구성되어 있는지 확인합니다.
+ 데이터의 cust_id는 성별 예측을 위한 정보가 아님을 알 수 있습니다.

```python

x_train=x_train.drop(columns=['cust_id'])
x_test=x_test.drop(columns=['cust_id'])
y_train = y_train.drop(columns=['cust_id'])
```

<br/>**2-2 결측치 제거**<br/>

+ info() 탐색 결과 환불금액에 결측치가 많음을 알 수 있었습니다.
+ 환불금액이 없다는 것은 활불내역이 없다는 것을 뜻합니다.
+ 그러므로 fillna(0) 함수로 결측치를 0으로 채워줍니다.

```python
x_train['환불금액']=x_train['환불금액'].fillna(0)
x_test['환불금액']=x_test['환불금액'].fillna(0)
```

<br/>**2-3 범주형 변수 인코딩**<br/>

+ 모델 학습을 위해 범주형 변수들은 인코딩을 수행합니다.
+ 
```python
from sklearn.preprocessing import LabelEncoder
# sklearn의 preprocessing 모듈에서 LabelEncoder 함수를 불러옵니다.
```


```python
encoder=LabelEncoder()   #인코딩을 위한 객체를 생성합니다.
```


```python
x_train['주구매상품'] = encoder.fit_transform(x_train['주구매상품'])
print(encoder.classes_)
#주구매상품 컬럼에 변환된 데이터를 저장합니다
#classes_ 키워드: 라벨 인코딩의 변환 순서를 반환합니다.
```

    ['가공식품' '가구' '건강식품' '골프' '구두' '기타' '남성 캐주얼' '남성 트랜디' '남성정장' '농산물' '대형가전'
     '디자이너' '란제리/내의' '명품' '모피/피혁' '보석' '생활잡화' '섬유잡화' '셔츠' '소형가전' '수산품' '스포츠'
     '시티웨어' '식기' '아동' '악기' '액세서리' '육류' '일용잡화' '젓갈/반찬' '주류' '주방가전' '주방용품'
     '차/커피' '축산가공' '침구/수예' '캐주얼' '커리어' '통신/컴퓨터' '트래디셔널' '피혁잡화' '화장품']
    

+ 마찬가지로, 나머지 범주형 컬럼과 x_test의 데이터도 똑같이 인코딩을 수행합니다.

```python
x_test['주구매상품'] = encoder.fit_transform(x_test['주구매상품'])
print(encoder.classes_)
x_train['주구매지점'] = encoder.fit_transform(x_train['주구매지점'])
print(encoder.classes_)
x_test['주구매지점'] = encoder.fit_transform(x_test['주구매지점'])
print(encoder.classes_)
```

    ['가공식품' '가구' '건강식품' '골프' '구두' '기타' '남성 캐주얼' '남성 트랜디' '남성정장' '농산물' '대형가전'
     '디자이너' '란제리/내의' '명품' '모피/피혁' '보석' '생활잡화' '섬유잡화' '셔츠' '수산품' '스포츠' '시티웨어'
     '식기' '아동' '악기' '액세서리' '육류' '일용잡화' '젓갈/반찬' '주류' '주방가전' '주방용품' '차/커피'
     '축산가공' '침구/수예' '캐주얼' '커리어' '통신/컴퓨터' '트래디셔널' '피혁잡화' '화장품']
    ['강남점' '관악점' '광주점' '노원점' '대구점' '대전점' '동래점' '미아점' '본  점' '부산본점' '부평점' '분당점'
     '상인점' '센텀시티점' '안양점' '영등포점' '울산점' '인천점' '일산점' '잠실점' '전주점' '창원점' '청량리점'
     '포항점']
    ['강남점' '관악점' '광주점' '노원점' '대구점' '대전점' '동래점' '미아점' '본  점' '부산본점' '부평점' '분당점'
     '상인점' '센텀시티점' '안양점' '영등포점' '울산점' '인천점' '일산점' '잠실점' '전주점' '창원점' '청량리점'
     '포항점']
    

<br/>**2-4 파생변수 생성**<br/>

+ insight: 환불내역의 유무에 따라 결과가 달라질 수도 있습니다.
+ 환불금액 유무 컬럼을 추가해 환불 내역이 있다면 1, 없다면 2를 저장합니다.

```python
cond=x_train['환불금액']>0
x_train.loc[cond,'환불금액 유무']=1
x_train.loc[~cond,'환불금액 유무']=0
#~cond: 조건에 맞지 않는 데이터를 반환하고 싶은 경우 사용합니다.
```

+ 마찬가지로, x_test에도 동일한 과정을 수행합니다.

```python
cond=x_test['환불금액']>0
x_test.loc[cond,'환불금액 유무']=1
x_test.loc[~cond,'환불금액 유무']=0
```

<br/>**2-5 데이터 스케일링**<br/>

+ 스케일링이 필요한지를 알아보기 위해 기초통계량을 조회합니다.
+ 만약, 데이터 분포가 고르다면, 굳이 스케일링을 수행할 필요는 없습니다.

```python
x_train.describe().T
#.T: 행과 열을 뒤바꿔 데이터를 보기 편하게 만들어줍니다.
```

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>총구매액</th>
      <td>3500.0</td>
      <td>9.191925e+07</td>
      <td>1.635065e+08</td>
      <td>-52421520.0</td>
      <td>4.747050e+06</td>
      <td>2.822270e+07</td>
      <td>1.065079e+08</td>
      <td>2.323180e+09</td>
    </tr>
    <tr>
      <th>최대구매액</th>
      <td>3500.0</td>
      <td>1.966424e+07</td>
      <td>3.199235e+07</td>
      <td>-2992000.0</td>
      <td>2.875000e+06</td>
      <td>9.837000e+06</td>
      <td>2.296250e+07</td>
      <td>7.066290e+08</td>
    </tr>
    <tr>
      <th>환불금액</th>
      <td>3500.0</td>
      <td>8.289786e+06</td>
      <td>3.010204e+07</td>
      <td>0.0</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>2.642250e+06</td>
      <td>5.637530e+08</td>
    </tr>
    <tr>
      <th>주구매상품</th>
      <td>3500.0</td>
      <td>1.461200e+01</td>
      <td>1.301995e+01</td>
      <td>0.0</td>
      <td>5.000000e+00</td>
      <td>9.000000e+00</td>
      <td>2.200000e+01</td>
      <td>4.100000e+01</td>
    </tr>
    <tr>
      <th>주구매지점</th>
      <td>3500.0</td>
      <td>1.073429e+01</td>
      <td>5.636480e+00</td>
      <td>0.0</td>
      <td>8.000000e+00</td>
      <td>9.000000e+00</td>
      <td>1.500000e+01</td>
      <td>2.300000e+01</td>
    </tr>
    <tr>
      <th>내점일수</th>
      <td>3500.0</td>
      <td>1.925371e+01</td>
      <td>2.717494e+01</td>
      <td>1.0</td>
      <td>2.000000e+00</td>
      <td>8.000000e+00</td>
      <td>2.500000e+01</td>
      <td>2.850000e+02</td>
    </tr>
    <tr>
      <th>내점당구매건수</th>
      <td>3500.0</td>
      <td>2.834963e+00</td>
      <td>1.912368e+00</td>
      <td>1.0</td>
      <td>1.666667e+00</td>
      <td>2.333333e+00</td>
      <td>3.375000e+00</td>
      <td>2.208333e+01</td>
    </tr>
    <tr>
      <th>주말방문비율</th>
      <td>3500.0</td>
      <td>3.072463e-01</td>
      <td>2.897516e-01</td>
      <td>0.0</td>
      <td>2.729090e-02</td>
      <td>2.564103e-01</td>
      <td>4.489796e-01</td>
      <td>1.000000e+00</td>
    </tr>
    <tr>
      <th>구매주기</th>
      <td>3500.0</td>
      <td>2.095829e+01</td>
      <td>2.474868e+01</td>
      <td>0.0</td>
      <td>4.000000e+00</td>
      <td>1.300000e+01</td>
      <td>2.800000e+01</td>
      <td>1.660000e+02</td>
    </tr>
    <tr>
      <th>환불금액 유무</th>
      <td>3500.0</td>
      <td>3.442857e-01</td>
      <td>4.752027e-01</td>
      <td>0.0</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
    </tr>
  </tbody>
</table>
</div>

+ 평균이 e+07정도 차이가나므로 스케일링 과정을 수행합니다.


```python
from sklearn.preprocessing import StandardScaler
#sklearn의 preprocessing 모듈에서 StandardScaler를 불러옵니다.
```


```python
scaler=StandardScaler()
```


```python
x_train=pd.DataFrame(scaler.fit_transform(x_train), 
                    columns=x_train.columns)
x_test=pd.DataFrame(scaler.fit_transform(x_test), 
                    columns=x_test.columns)
```
+ 스케일링을 수행하면 데아터가 array(배열)로 저장되므로 pd.DataFrame 명령을 통해 데이터프레임으로 변환해주고, columns 옵션을 통해 컬럼 명을 붙여줍니다.

```python
x_train.describe().T
```



</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>총구매액</th>
      <td>3500.0</td>
      <td>-4.409171e-17</td>
      <td>1.000143</td>
      <td>-0.882909</td>
      <td>-0.533218</td>
      <td>-0.389621</td>
      <td>0.089237</td>
      <td>13.648260</td>
    </tr>
    <tr>
      <th>최대구매액</th>
      <td>3500.0</td>
      <td>-4.838986e-17</td>
      <td>1.000143</td>
      <td>-0.708278</td>
      <td>-0.524864</td>
      <td>-0.307219</td>
      <td>0.103110</td>
      <td>21.475852</td>
    </tr>
    <tr>
      <th>환불금액</th>
      <td>3500.0</td>
      <td>-7.261255e-16</td>
      <td>1.000143</td>
      <td>-0.275429</td>
      <td>-0.275429</td>
      <td>-0.275429</td>
      <td>-0.187640</td>
      <td>18.455314</td>
    </tr>
    <tr>
      <th>주구매상품</th>
      <td>3500.0</td>
      <td>-6.236281e-17</td>
      <td>1.000143</td>
      <td>-1.122438</td>
      <td>-0.738357</td>
      <td>-0.431093</td>
      <td>0.567518</td>
      <td>2.027026</td>
    </tr>
    <tr>
      <th>주구매지점</th>
      <td>3500.0</td>
      <td>-8.333017e-17</td>
      <td>1.000143</td>
      <td>-1.904703</td>
      <td>-0.485175</td>
      <td>-0.307733</td>
      <td>0.756913</td>
      <td>2.176441</td>
    </tr>
    <tr>
      <th>내점일수</th>
      <td>3500.0</td>
      <td>2.518303e-16</td>
      <td>1.000143</td>
      <td>-0.671807</td>
      <td>-0.635003</td>
      <td>-0.414180</td>
      <td>0.211486</td>
      <td>9.780490</td>
    </tr>
    <tr>
      <th>내점당구매건수</th>
      <td>3500.0</td>
      <td>-2.288170e-16</td>
      <td>1.000143</td>
      <td>-0.959661</td>
      <td>-0.611003</td>
      <td>-0.262346</td>
      <td>0.282432</td>
      <td>10.066639</td>
    </tr>
    <tr>
      <th>주말방문비율</th>
      <td>3500.0</td>
      <td>9.192647e-17</td>
      <td>1.000143</td>
      <td>-1.060530</td>
      <td>-0.966329</td>
      <td>-0.175472</td>
      <td>0.489224</td>
      <td>2.391196</td>
    </tr>
    <tr>
      <th>구매주기</th>
      <td>3500.0</td>
      <td>-1.220294e-16</td>
      <td>1.000143</td>
      <td>-0.846966</td>
      <td>-0.685318</td>
      <td>-0.321610</td>
      <td>0.284570</td>
      <td>5.861421</td>
    </tr>
    <tr>
      <th>환불금액 유무</th>
      <td>3500.0</td>
      <td>3.445498e-16</td>
      <td>1.000143</td>
      <td>-0.724606</td>
      <td>-0.724606</td>
      <td>-0.724606</td>
      <td>1.380060</td>
      <td>1.380060</td>
    </tr>
  </tbody>
</table>
</div>

+ 평균이 0, std가 1에 근사한 값으로 변환되었습니다.

<br/>**2-6 다중공산성 확인**<br/>

+ 다중공산성: 독립변수들 간에 강한 상관관계가 있다면 예측 결과에 부정적 영향을 미치게 됩니다.
+ 금액과 관련된 변수들의 상관관계를 조회한 후 상관계수가 큰 변수는 제거해주겠습니다.


```python
x_train[['총구매액','최대구매액','환불금액 유무']].corr()
```


</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>총구매액</th>
      <th>최대구매액</th>
      <th>환불금액 유무</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>총구매액</th>
      <td>1.000000</td>
      <td>0.700080</td>
      <td>0.403357</td>
    </tr>
    <tr>
      <th>최대구매액</th>
      <td>0.700080</td>
      <td>1.000000</td>
      <td>0.330687</td>
    </tr>
    <tr>
      <th>환불금액 유무</th>
      <td>0.403357</td>
      <td>0.330687</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>

+ 총구매액과 최대구매액의 상관계수가 0.700080으로 큰 것을 알 수 있습니다.


```python
del x_train['최대구매액']
del x_test['최대구매액']
#del 키워드를 통해 최대구매액 컬럼을 제거합니다.
```


```python
x_train.head(3)  #작업내역 확인
```


</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>총구매액</th>
      <th>환불금액</th>
      <th>주구매상품</th>
      <th>주구매지점</th>
      <th>내점일수</th>
      <th>내점당구매건수</th>
      <th>주말방문비율</th>
      <th>구매주기</th>
      <th>환불금액 유무</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.14458</td>
      <td>-0.047505</td>
      <td>-0.738357</td>
      <td>-1.904703</td>
      <td>-0.009338</td>
      <td>0.554247</td>
      <td>0.758623</td>
      <td>-0.159962</td>
      <td>1.380060</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.54919</td>
      <td>-0.265461</td>
      <td>0.490702</td>
      <td>1.466677</td>
      <td>-0.635003</td>
      <td>-0.698168</td>
      <td>-1.060530</td>
      <td>-0.806554</td>
      <td>1.380060</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.54270</td>
      <td>-0.275429</td>
      <td>-0.661541</td>
      <td>-1.727262</td>
      <td>-0.635003</td>
      <td>-0.436675</td>
      <td>-1.060530</td>
      <td>-0.806554</td>
      <td>-0.724606</td>
    </tr>
  </tbody>
</table>
</div>

<br/>#3. 모델 학습 및 평가<br/>

+ 성별 예측 모델은 성별을 분류할 수 있는 모델을 만들어야 하므로, 의사결정나무 분류기를 사용하겠습니다.

<br/>#3-1 모델 학습시키기<br/>

```python
from sklearn.tree import DecisionTreeClassifier
model = DecisionTreeClassifier(
         max_depth=10, criterion='entropy')
 # model 객체에 하이퍼 파라미터를 튜닝합니다.
 # max+depth = 의사결정나무의 최대 깊이를 조정합니다. <br/> 깊이가 너무 깊어지면 과적합될 수 있으니 적당히 10으로 조정합니다.
model.fit(x_train, y_train)       #model에 데이터 학습시키기
```

<br/>#3-2 테스트<br/>

+ 훈련용 데이터로 학습시킨 모델에 검증용 데이터인 x_test를 넣의 y의 예측값(y_test_pred)을 반환합니다.

```python
y_test_pred = model.predict(x_test) 
#훈련용 데이터로 학습시킨 모델에 x_test 데이터를 넣어 y의 예측값을 반환합니다.
print(pd.DataFrame(y_test_pred).head(3))   #예측된 데이터를 조회합니다.
```

       0
    0  1
    1  0
    2  0

+ 예측된 값이 정상적으로 출력됨을 알 수 있습니다.

<br/>#3-3 예측 수행하기<br/>

+ 문제에선 성별 분류가 아닌 남자일 확률을 요구하고 있습니다.
+ predict_proba(): 예측값의 확률을 반환합니다.

```python
y_test_proba=model.predict_proba(x_test)
print(pd.DataFrame(y_test_proba).head(3))
```

              0         1
    0  0.000000  1.000000
    1  0.921569  0.078431
    2  1.000000  0.000000
    
+ 0 컬럼은 여자일 확률, 1 컬럼을 남자일 확률을 뜻합니다.
+ 0 컬럼은 필요없으니 삭제해줍니다.

```python
result=pd.DataFrame(y_test_proba)   #result 변수에 예측값을 저장합니다.
del result[0]                       #0 컬럼 삭제
result.head()                       #데이터 확인
```



</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.078431</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>

<br/>**3-4 모델 평가하기**<br/>

+ y_test 값(검증용 데이터)이 존재하지 않으니<br/>model에 학습용 데이터를 넣어 예측값 y_train_pred를 생성한 후<br/>실제 y_train 데이터로 평가를 수행합니다.

+ 먼저, model을 통해 예측값 y_train_pred을 생성합니다. 
```python
y_train_pred=model.predict(x_train)
```

+ 모델 평가를 위해 roc_auc_score을 불러옵니다.
+ roc_auc_score: 1에 가까울 수록 좋은 모델을 뜻합니다.

```python
from sklearn.metrics import roc_auc_score
# sklearn의 metrics 모듈의 roc_auc_score를 불러옵니다.
```


```python
print(roc_auc_score(y_train,y_train_pred))
#나름 괜찮은 모델이 생성되었음을 알 수 있습니다.
```

    0.7116504948951758
    

<br/>#4 결과 제출하기<br/>

```python
#최종 데이터는 cust_id 값과 y_test의 예측값이 함계 출력되어야 합니다.
pd.concat([x_test_cust_id, result],axis=1)  #수평결합
#concat 함수의 기본값은 axis=0 으로 수직결합을 수행합니다.
```



</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cust_id</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3500</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3501</td>
      <td>0.078431</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3502</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3503</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3504</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2477</th>
      <td>5977</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>2478</th>
      <td>5978</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2479</th>
      <td>5979</td>
      <td>0.400000</td>
    </tr>
    <tr>
      <th>2480</th>
      <td>5980</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2481</th>
      <td>5981</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
<p>2482 rows × 2 columns</p>
</div>




```python
final=pd.concat([x_test_cust_id, result],axis=1).rename(columns={1:'gender'})
#rename 함수: 문제와 동일하게 1 컬럼을 gender 컬럼으로 변경해줍니다.
#final.to_csv('data/12345.csv',index=Falese)
#최종 파일로 저장시 꼭 인덱스 열은 index=False 옵션으로 제거합니다.
```
