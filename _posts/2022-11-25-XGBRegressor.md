<br/>**Data**<br/>
+ bike_y_train.csv: 시간당 자전거 대여량 데이터
+ bike_x_train.csv, bike_x_test.csv: 고객의 자전거 대여 속성

<br/>**Question**<br/>
+ 자전거 대여량 예측 모형 생성하기

<br/># 1. 데이터 탐색<br/>
<br/>**1-1 라이브러리 및 데이터 불러오기**<br/>


```python
import pandas as pd
x_train=pd.read_csv('...data2/bike_x_train.csv', encoding='CP949')
x_test=pd.read_csv('...data2/bike_x_test.csv', encoding='CP949')
y_train=pd.read_csv('...data2/bike_y_train.csv', encoding='CP949')
#인코딩 오류로  encoding='CP949' 옵션을 추가했습니다.
#실제 시험환경에선 사용할 일이 없다고 합니다.
```

<br/>**1-2 EDA 수행**<br/>


```python
x_train.info()
#결측치가 없음을 알 수 있습니다.
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 10886 entries, 0 to 10885
    Data columns (total 9 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   datetime  10886 non-null  object 
     1   계절        10886 non-null  int64  
     2   공휴일       10886 non-null  int64  
     3   근무일       10886 non-null  int64  
     4   날씨        10886 non-null  int64  
     5   온도        10886 non-null  float64
     6   체감온도      10886 non-null  float64
     7   습도        10886 non-null  int64  
     8   풍속        10886 non-null  float64
    dtypes: float64(3), int64(5), object(1)
    memory usage: 765.5+ KB
    


```python
print(x_train.head(3))
# 계절, 공휴일, 근무일, 날씨가 범주형 데이터로 이루어져 있습니다.
print(y_train.head(3))
```

              datetime  계절  공휴일  근무일  날씨    온도    체감온도  습도   풍속
    0  2011-01-01 0:00   1    0    0   1  9.84  14.395  81  0.0
    1  2011-01-01 1:00   1    0    0   1  9.02  13.635  80  0.0
    2  2011-01-01 2:00   1    0    0   1  9.02  13.635  80  0.0
             癤풼atetime  count
    0  2011-01-01 0:00     16
    1  2011-01-01 1:00     40
    2  2011-01-01 2:00     32
    


```python
x_train.describe().T
#데이터의 분포가 일의자리에서 십의자리로 고르게 분포되어 있습니다.
#스케일링은 따로 수행하지 않습니다.
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
      <th>계절</th>
      <td>10886.0</td>
      <td>2.506614</td>
      <td>1.116174</td>
      <td>1.00</td>
      <td>2.0000</td>
      <td>3.000</td>
      <td>4.0000</td>
      <td>4.0000</td>
    </tr>
    <tr>
      <th>공휴일</th>
      <td>10886.0</td>
      <td>0.028569</td>
      <td>0.166599</td>
      <td>0.00</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.0000</td>
      <td>1.0000</td>
    </tr>
    <tr>
      <th>근무일</th>
      <td>10886.0</td>
      <td>0.680875</td>
      <td>0.466159</td>
      <td>0.00</td>
      <td>0.0000</td>
      <td>1.000</td>
      <td>1.0000</td>
      <td>1.0000</td>
    </tr>
    <tr>
      <th>날씨</th>
      <td>10886.0</td>
      <td>1.418427</td>
      <td>0.633839</td>
      <td>1.00</td>
      <td>1.0000</td>
      <td>1.000</td>
      <td>2.0000</td>
      <td>4.0000</td>
    </tr>
    <tr>
      <th>온도</th>
      <td>10886.0</td>
      <td>20.230860</td>
      <td>7.791590</td>
      <td>0.82</td>
      <td>13.9400</td>
      <td>20.500</td>
      <td>26.2400</td>
      <td>41.0000</td>
    </tr>
    <tr>
      <th>체감온도</th>
      <td>10886.0</td>
      <td>23.655084</td>
      <td>8.474601</td>
      <td>0.76</td>
      <td>16.6650</td>
      <td>24.240</td>
      <td>31.0600</td>
      <td>45.4550</td>
    </tr>
    <tr>
      <th>습도</th>
      <td>10886.0</td>
      <td>61.886460</td>
      <td>19.245033</td>
      <td>0.00</td>
      <td>47.0000</td>
      <td>62.000</td>
      <td>77.0000</td>
      <td>100.0000</td>
    </tr>
    <tr>
      <th>풍속</th>
      <td>10886.0</td>
      <td>12.799395</td>
      <td>8.164537</td>
      <td>0.00</td>
      <td>7.0015</td>
      <td>12.998</td>
      <td>16.9979</td>
      <td>56.9969</td>
    </tr>
  </tbody>
