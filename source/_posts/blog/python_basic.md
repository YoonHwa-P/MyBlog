---
layout: temp
title: "Lecture_python_basic"
date: 2021-11-02 14:21:20
tags: python_basic
---


## Hello World


```python
print("Hello, world!")
print("hi ^0^")
```

    Hello, world!
    

## 주석처리


```python

# 한 줄 주석 처리
"""
여러 줄 주석 예제 동일한 따옴표(큰따옴표 혹은 작은 따옴표) 세 개와 세 개 사이에는어떠한 내용, 몇 줄이 들어가더라도 모두 주석으로 처리된다.
"""
print("Hello, world!")

```

    Hello, world!
    

## 변수의 종류



```python
num_int = 1
print(type(num_int))
```

    <class 'int'>
    


```python
num_float = 0.2
print(type(num_float))
```

    <class 'float'>
    


```python
bool_true = True
print(type(bool_true))
```

    <class 'bool'>
    


```python
none_x = None
print(type(none_x))
```

    <class 'NoneType'>
    

## 사칙 연산





```python
a = 3
b = 2
print('a + b = ', a+b)
print('a - b = ', a-b)
print('a * b = ', a*b)
print('a / b = ', a/b)
print('a // b = ', a//b)
print('a % b = ', a%b)
print('a ** b = ', a**b)
```

    a + b =  5
    a - b =  1
    a * b =  6
    a / b =  1.5
    a // b =  1
    a % b =  1
    a ** b =  9
    


```python
c = 3.0
d = 2.0
print('c + d =', c+d)
print('c - d =', c-d)
print('c * d =', c*d)
print('c / d =', c/d)
print('c // d =', c//d)
print('c % d =', c%d)
print('c ** d =', c**d)
```

    c + d = 5.0
    c - d = 1.0
    c * d = 6.0
    c / d = 1.5
    c // d = 1.0
    c % d = 1.0
    c ** d = 9.0
    

## 논리형 연산자


```python
print(True and True)
print(True and False)
print(False and True)
print(False and False)
```

    True
    False
    False
    False
    


```python
print(True or True)
print(True or False)
print(False or True)
print(False or False)
```

    True
    True
    True
    False
    

## 비교 연산자


```python
print(4 > 3)
print(4 < 3)
print(4 >= 3)
print(4 <= 3)
print(4 > 4)
print(4 >= 4)
```

    True
    False
    True
    False
    False
    True
    

## 논리형 & 비교 연산자 응용


```python
#input("숫자를 입력하세요")_연습하기
data = input ("숫자를 입력하세요.")
data2 =int(data)
print(type(data2))

# class 가 달라진다. data = string , data 2 = int  -> 형변환 
```




```python
num1 = int(input("첫번째 숫자를 입력하세요: "))
num2 = int(input("두번째 숫자를 입력하세요: "))
num3 = int(input("세번째 숫자를 입력하세요: "))
num4 = int(input("네번째 숫자를 입력하세요: "))

var1 = num1 >= num2
var2 = num3 < num4
print(var1 and var2)
```

    첫번째 숫자를 입력하세요: 10
    두번째 숫자를 입력하세요: 20
    세번째 숫자를 입력하세요: 30
    네번째 숫자를 입력하세요: 11
    False
    

## String


```python
print("'Hello, world!'")
print('"Hello, world!"')
```

    'Hello, world!'
    "Hello, world!"
    

## String Operators

```python
str1 = "Hello "
str2 = "World "
print('str1 + str2 = ', str1 + str2)

greet = str1 + str2
print('greet * 3 = ', greet * 3)
```

    str1 + str2 =  Hello World 

    greet * 3 =  Hello World Hello World Hello World 
    

## Indexing


```python
greeting = "Hello Kaggle"
print(greeting[6])
```

    K
    

## Slicing


