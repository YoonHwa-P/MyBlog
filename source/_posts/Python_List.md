---
title: "python_List"
date: 2021-11-05 09:11:28
categories:
- python
tags: 
- python
- List
- studyPython
---



## google Colaboratory 를 이용한 실습




<a href="https://colab.research.google.com/github/YoonHwa-P/MyBlog/blob/main/21_11_01_Python_List.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

# Intro_ print와 주석처리

- print Out


```python
print("Hello, World !!")
```

    Hello, World !!
    

이제 우리는 모든 것을 나타 낼 수 있습니다. 

- 주석처리


```python
# 한 줄로 주석 처리 하는 방법 
"""
여러줄은 이렇게 
print("Hello, World??")
"""

print("Hello, World!!") #주석 처리한 부분은 안나오지
```

    Hello, World!!
    

# 변수의 종류


* int : 정수
* float : 실수
* bool : 참/거짓
* none : null
* String : 문자


---



이전에 배웠던 JAVA와는 다르게 변수형을 지정 해 주지 않아도 된다


Type 함수는 그 변수의 type이 어떤 것인지 알려준다. 


```python
N_int = 1
print (type(N_int))
#Type 함수 : type 을 print 해 준다 

N1_float = 2.0
print (type(N1_float))

bool_N = True
print (type(bool_N))

X= None
print (type(X))
```

    <class 'int'>
    <class 'float'>
    <class 'bool'>
    <class 'NoneType'>
    
---

# 연산

- 사칙연산

Number type


```python
a = 1
b = 2
c = 3.14
d = 1.414
print ("정수_int_의 사칙연산 ")
print ('a + b = ' , a+b)
print('a - b = ', a-b)
print('a * b = ', a*b)
print('a / b = ', a/b)
print('a // b = ', a//b)
print('a % b = ', a%b)
print('a ** b = ', a**b)
print ("실수_float_의 사칙연산 ")
print('c + d =', c+d)
print('c - d =', c-d)
print('c * d =', c*d)
print('c / d =', c/d)
print('c // d =', c//d)
print('c % d =', c%d)
print('c ** d =', c**d)

```

    정수_int_의 사칙연산 
    a + b =  3
    a - b =  -1
    a * b =  2
    a / b =  0.5
    a // b =  0
    a % b =  1
    a ** b =  1
    실수_float_의 사칙연산 
    c + d = 4.554
    c - d = 1.7260000000000002
    c * d = 4.43996
    c / d = 2.2206506364922207
    c // d = 2.0
    c % d = 0.3120000000000003
    c ** d = 5.042646477882085
    

- String 연산


```python
str1 = "hi"
str2 = "bye"
print('str1 + str2 = ', str1 + str2 )
print ('str 1 * 3 + str * 2 = ', str1 * 3 + str2 * 2  )
```

    str1 + str2 =  hibye
    str 1 * 3 + str * 2 =  hihihibyebye
    

# indexing


```python
greeting = "Hi!Hellow? AnNyung!"
print(greeting)
#greeting, 이하니처럼 인사를 해 봅시다. 

print(greeting[6:])
print(greeting [:6])


G = "12345 6789@"
print (G[2:])
print (G[:3])
print (G[1:9])
print (G[0:7:3])

""" 
[a : b : c]
  a :  시작 index
  b : 끝 Index
  c : 띄워서
""" 
```

    Hi!Hellow? AnNyung!
    low? AnNyung!
    Hi!Hel
    345 6789@
    123
    2345 678
    146
    




    ' \n[a : b : c]\n  a :  시작 index\n  b : 끝 Index\n  c : 띄워서\n'


---

# List

- List 함수 종류
  + .append() : 추가
  + .extend() : 연장
  + .remove() : 제거
  + .del() : 제거
  + .pop()  :Index설정
  + .clear() : 전체 삭제

  Internet에 더 많은 함수를 찾을 수 있는 documents 가 있을 것이다. 