</table>
</div>



<br/>**1-3 독립변수와 종속변수**<br/>
+ 회귀 분석에서 중요한 것은 독립변수와 종속변수간의 관계를 먼저 파악하는 것입니다.
+ 먼저, 범주형 변수들과 종속변수간의 관계를 파악해 보겠습니다.


```python
data=pd.concat([x_train,y_train],axis=1)
#먼저 x 데이터(독립변수)와 y 데이터(종속변수)를 결합해줍니다.
#axis=1: 데이터를 수평으로 결합합니다.
```


```python
#'계절' 컬럼과 종속변수와의 관계
data.groupby(['계절'])['count'].sum()
#groupby 함수를 통해 계절별 count 컬럼의 그룹합계를 출력합니다.
```




    계절
    1    312498
    2    588282
    3    640662
    4    544034
    Name: count, dtype: int64



+ 1(봄)과 3(가을)의 데이터가 약 2배 차이남을 알 수 있습니다.


```python
#'공휴일' 컬럼과 종속변수의 관계
data.groupby(['공휴일'])['count'].sum()
```




    공휴일
    0    2027668
    1      57808
    Name: count, dtype: int64



+ 0(공휴일 아님)이 1(공휴일)보다 약 40배 더 큰 것을 알 수 있습니다.


```python
#'근무일' 컬럼과 종쇽변수의 관계
```


```python
data.groupby(['근무일'])['count'].sum()
```




    근무일
    0     654872
    1    1430604
    Name: count, dtype: int64



+ 1(근무일)이 0(근무일 아님)보다 2배이상 큰 것을 알 수 있습니다.


```python
#'날씨' 컬럼과 종속변수의 관계
```


```python
data.groupby(['날씨'])['count'].sum()
#1(아주 깨끗), 2(안개,구름), 3(조금의 눈,비,천둥), 4(아주 많은 비,우박)
```




    날씨
    1    1476063
    2     507160
    3     102089
    4        164
    Name: count, dtype: int64



+ 날씨가 좋을 수록 값이 커짐을 알 수 있습니다.
+ **각 독립변수들이 종속변수에 영향을 주고 있습니다.**

<br/># 2. 데이터 전처리<br/>

<br/>**2-1 파생변수, 요일 데이터 생성**<br/>


```python
#먼저 datetime의 데이터타입을 변환해줍니다.
x_train['datetime']=pd.to_datetime(x_train['datetime'])
```


```python
#년, 월, 일, 시간, 요일 변수 생성
x_train['year']=x_train['datetime'].dt.year
x_train['month']=x_train['datetime'].dt.month
x_train['day']=x_train['datetime'].dt.day
x_train['day_of_week']=x_train['datetime'].dt.day_of_week
x_train['hour']=x_train['datetime'].dt.hour
x_train.head(3)
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
      <th>datetime</th>
      <th>계절</th>
      <th>공휴일</th>
      <th>근무일</th>
      <th>날씨</th>
      <th>온도</th>
      <th>체감온도</th>
      <th>습도</th>
      <th>풍속</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>day_of_week</th>
      <th>hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2011-01-01 00:00:00</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>9.84</td>
      <td>14.395</td>
      <td>81</td>
      <td>0.0</td>
      <td>2011</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2011-01-01 01:00:00</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>9.02</td>
      <td>13.635</td>
      <td>80</td>
      <td>0.0</td>
      <td>2011</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2011-01-01 02:00:00</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>9.02</td>
      <td>13.635</td>
      <td>80</td>
      <td>0.0</td>
      <td>2011</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
#각 변수들과 종속변수간의 관계를 살펴 봅니다.
data2=pd.concat([x_train,y_train],axis=1)
```