```python
greeting = "Hello Kaggle"
print(greeting[:])
print(greeting[6:])
print(greeting[:6])
print(greeting[3:8])
print(greeting[0:9:2])
```

    Hello Kaggle
    Kaggle
    Hello 
    lo Ka
    HloKg
    


```python
greeting[13]
```





## 리스트


```python
a = [] # 빈 리스트
a_func = list() #list()함수로도 빈 리스트를 만들 수 있다.
b = [1] # 숫자도 요소가 될 수 있다.
c = ['apple'] # 문자열도 요소가 될 수 있다
d = [1, 2, ['apple']] # 리스트 안에 리스트를 요소로 넣을 수 있다.

print(a)
print(a_func)
print(b)
print(c)
print(d)
```

    []
    []
    [1]
    ['apple']
    [1, 2, ['apple']]
    


```python
a =    [1,    2,   3]
# index [[0], [1], [2]]
print(a[0]) # 첫번째 요소
print(a[1]) # 두번째 요소
print(a[2]) # 세번째 요소
print(a[-1])
```

    1
    2
    3
    3
    


```python
a = [['apple','banana','cherry'], 1]

print(a[0]) # 리스트 내의 리스트
print(a[0][0]) # 리스트 내의 리스트의 첫번째 문자열
print(a[0][0][3]) # 리스트 내의 리스트의 첫번째 문자열 'apple' 중 첫번째 인덱스
print(a[0][1]) # 리스트 내의 리스트의 두번째 문자열
print (a[0][2])
```

    ['apple', 'banana', 'cherry']
    apple
    l
    banana
    cherry
    


