---
title: "name & namespace"
date: 2021-12-06 16:21:20
categories:
- python
tags:
- Study
- python
- pythonFuction
- namespace
---


##Name

- name : identifier, variable, reference
- object의 구분을 위해 사용

#### object

- object는 value, type, 주소 의 3원소를 가지고 있다. 

Ex)

```python
a = 10
b = 0.12

print(a + b)

```

Memory에 저장된 형태 

object(10, int, ref_cnt=1)0x100

= 100번째 저장된(주소) object는 10(value), int(type)의 속성을 갖는다. 

** 하나의 object는 여러개의 이름(name or ref.)를 가질 수 있다. <br>
** ref_cnt는 reference count를 의미한다.


### name binding
    - assignment
    - import
    - class 정의
    - function 정의 ...

#### assignment

- dictionary (key : value)
- name binding (name : address)

객체(object)에 이름과 주소를 할당


### 이름 작성 규칙

- 하나의 단어로 작성, 문자(한글), 숫자, _ 가능(띄워쓰기 불가능)
- Keyword(+ 다른곳의 함수 이름)은 안쓰는 것이 좋고 쓰지만자
- 숫자로 시작할 수 없으며, 대소문자 구분가능
- _로 시작하는 이름은 class에서 쓰이므로 지양




## name space

- namespace는 이름 관리를 위한 dict container이다. 
- 모든 class, instance, fuction은 자신의 namespace를 가지고 있다. 

### built-in namespace

python에서 제공하는 함수, class, instance 등이 들어있는 곳.

### global name space

내가 만든 함수, 인수, class들이 들어 있는 곳


```python
a = 100
b = a
c = a
a = 101
print(a, b, c)
```

◎ namespace (global)

    a : 0x100
    b : 0x100
    c : 0x100

◎ in memory

    object(100, int ref_cnt=3) 0x100
    object(101, int ref_cnt=1) 0x200

---

- 그렇다면, 왜 ref_cnt 를 하는 것일까 ??
  - 사용되지 않는 객체를 삭제 하기 위해
  - 하나의 객체가 여러개의 이름을 가질 수있다.
  - 하지만, 하나의 이름은 하나의 객체만 가질 수 있다.

---


> In
```python
import sys

a= "Python"
b= a
c= a 
a= "python"
print(f'a=[a], b=[b], c=[c]')

sys.getrefcount(a)
sys.getrefcount(b)
sys.getrefcount(c)
```

> Out
>> a=[a], b=[b], c=[c] <br>
>> 4 <br>
>> 5 <br>
>> 5

## 키워드 

pprint 안의 함수 pprint를 사용하여 키워드 리스트를 출력해 본다.

>in
```python
import keyword
import pprint as pp

pp.pprint(keyword.kwlist)
```

>out
>> ['False',
 'None',
 'True',
 '__peg_parser__',
 'and',
 'as',
 'assert',
 'async',
 'await',
 'break',
 'class',
 'continue',
 'def',
 'del',
 'elif',
 'else',
 'except',
 'finally',
 'for',
 'from',
 'global',
 'if',
 'import',
 'in',
 'is',
 'lambda',
 'nonlocal',
 'not',
 'or',
 'pass',
 'raise',
 'return',
 'try',
 'while',
 'with',
 'yield']


#### assignment

- assignment는 Expression이 아닌 statment이다. 
  - Expression은 한개의 객체로 Evaluate될 수 있는것
  - 이름에 binding할 수 있다. (syntax에서 사용 할 수 있는 위치가 다르기 때문)
  - ver 3.8~ assignment expression (:=)이 추가되어 제공

- assignment의 종류
|  종류     |      기호      |      ex       |  exp  |
|:-----------:|:-------------:|:-------------:|:------:|
| assignment | =  | a = 10 | 10에 a 를 바인딩 |
| assignmented assignment |  **=, +=, -=, *=, //=, %=. <<=, >>=, &=, &#124;=, ^=, @=  | a+= 10  |a에 10을 더한 결과 객체에 a를 바인딩 |


Ref. [youtube, 1hr](https://www.youtube.com/watch?v=_WgARuzoPeU)