^^

  [List Function](https://docs.python.org/3/tutorial/datastructures.html)

List란, java로 치면 배열

List create 부터 


```python
print("List 를 생성 해 봅시다.")
A = [] #변수선언으로 빈 리스트 만들기
a= list() #List 함수로 만들기 

a = ['I', 'Have', ['a', 'DreAm']]


print ("a함수에 직접적 Index로 넣어주기")
print (a)
print ("a[0]= 'YOU'의 결과 ")
a[0]= 'YOU'
print (a)


#List에 값 추가 해 주기
print(A)
A.append('U')
A.append('song')
A.append(['to', 'sing'])
print("A[0:] : A의 모든 원소 추출")
print(A[0:])
print(" ")
#Extend 
print("a : a에 Extend를 이용하여 원소 추가하기")
print(a)
a.extend('U')
print(a)
a.extend(['song', 'to', 'sing'])
print(a)

#Extend 와 같은 느낌으로 "+=" 연산자를 써 줄수 있다. 
print("a : a에 연산자를 이용하여  원소 추가하기")  
a += ['Like U']
print(a)

#Insert는 중간에 인덱스 값을 넣어 주어서 List에 값을 추가 해 줄 수 있다. 
print(a)
print("a : a에 insert를 이용하여 원하는 Index번호에 원소 추가하기")
a.insert(1,'do not')
print(a)
del a[1:1]
#다음 코드를 위해 a의 Index 1에 넣어준 원소를 제거 했음 
```

    List 를 생성 해 봅시다.
    a함수에 직접적 Index로 넣어주기
    ['I', 'Have', ['a', 'DreAm']]
    a[0]= 'YOU'의 결과 
    ['YOU', 'Have', ['a', 'DreAm']]
    []
    A[0:] : A의 모든 원소 추출
    ['U', 'song', ['to', 'sing']]
     
    a : a에 Extend를 이용하여 원소 추가하기
    ['YOU', 'Have', ['a', 'DreAm']]
    ['YOU', 'Have', ['a', 'DreAm'], 'U']
    ['YOU', 'Have', ['a', 'DreAm'], 'U', 'song', 'to', 'sing']
    a : a에 연산자를 이용하여  원소 추가하기
    ['YOU', 'Have', ['a', 'DreAm'], 'U', 'song', 'to', 'sing', 'Like U']
    ['YOU', 'Have', ['a', 'DreAm'], 'U', 'song', 'to', 'sing', 'Like U']
    a : a에 insert를 이용하여 원하는 Index번호에 원소 추가하기
    ['YOU', 'do not', 'Have', ['a', 'DreAm'], 'U', 'song', 'to', 'sing', 'Like U']
    

리스트 연산 


```python
C = a+A
print("a+A의 연산결과 : ", C)
```

    ['U', 'do not', 'song', 'to', 'sing', 'U', 'song', ['to', 'sing']]
    

리스트에 값 삭제 해 주기 


```python
b=[1, 2, 3, 4, 5, 6 ]
B=[7, 8, 9, 10, 11, 12, 13, 1, 4]
print("리스트 값 삭제하기 ")
print(b)
print(B)

print("b의 1번째 삭제")
#1개씩 삭제 해 주기 
del b[1]
print(b)

print("B의 1~2 번째 삭제 : [1:2]")
#범위로 삭제 해 주기 
del B[1:2]
print(B)


#중복 값을 삭제 해 주는 function
K=[0, 1, 2, 3, 4, 5, 8, 6, 7, 9, 10, 0, 12]
print("K : ", K)

K.remove(0)
print("K.remove(0) : ", K)

K.insert(0, 1)
print("K.insert(0,1) : ", K)



#pop은 Index를 정해서 특정 문자에 정해 줄 수 있다.  
S = b.pop()
SS= B.pop(1)
print("b.pop() : ")
print (S, " : ()안에 아무것도 쓰지 않으면 맨 마지막 ")
print("B.pop(1) : ")
print(SS, " : ()안에 숫자를 쓰면 (1) 1번Index 뽑아내기")

#pop과 같은 느낌으로 특정 문자를 뽑아 내기 
print("Index 숫자로 pop과 같은 기능을 실행 할 수 있다. ")
print (a[0:])
print (a[2][1])
print (a[2][1][3])

#List 전체 삭제


```

    리스트 값 삭제하기 
    [1, 2, 3, 4, 5, 6]
    [7, 8, 9, 10, 11, 12, 13, 1, 4]
    b의 1번째 삭제
    [1, 3, 4, 5, 6]
    B의 1~2 번째 삭제 : [1:2]
    [7, 9, 10, 11, 12, 13, 1, 4]
    K :  [0, 1, 2, 3, 4, 5, 8, 6, 7, 9, 10, 0, 12]
    K.remove(0) :  [1, 2, 3, 4, 5, 8, 6, 7, 9, 10, 0, 12]
    K.insert(0,1) :  [1, 1, 2, 3, 4, 5, 8, 6, 7, 9, 10, 0, 12]
    b.pop() : 
    6  : ()안에 아무것도 쓰지 않으면 맨 마지막 
    B.pop(1) : 
    9  : ()안에 숫자를 쓰면 (1) 1번Index 뽑아내기
    Index 숫자로 pop과 같은 기능을 실행 할 수 있다. 
    ['hi', 'do not', 'Have', ['a', 'DreAm'], 'U', 'song', 'to', 'sing', 'd', 'i', 'd', ' ', 'n', 'o', 't']
    a
    


    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    <ipython-input-112-d37801788a81> in <module>()
         40 print (a[0:])
         41 print (a[2][1])
    ---> 42 print (a[2][1][3])
         43 
         44 #List 전체 삭제
    

    IndexError: string index out of range


List 값을 덮어 쓰는 기능 


```python
print ("A[] = ", A) 
print ("A[1:2]= ['i', 'do', 'NOT', 'know']의 결과 ")
A[1:2]= ['i', 'do', 'NOT', 'know']
print (A) 
# A[]에 1번 Index에 범위보다 큰 수가 엎어 씌어 진 것을 볼 수 있다. 

del A [1:4]
print("del A [1:4] : ", A)
```

    A[] =  ['U', 'i', 'do', 'NOT', 'know', 'do', 'NOT', 'know', 'do', 'NOT', 'know', ['to', 'sing']]
    A[1:2]= ['i', 'do', 'NOT', 'know']의 결과 
    ['U', 'i', 'do', 'NOT', 'know', 'do', 'NOT', 'know', 'do', 'NOT', 'know', 'do', 'NOT', 'know', ['to', 'sing']]
    del A [1:4] :  ['U', 'know', 'do', 'NOT', 'know', 'do', 'NOT', 'know', 'do', 'NOT', 'know', ['to', 'sing']]
    


<hr style="border-color: #18a217" >

# Ref.


https://dojang.io/course/view.php?id=7

보고 공부 할 수 있어요 
