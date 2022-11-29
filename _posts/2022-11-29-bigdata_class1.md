---
layout: single
title:  "[빅데이터분석기사] 실기 - 유형1 함수 모음"
---


+ 편의상 dataframe은 df로 표현하겠습니다.

**1. IQR 계산**

    Q1 = np.percentile(df['column'],25)
    Q2 = np.percentile(df['column'],75)
    IQR = Q3 - Q1
    btom_cut_off = Q1 - 1.5 * IQR
    top_cut_pff = Q3 + 1.5 * IQR
    
   
**2. 결측치 제거, 대체**

    #결측치 제거
    df.dropna(subset='기준 컬럼')
    #결측치 대체
    df.fillna(method='bfill')     #바로 다음값으로 대체
    df.fillna(method='ffill')     #바로 직전값으로 대체

    #심화: map 함수 활용
    df['column'].fillna(df['기준 column'].map({A:a, B:b, ...}))
    # map 함수: 사전(dict)에 정의된 내용을 변수에 저장
 
**3. 그룹합계**

    df.groupby(['기준 컬럼1', '기준 컬럼2', ...]).sum()
    
**4. 올림, 내림, 버림**

    np.ceil()       #올림
    np.floor()      #내림
    np.trunc()      #버림
    
**5. 표준편차, 분산**

    series.std()    #표준편차: 분산의 제곱근
    series.var()    #분산
    
**6. 누적합**

    series.cumsum()
    #사용 예시
    df2=df[df['f2]==1]['f1'].cumsum()
    조건에 만족하는 f1 컬럼의 누적합 반환
    
**7. MinMaxScaler**

    from sklearn.perprocessing import MinMaxScaler
    scaler = MinMaxScaler()
    scaler.fit_transform()
    
**8. 상하위값**

    series.quantile(0.95)
    series.quantile(0.05)
    
**9. StandardScaler**

    from sklearn.preprocessing import StandardScaler
    scaler = StandardScaler()
    sclaer.fit_transform()
    
**10. 값 수동변경**

    series.replace(A,B)
    # 값 A를 값 B로 변경
    
**11. 상관계수**

    df.corr()
    
**12. 시계열**

    #데이터타입 변경
    pd.to_datetime()
    #연도,월,일,요일,시간 데이터 반환
    series.dt.year
    series.dt.month
    series.dt.day
    series.dt.day_of_week     #monday=0, sunday=6 반환
    series.dt.hour
    
**13. apply, lambda 세트**

    series.apply(lambda x : 표현식, axis = 0 or 1)
    #apply 함수: 입력값을 넣으면 반복적으로 행 or 열에 수행
    #lambda 함수: 즉시 정의해서 사용하는 익명 함수
    
+ 사용예시: day_of_week와 연계하여 x >= 5인 주말 데이터 생성 가능

**14. 인덱스 초기화**

    df.reset_index(drop=True)
    # drop=True: 기존의 인덱스 제거
    
**15. 데이터 병합**

    pd.merge(df1, df2, how='결합방식', on=['key'])
    # merge: key 기준 데이터 결합
    # 기본 결합방식은 inner join(key 기준 교집합)
    # how 인자: how = 'left'로 left join 수행가능(왼쪽 df key 기준 결합)
    
**16. 중복값 제거**

    df.drop_duplicates(subset=['column'])
    # 대상 컬럼 기준 중복된 데이터 발생시 뒤에 나오는 행 삭제
    
**17. resampling**

    df. resample(rule = '단위', on = 'column').sum()
    # 단위 간격 마다의 값으로 df 반환
    # on 인자: sampling 대상 컬럼 지정
    # rule 인자: '1W' = 1주, '2W' = 2주, '1M' = 한달 단위
    
**18. qcut**

    pd.qcut(df['column'], 그룹 수, labels=['그룹1', '그룹2', ...])
    # 동일한 데이터 개수로 구간 나눔
    # 기준 컬럼 설정시 크기순으로 나눔
    # labes 인자: 그룹명 지정
    
**19. 지연값 생성**

    series.shitt(지연시킬 칸 수)
    
**20. 문자열 인덱싱**

    series.str[:1] == 'E'
    #첫글자가 E 인 데이터 반환
    