```python
print(data2.groupby(['year'])['count'].sum())
print(data2.groupby(['month'])['count'].sum())
print(data2.groupby(['day'])['count'].sum())
print(data2.groupby(['day_of_week'])['count'].sum())
print(data2.groupby(['hour'])['count'].sum())
```

    year
    2011     781979
    2012    1303497
    Name: count, dtype: int64
    month
    1      79884
    2      99113
    3     133501
    4     167402
    5     200147
    6     220733
    7     214617
    8     213516
    9     212529
    10    207434
    11    176440
    12    160160
    Name: count, dtype: int64
    day
    1     103692
    2     105381
    3     111561
    4     112335
    5     109115
    6     108600
    7     105486
    8     102770
    9     108041
    10    111645
    11    111146
    12    109257
    13    111448
    14    112406
    15    115677
    16    109837
    17    118255
    18    108437
    19    110387
    Name: count, dtype: int64
    day_of_week
    0    295296
    1    291985
    2    292226
    3    306401
    4    302504
    5    311518
    6    285546
    Name: count, dtype: int64
    hour
    0      25088
    1      15372
    2      10259
    3       5091
    4       2832
    5       8935
    6      34698
    7      96968
    8     165060
    9     100910
    10     79667
    11     95857
    12    116968
    13    117551
    14    111010
    15    115960
    16    144266
    17    213757
    18    196472
    19    143767
    20    104204
    21     79057
    22     60911
    23     40816
    Name: count, dtype: int64
    

+ day 변수와 day_of_week 변수는 유의미한 통계량 차이가 없으니 삭제해주도록 하겠습니다.


```python
del x_train['day']
del x_train['day_of_week']
x_train.head(3)
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
      <th>datetime</th>
      <th>계절</th>
      <th>공휴일</th>
      <th>근무일</th>
      <th>날씨</th>
      <th>온도</th>
      <th>체감온도</th>
      <th>습도</th>
      <th>풍속</th>
      <th>year</th>
      <th>month</th>
      <th>hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2011-01-01 00:00:00</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>9.84</td>
      <td>14.395</td>
      <td>81</td>
      <td>0.0</td>
      <td>2011</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2011-01-01 01:00:00</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>9.02</td>
      <td>13.635</td>
      <td>80</td>
      <td>0.0</td>
      <td>2011</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2011-01-01 02:00:00</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>9.02</td>
      <td>13.635</td>
      <td>80</td>
      <td>0.0</td>
      <td>2011</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
#x_test에도 마찬가지로 year, month, hour 변수를 추가해 주겠습니다.
```


```python
x_test['datetime']=pd.to_datetime(x_test['datetime'])
x_test['year']=x_test['datetime'].dt.year
x_test['month']=x_test['datetime'].dt.month
x_test['hour']=x_test['datetime'].dt.hour
```

+ 추가로 datetime 컬럼은 최종제출시에만 필요한 변수가 되었으니 삭제해줍니다.


```python
x_test_datetime=x_test['datetime']
x_train=x_train.drop(columns=['datetime'])
x_test=x_test.drop(columns=['datetime'])
y_train=y_train.drop(columns=['癤풼atetime'])
#인코딩 오류로 y_train의 컬럼 이름만 이상하게 표시되는데 무시하겠습니다.
```

<br/># 3. 모델 학습 및 평가<br/>

<br/>**3-1 데이터분리**<br/>

+ 먼저, 모델을 2개 생성한 후 더 성능이 좋은 모델을 사용하도록 하겠습니다.
+ 성능을 테스트하기 위해선 검증용 Y_TEST 데이터가 필요하니, 기존 훈련용 데이터를 혼련용과 검증용 데이터로 분리하여 사용하겠습니다.


```python
from sklearn.model_selection import train_test_split
#sklearn 패키지의 moder_selection 모듈에서 train_test_split 함수를 불러옵니다.
```


```python
X_TRAIN, X_TEST, Y_TRAIN, Y_TEST\
 =train_test_split(x_train, y_train, test_size=0.2)
```

+ train_test_split 사용시 주의점은 꼭 순서가 x_train, x_test, y_train, y_test로 지켜져야 한다는 것입니다.
+ test_size=0.2: 훈련용 데이터 80%, 검증용 데이터 20%의 비율로 데이터를 분리합니다.
+ 추가로, 원래 데이터와의 구분을 위해 대문자를 사용했습니다.


```python
print(X_TRAIN.shape, X_TEST.shape, Y_TRAIN.shape, Y_TEST.shape)
#8대 2의 비율로 적절하게 분리되었습니다.
```

    (8708, 11) (2178, 11) (8708, 1) (2178, 1)
    

**3-2 데이터 학습(XGB 회귀 활용)**

+ XGB회귀: 다수의 약한 분류기를 묶어 정확도를 높입니다.


```python
pip install xgboost
#xgboost 모듈을 먼저 설치해줍니다.
```

    


```python
from xgboost import XGBRegressor
#xgboost 모듈에서 XGBRegressor를 호출합니다.
```

