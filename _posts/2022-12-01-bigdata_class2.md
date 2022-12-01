**1. 라이브러리 모음**

    import pandas as pd
    import numpy as np
    
**2. 전처리**

    df.dropna(subset='기준 컬럼')        #결측치 있는 행 삭제
    df.fillna()                          #결측치 대체
    df.fillna(method='bfill' or 'ffill')
    
    from sklearn.preprocessing import LabelEncoder   #인코딩
    encoder = LabelEncoder()
    encoded_data = encoder.fit_transform()
    
**3-1. 모델 학습(분류)**

    #RandomForestClassifier
    from sklearn.ensemble import RandomForestClassifier
    model= RandomForestClassifier(n_estimators=200, max_depth=7)
                              or (n_estimators=100, max_depth=5)
    model.fit(x_train, y_train)                      #모델 학습시키기
    y_predict=pd.DataFrame(model.predict(x_test))    #예측값 생성
    
    #SVC
    from sklearin.svm import SVC     #SVC만 대문자
    model=SVC()
    model.fit(x_train, y_train)
    y_predict=pd.DataFrame(model.predict(x_test))
    
**3-2. 모델 학습(예측)**

    #XGBRegressor
    pip install xgboost
    from xgboost import XGBRegressor
    model1=XGBRegressor(n_estimators=100, max_depth=3)
                     or(n_estimators=200, max_depth=5)
    model.fit(x_train, y_train)
    y_pred=pd.DataFrame(model.predict(x_test))
    
    
    
**4-1. 모델 평가(분류)**

    #roc_auc_score
    from sklearn.metrics import roc_auc_score
    print(roc_auc_score(y_test, y_predict)       #test가 먼저
    
**4-2 모델 평가(예측)**

    #mean_squared_error
    from sklearn.metrics import mean_squared_error
    print(mean_squared_error(y_test, y_predict) #test가 먼저
    
    #r2_score
    from sklearn.metrics import r2_score
    print(t2_socre(y_test, y_predict)
