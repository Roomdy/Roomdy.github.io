###### <span style="font-size:20px; font-weight:bold"> 숫자형


```python
print ('',123,'\n',1.2e3,'\n',35.08)
print ('',type(123),'\n',type(1.2e3),'\n',type(35.08))
```

     123 
     1200.0 
     35.08
     <class 'int'> 
     <class 'float'> 
     <class 'float'>
    

###### <span style="font-size:20px; font-weight:bold">문자형


```python
print ('',type('Hello'),'\n',type('안녕, 훈쌤이라고 합니다'))
print ('','Hello','\n','안녕, 훈쌤이라고 합니다') 
```

     <class 'str'> 
     <class 'str'>
     Hello 
     안녕, 훈쌤이라고 합니다
    


```python
print('''이거 저거 요모 조모
이러쿵 저러쿵
요렇게 저렇게''')
```

    이거 저거 요모 조모
    이러쿵 저러쿵
    요렇게 저렇게
    


```python
"안녕하세요, '훈쌤'이라고 합니다.^^"
```




    "안녕하세요, '훈쌤'이라고 합니다.^^"




```python
print("12345")
print(type("12345"))
```

    12345
    <class 'str'>
    

###### <span style="font-size:20px; font-weight:bold">논리형


```python
print(type(True))
print(type("True"))
```

    <class 'bool'>
    <class 'str'>
    


```python
print(True)
print(False)
```

    True
    False
    


```python
print(1==1)
print(type(1==1))
print(float(1==1))
print(int(1>3))
```

    True
    <class 'bool'>
    1.0
    0
    


```python
str(3+5)
```




    '8'




```python
str(1==1)
```




    'True'



### 연산자
<br>

- **산술연산자**
    - \+ : 덧셈 (문자열 가능)
    - \- : 뺄셈
    - \* : 곱셈 (문자열과 정수의 곱셈 가능)
    - / : 나눗셈
    - // : 나눗셈의 몫
    - % : 나눗셈의 나머지 
    - ** : 거듭 제곱
<br><br>
- **논리연산자** (숫자형만 가능
    - < : 작다
    - \> : 크다
    - <= : 작거나 같다
    - \>= : 크거나 같다
    - == : 같다
    - != : 다르다
<br><br>
- **연산자 우선순위** (내림 차순)
    - ( 　 )　　<span style="font-size:15px">　　_--> **우선 순위 높음**_</span>
    - **
    - \* / // %
    - \+ -
    - < <= > >= == !=
    - not
    - and
    - or　　　　<span style="font-size:15px">　_--> **우선 순위 낮음**_</span>

###### <span style="font-size:20px; font-weight:bold">덧셈


```python
print(2+3)
print(12.23+67.1)
print(3.1415+7+True)
print("Hi"+"에이치아이"+"커피한잔할래용~")
```

    5
    79.33
    11.1415
    Hi에이치아이커피한잔할래용~
    

###### <span style="font-size:20px; font-weight:bold">뺄셈


```python
print(3-2)
print(12.34-10)
print(True-100)
```

    1
    2.34
    -99
    

###### <span style="font-size:20px; font-weight:bold">곱셈


```python
print(12.3*4)
print("어? 이쁘다. "*3)
print(3**4)
print(9**0.5)
```

    49.2
    어? 이쁘다. 어? 이쁘다. 어? 이쁘다. 
    81
    3.0
    

###### <span style="font-size:20px; font-weight:bold">나눗셈


```python
print(4/2)
print(13/3)
print(13.0/3)
print(-3.14150/1.25)
```

    2.0
    4.333333333333333
    4.333333333333333
    -2.5132000000000003
    

###### <span style="font-size:20px; font-weight:bold">몫과 나머지


```python
print(4//2)
print(13.0//3)
print(13//3)

print(4%2)
print(13%3)
print(13.5%3)
```

###### <span style="font-size:20px; font-weight:bold">대소비교


```python
print(2<3)
print(3.1415>100)
print("ABC"<="abc")
print("Kim">="lee")
```

    True
    False
    True
    False
    

###### <span style="font-size:20px; font-weight:bold">동일여부 비교


```python
print(3==1+2)
print(12=='123')
print(5!=5)
print('PYTHON'!='python')
```

    True
    False
    False
    True
    

###### <span style="font-size:20px; font-weight:bold">비교연산자


```python
print(not False)
print(not 1)
print(not 0)

print(3<4 and 'a'!='b')
print(3.14>5.5 and 'a'<='b')
print(3.14>5.5 or 'a'=='b')
```

    True
    False
    True
    True
    False
    


```python

```