+ model1, model2를 생성한 후 r2 score로 성능 비교를 해보겠습니다.


```python
model1= XGBRegressor(n_estimators=100, max_depth=3)
model1.fit(X_TRAIN,Y_TRAIN)
model2= XGBRegressor(n_estimators=200, max_depth=5)
model2.fit(X_TRAIN,Y_TRAIN)
# n_estimators: 생성할 약한 분류기의 수
# max_depth: 분류기의 깊이, 너무 깊으면 과적합의 가능성이 커집니다.
```




    XGBRegressor(base_score=0.5, booster='gbtree', callbacks=None,
                 colsample_bylevel=1, colsample_bynode=1, colsample_bytree=1,
                 early_stopping_rounds=None, enable_categorical=False,
                 eval_metric=None, feature_types=None, gamma=0, gpu_id=-1,
                 grow_policy='depthwise', importance_type=None,
                 interaction_constraints='', learning_rate=0.300000012, max_bin=256,
                 max_cat_threshold=64, max_cat_to_onehot=4, max_delta_step=0,
                 max_depth=5, max_leaves=0, min_child_weight=1, missing=nan,
                 monotone_constraints='()', n_estimators=200, n_jobs=0,
                 num_parallel_tree=1, predictor='auto', random_state=0, ...)



<br/>**3-3 결과 예측**<br/>

```python
# 더 성능이 좋은 모델을 찾기 위해 r2 score를 계산합니다.
# 먼저, 각각의 모델들로 Y예측값을 생성합니다.
Y_TEST_PRED1=pd.DataFrame(model1.predict(X_TEST))
Y_TEST_PRED2=pd.DataFrame(model2.predict(X_TEST))
```


```python
from sklearn.metrics import r2_score
#sklearn의 평가 모듈인 metrics 모듈을 불러와 r2_score를 호출합니다.
```


```python
r2_1=r2_score(Y_TEST,Y_TEST_PRED1)
r2_2=r2_score(Y_TEST,Y_TEST_PRED2)
r2_1, r2_2
```




    (0.9091770899818591, 0.9423470932966658)



+ r2_score는 1에 가까울수록 좋은 모델임을 뜻하니 2번 모델을 사용하도록 하겠습니다.


```python
#실제 제출을 위한 y_pred 데이터를 생성합니다.
#여기선 임의로 생성한 X_TEST 데이터가 아닌, 주어진 x_test(소문자) 데이터를 사용해야 합니다.
y_test_pred=pd.DataFrame(model2.predict(x_test))
y_test_pred.head(3)
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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.975508</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-2.576652</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-9.153737</td>
    </tr>
  </tbody>
</table>
</div>



+ 예측값에 음수가 존재함을 알 수 있습니다.
+ 자전거 대여량엔 음수가 존재할 수 없으니 모두 0으로 바꾸겠습니다.
+ 추가로 컬럼명을 0에서 count로 변경하겠습니다.


```python
cond=y_test_pred[0]<0
y_test_pred.loc[cond,0]=0
y_test_pred.head(3)  # 음수값이 0으로 변경되었음을 확인합니다.
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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.975508</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
y_test_pred=y_test_pred.rename(columns={0 :'count'})
 #rename 함수를 통해 컬럼명을 변경합니다.
 #rename 함수는 단순 조회함수기 때문에 꼭 저장해주어야 합니다.
y_test_pred.head(3)
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
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.975508</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>



<br/># 4. 결과 제출<br/>

+ 따로 저장해뒀던 datetime 컬럼과 예측값을 수평결합한 후
+ to_csv()함수를 통해 csv 파일로 저장해줍니다.
+ 파일 저장시 index=False 옵션으로 인덱스를 제거해줍니다.


```python
final=pd.concat([x_test_datetime,y_test_pred],axis=1)
print(final)
#final.to_csv('data/12345.csv', index=False)
```

                    datetime       count
    0    2011-01-20 00:00:00    5.975508
    1    2011-01-20 01:00:00    0.000000
    2    2011-01-20 02:00:00    0.000000
    3    2011-01-20 03:00:00    0.000000
    4    2011-01-20 04:00:00    0.000000
    ...                  ...         ...
    6488 2012-12-31 19:00:00  277.086426
    6489 2012-12-31 20:00:00  188.852402
    6490 2012-12-31 21:00:00  143.609970
    6491 2012-12-31 22:00:00  100.622528
    6492 2012-12-31 23:00:00   59.089928
    
    [6493 rows x 2 columns]
    