```python
a = [1,2,3,4,5,6,7,8,9,10]

b = a[:4]  # 인덱스 0부터 3까지
c = a[1:4] # 인덱스 1부터 3까지
d = a[0:7:2] # 인덱스 0부터 6까지 인덱스 2씩 건너 띄우기
e = a[::-1] # 리스트 a의 역순
f = a[::2] # 리스트 전체구간에서 인덱스 2씩 건너띄우기

print("a[:4]", b)
print("a[1:4]", c)
print("a[0:7:2]", d)
print("a[::-1]", e)
print("a[::2]", f)
```

    a[:4] [1, 2, 3, 4]
    a[1:4] [2, 3, 4]
    a[0:7:2] [1, 3, 5, 7]
    a[::-1] [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
    a[::2] [1, 3, 5, 7, 9]
    


```python
a = ['alice', 'bob', 'cat']
b = ['apple', 'banana', 'cherry']
c = a+b

print(c)
```

    ['alice', 'bob', 'cat', 'apple', 'banana', 'cherry']
    


```python
a = ['a','b','c']
b = a*3
c = a*0
print("a * 3:", b)
print("a * 0:", c)
```

    a * 3: ['a', 'b', 'c', 'a', 'b', 'c', 'a', 'b', 'c']
    a * 0: []
    

## 리스트 값 수정하기


```python
a = [0,1,2]
a[1] = "b"

print(a)
```

    [0, 'b', 2]
    

## 리스트 값 추가하기


```python
a = [100, 200, 300]
a.append(400)
print(a)

a.append([500,600])
print(a)
```

    [100, 200, 300, 400]
    [100, 200, 300, 400, [500, 600]]
    


```python
a = [1,2,3]
a.extend([40,500])
print('a.extend([40,500]) result')
print(a)    
```

    a.extend([40,500]) result
    [1, 2, 3, 40, 500]
    


```python
a = [0,1,2]

a.insert(1,100)
print(a)
```

    [0, 100, 1, 2]
    


```python
a = [0,1,2,3]
a[2:2] = [100,200]
print(a)

# 시작과 끝의 범위보다 큰 수를 덮어쓰는 예시
b = [0,1,2,3]
b[1:2] = [100,200,300,400] 
print(b)

# 시작과 끝의 범위가 작을때의 예시
c = [0,1,2,3]
c[1:3] = [100]
print(c)
```

    [0, 1, 100, 200, 2, 3]
    [0, 100, 200, 300, 400, 2, 3]
    [0, 100, 3]
    

## 리스트 값 삭제하기


```python
a =[1,2,1,2]

#리스트의 첫번째 1이 삭제
a.remove(1)
print(a)

#리스트의 두번째 1이 삭제
a.remove(1)
print(a)
```

    [2, 1, 2]
    [2, 2]
    


```python
a = [0,1,2,3,4,5,6,7,8,9]

# 1 삭제
del a[1]
print(a)

b = [0,1,2,3,4,5,6,7,8,9]
# 범위로 삭제
del b[1:3] #list는 항상 시작하는 index부터, 종료하는 n의 n-1까지의 범위를 잡아줍니다.
print(b)
```

    [0, 2, 3, 4, 5, 6, 7, 8, 9]
    [0, 3, 4, 5, 6, 7, 8, 9]
    


```python
#인덱스를 지정한 pop()
a = [0,1,2,3,4]
r = a.pop(1)

print(a)
print(r)
```

    [0, 2, 3, 4]
    1
    


```python
#인덱스를 지정하지 않은 pop()
b = ['a','b','c','d']
x = b.pop()

print(b)
print(x)
```

    ['a', 'b', 'c']
    d
    

## 그 외 유용한 메서드


```python
a = [0,1,2,3]
print(a)

a.clear()
print(a)
```

    [0, 1, 2, 3]
    []
    


```python
a = ["Gold", "Gold", "Silver", "Silver"]
print("Silver가 처음 등장하는 인덱스 번호", a.index("Silver"))
```

    Silver가 처음 등장하는 인덱스 번호 2
    


```python
a = [1, 4, 5, 2, 3]
b = [1, 4, 5, 2, 3]

a.sort()
print("sort():",a)

b.sort(reverse=True)
print("sort(reverse=True):", b)
```

    sort(): [1, 2, 3, 4, 5]
    sort(reverse=True): [5, 4, 3, 2, 1]
    


```python
b = [4,3,2,'a']

b.sort()
print(b)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-38-1624da3f09a9> in <module>()
          1 b = [4,3,2,'a']
          2 
    ----> 3 b.sort()
          4 print(b)
    

    TypeError: '<' not supported between instances of 'str' and 'int'


## 튜플


```python
tuple1 = (0) # 끝에 콤마(,)를 붙이지 않았을 때
tuple2 = (0,) # 끝에 콤마(,)를 붙여줬을 때
tuple3 = 0,1,2

print(tuple1)
print(tuple2)
print(tuple3)

print(type(tuple1)) # 콤마(,)를 붙여주지 않으면 튜플이 아닙니다.
print(type(tuple2)) # 콤마(,)를 붙여주어야 튜플 자료형 입니다.
print(type(tuple3)) # 여러개의 값 일경우 괄호를 없애주어도 튜플 자료형 입니다.
```

    0
    (0,)
    (0, 1, 2)
    <class 'int'>
    <class 'tuple'>
    <class 'tuple'>
    


```python
a = (0,1,2,3,'a')
del a['a']
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-41-c41b8ecfc68f> in <module>()
          1 a = (0,1,2,3,'a')
    ----> 2 del a['a']
    

    TypeError: 'tuple' object does not support item deletion



```python
a = (0,1,2,3,'a')
a[1]='t'
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-42-04fb068f82e0> in <module>()
          1 a = (0,1,2,3,'a')
    ----> 2 a[1]='t'
    

    TypeError: 'tuple' object does not support item assignment


### 튜플 인덱싱 및 슬라이싱 하기


```python
t = (0,1,2,'b',4)

print(t[1])
print(t[3])
```


```python
t = (0,1,2,3,4)
print(t[2:])
print(t[0:2])
```

    (2, 3, 4)
    (0, 1)
    

### 더하기 및 곱셈 연산자 사용


```python
t1 = (0,1,2,3,4)
t2 = ('a','b','c')
t3 = t1+t2
print(t1+t2)
print(t3)
```


```python
t1 = ('a','b')
print(t1*0)
print(t1*3)
```

## 딕셔너리



```python
dic = {'teacher':'alice', 'class': 5, 'studentid': '15', 'list':[1,2,3]}

print(dic['teacher'])
print(dic['class'])
print(dic['list'])
```

    alice
    5
    [1, 2, 3]
    


```python
dic = {'teacher':'alice', 'class': 5, 'studentid': '15', 'list':[1,2,3]}
print(dic['real'])
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-44-fd82dcc94904> in <module>()
          1 dic = {'teacher':'alice', 'class': 5, 'studentid': '15', 'list':[1,2,3]}
    ----> 2 print(dic['real'])
    

    KeyError: 'real'



```python
a = {'name': 'bob', 'job': 'farmer', 'age': 35}
a.keys()
```




    dict_keys(['name', 'job', 'age'])




```python
a = {'name': 'bob', 'job': 'farmer', 'age': 35}
a.values()
```




    dict_values(['bob', 'farmer', 35])




```python
a = {'name': 'chris', 'job': 'painter', 'age': 30}
print(a.get('name'))
print(a.get('dinner'))
print(a.get('dinner', 'empty'))
```

    chris
    None
    empty
    

## 집합 연산자


```python
s = {}
print(type(s))

s = set()
print(type(s))

s = {1,2,3}
print(type(s))
```

    <class 'dict'>
    <class 'set'>
    <class 'set'>
    


```python
a = {1,3,5}
b = {2,4,6}

c = a|b
d = a.union(b)
print("a|b:", c)
print("a.union(b)", d)
```

    a|b: {1, 2, 3, 4, 5, 6}
    a.union(b) {1, 2, 3, 4, 5, 6}
    


```python
a = {1,3,5}
b = {2,4,6}
c = a&b
print(c)

e = {1,2,5}
f = {2,3,5}
g1 = e&f
g2 = e.intersection(f)
print("e&f:", g1)
print("e.intersection(f):", g2)
```

    set()
    e&f: {2, 5}
    e.intersection(f): {2, 5}
    


```python
a = {1,3,5}
b = {2,4,5}

c1 = a-b
c2 = a.difference(b)
print("a-b:", c1)
print("a.difference(b)", c2)
```

    a-b: {1, 3}
    a.difference(b) {1, 3}
    


```python
a = {1,2,3,4,5}
b = {3,4,5,6,7}

c1 = a^b
c2 = a.symmetric_difference(b)
print("a^b", c1)
print("a.symmetric_difference(b)", c2)
```

    a^b {1, 2, 6, 7}
    a.symmetric_difference(b) {1, 2, 6, 7}
    

## if 조건문


```python
a = -5

if a>5:
    print('a is bigger than 5')

elif a > 0:
    print("a is bigger than 0 but a is smaller than 5 ")

else:
    print("a is negative")
```

    a is negative
    

## 반복문


```python
print("Hello World")
```

    Hello World
    


```python
print("Hello World")
print("Hello World")
print("Hello World")
```

    Hello World
    Hello World
    Hello World
    


```python
for i in range(10000):
  print("Hello World")
```

    [1;30;43m스트리밍 출력 내용이 길어서 마지막 5000줄이 삭제되었습니다.[0m
    Hello World
    Hello World
    Hello World
    Hello World
    (손으로 자름.)
    Hello World
    Hello World
    


```python
a = "Kaggle"

for x in a:
    print(x)

    if x == 'g':
        break
```

    K
    a
    g
    


```python
alphabets = ['A', 'B', 'C']
for index, value in enumerate(alphabets):
    print(index, value)
```

    0 A
    1 B
    2 C
    
