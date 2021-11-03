---
title: "Study Numpy"
layout: temp
title: Study Numpy
date: 2021-11-01 12:22:02
tags: 
Study Numpy
---



# Numpy
<a href="https://colab.research.google.com/github/YoonHwa-P/MyBlog/blob/main/Numpy.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

## **Numpy 정의**

NumPy는 행렬이나 일반적으로 대규모 다차원 배열을 쉽게 처리 할 수 있도록 지원하는 파이썬의 라이브러리
Numpy는 데이터 구조 외에도 수치 계산을 위해 효율적으로 구현된 기능을 제시 한다. 


Ref. [Wiki](https://ko.wikipedia.org/wiki/NumPy)




---
## 라이브러리에서 numpy 불러오기


우리는 numpy를 import 하여 numpy에 내장되어 있는 함수를 가져와 쓸 수 있다. 일반적으로 np에 저장하여 많이 사용 하는 듯 하다. 


```python
import numpy as np
print(np.__version__)

print ("Numpy의 version을 확인 해 볼 수 있다. ")

temp = np.array([1, 2, 3])
print(type(temp))

print ("Numpy의 type은 nd array인 것을 볼 수 있다.  ")
```

    1.19.5
    Numpy의 version을 확인 해 볼 수 있다. 
    <class 'numpy.ndarray'>
    

Numpy의 Type을 보면 nd array 인 것을 볼 수 있는데 

ND : N dimension  을 의미한다. 
즉 한국어로 번역 해 보면 N차 행렬 정도로 볼 수 있다. 

## Numpy 에서 배열을 생성 해 보자.

### 1차원 배열 생성
1차원의 배열을 생성해서 array와 List의 다른 점을 알아보자.

차이점은 shpae를 찍어 보면 알 수 있다. 
내장 함수 : (fuction or method or attribute)


```python
data1 = [0, 1, 2, 3, 4]
data2 = [10, 9, 8, 7, 6]

My_array = np.array(data1)

print("data1은 List이다. ")
print(data1)
print(type(data1))
#print(data1.shape)  
#List의 경우 shape 함수가 내장 되어 있지 않아 shape를 알 수 없다. 
print("My_array은 numpy형식의 tuple인 것을 알 수 있다.  ")
print(My_array)
print(My_array.shape)
print(type(My_array))

print(".dtype() 는 data의 type을 확인 할 수 있는 function 이다.")
print(My_array.dtype)

```

    data1은 List이다. 
    [0, 1, 2, 3, 4]
    <class 'list'>
    My_array은 numpy형식의 tuple인 것을 알 수 있다.  
    [0 1 2 3 4]
    (5,)
    <class 'numpy.ndarray'>
    .dtype() 는 data의 type을 확인 할 수 있는 function 이다.
    int64
    

 [List](https://colab.research.google.com/github/YoonHwa-P/MyBlog/blob/main/21_11_01_Python_List.ipynb) 형식의 경우 shape 함수가 내장 되어 있지 않은 반면, 
numpy 형식의 np.array 의 경우  [tuple](https://colab.research.google.com/github/YoonHwa-P/MyBlog/blob/main/21_11_01_Python_tuple.ipynb) shape 함수가 내장 되어 에러가 나지 않과 (5, )의 형태로 result가 나오는 것 을 볼 수 있다. 


.dtype() 는 data의 type을 확인 할 수 있는 function 이다.
이때 나타나는 int 64는 64byte의 타입 이라는 것을 알려 준다. 


https://rfriend.tistory.com/285


### 2차원 배열 생성 

4 X 3 배열을 만들어 보자.


```python
my_array4 = np.array([[2,4,6],[8,10,12],[14,16,18],[20,22,24]])
print(my_array4)

my_array4.shape
```

    [[ 2  4  6]
     [ 8 10 12]
     [14 16 18]
     [20 22 24]]
    




    (4, 3)



### 3차원 배열 생성 


```python
my_array5 = np.array([[[1, 2], [3, 4]], [[5, 6], [7, 8]]])
print(my_array5)
my_array5.shape
```

    [[[1 2]
      [3 4]]
    
     [[5 6]
      [7 8]]]
    




    (2, 2, 2)



## Numpy 기본 함수들

  +  arange
  +  zeroes, ones
  + reshape
  + sort
  + argsort


### arange
np.arange(5)를 넣으면 array안에 5개의 숫자가 순서대로 나오는 배열이 자동으로 만들어진다.


```python
Array = np.arange(5)
print(Array)
```

    [0 1 2 3 4]
    

np.arange(a, b, c) : a의 숫자부터 b 숫자까지 C씩 띄워서 배열생성 


```python
aArray = np.arange(1, 9, 2)
print(aArray)
```

    [1 3 5 7]
    

### zeroes, ones 
zeroes와 ones 함수를 살펴 보자 
각 함수들은 0과 1을 채워 넣어 배열을 생성하는 함수 들 이다. 



```python
print("Zeros_Array")
zeros_array = np.zeros((2,3))
print(zeros_array)
print("Data Type is:", zeros_array.dtype)
print("Data Shape is:", zeros_array.shape)

print("Ones_Array")
ones_array = np.ones((3,4), dtype='int32')
print(ones_array)
print("Data Type is:", ones_array.dtype)
print("Data Shape is:", ones_array.shape)
```

    Zeros_Array
    [[0. 0. 0.]
     [0. 0. 0.]]
    Data Type is: float64
    Data Shape is: (2, 3)
    Ones_Array
    [[1 1 1 1]
     [1 1 1 1]
     [1 1 1 1]]
    Data Type is: int32
    Data Shape is: (3, 4)
    

8행에 보면 Array를 행성 하면서 dtype을 **int32**로 지정 해 준 것을 볼 수 있다. 

Zeros_Array의 경우 채워진 0들이 모두 float type의 실수 이기 때문에 0. 이라고 나타는 것을 볼 수 있지만, Ones_Array의 경우 1 만 나타난 Int 형태의 type인 것을 볼 수 있다. 

### reshape
reshape는 행렬의 모양을 바꿔주는 함수이다. 
행렬의 모양을 바꿀 때에는 약간의 제약이 있는데
예를 들어 설명 해 보자면,


 3X4 = 12, 6X2 =12로 형태 변환을 할 수 있지만,
3X5 = 15이기 때문에 변환이 불가능 하다. 
이것이 이해가 가지 않는다면 중학교로 돌아 가야 할 지도 모른다. 





```python
print(ones_array)
print("Data Shape is:", ones_array.shape)
print("Ones_array의 형태를 reshpe로 바꿔보자 \n")
after_reshape = ones_array.reshape(6,2)
print(after_reshape)
print("Data Shape is:", after_reshape.shape)
```

    [[1 1 1 1]
     [1 1 1 1]
     [1 1 1 1]]
    Data Shape is: (3, 4)
    Ones_array의 형태를 reshpe로 바꿔보자
    [[1 1]
     [1 1]
     [1 1]
     [1 1]
     [1 1]
     [1 1]]
    Data Shape is: (6, 2)
    

2차원 배열은 3차원으로도 reshape 할 수 있다.
 
제약 조건은 3 X 4 = 12 였기 때문에 2 X 3 X 2 = 12가 되기 때문에 reshape 가 가능 하다. 


```python
after_reshape = ones_array.reshape(2,3,2)
print(after_reshape)
print("Data Shape is:", after_reshape.shape, "\n")

after_reshape2= ones_array.reshape(-1,6)
print("reshape(-1,6)? \n")
print(after_reshape2)

after_reshape3= ones_array.reshape(3,-1)
print("reshape(3, -1)? \n")
print(after_reshape3)
print("Data Shape is:", after_reshape3.shape)
```

    [[[1 1]
      [1 1]
      [1 1]]
    
     [[1 1]
      [1 1]
      [1 1]]]
    Data Shape is: (2, 3, 2) 
    
    reshape(-1,6)? 
    
    [[1 1 1 1 1 1]
     [1 1 1 1 1 1]]
    reshape(3, -1)? 
    
    [[1 1 1 1]
     [1 1 1 1]
     [1 1 1 1]]
    Data Shape is: (3, 4)
    

만일 2차 행렬 reshape에서 한개의 변수만 정해 졌다면, 나머지 는 -1을 써주면 자동으로 알맞은 변수를 정해 줍니다. 


3차 행렬 역시 남은 1개만 -1을 써서 reshape 함수로 행렬을 변환 할 수 있습니다. 



---

21.11.02(이 아래 부분은 다음에 UPdate하기로 한다. )


### short




```python

```

### argsort

## Numpy 인덱싱과 슬라이딩

## Numpy 정렬



