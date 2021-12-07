---
title: "Python 함수 실행하기"
date: 2021-12-07 11:42:17
categories:
- python
tags:
- Study
- python
- pythonFuction
---


## day 1 Lecture (02)
- 사용자 함수 만들고 실행 하기 

<br><br><hr>

### personal function 만들기 

- python에서 함수 만들기 

#### 사용자 정의 함수 

<형식>

```python
def func_name(parameter):
   # ...
   # do something codes
   # ...
    return parameter
```

사용자가 직접 만들어 사용하는 함술로 매개변수 (parameter: 함수에 입력으로 전달 된 값을 받는 변수)와 
인자(argument: 함수를 호출 할 때 전달 하는 입력값)이 있다. 


```python
#/c/Users/brill/Desktop/PyThon_Function/venv/Scripts/python
# -*- coding : utf-8 -*-

def cnt_letter():
    """안에 있는 문자를 세는 함수입니다. """ # 함수를 설명 하는 문구
    print("hi")
    return None
```

- python 문서를 만들때 어떤 위치 (directory)에서 작업 했는지 제일 위에 써 줘야 한다. 
- 또한 cording을 어떤 언어로 했는지 확인 해 줘야 한다. (한글 : UTF-8)
- def : definition 함수를 정의한다. 뒤에 cnt_letter라는 함수 이름을 써준다. 
- def cnt_letter(): ()안에 args를 넣어 주면 된다. 
- Doc : """ `여기` """ `여기` 안에 이 함수가 어떤 함수인지 설명을 써 주어야 한다. 
  - 혼자 일 하는 것이 아니기 때문에 함수를 공통적으로 사용 하기때문.
- print : print
- return : return None

<br><br>

### 함수 실행하기


```python
if __name__ == "__main__":
    print(cnt_letter())
    print(help(cnt_letter()))
    """도움말 : 어떤 함수인지 확인 할 수 있다. """
```

- 함수를 실행한다. 
- __name__ 은 글로벌 변수로 보통 __main__으로 할당된다.
- import 할 때와 구별 하려고 사용한다고 하는데 잘 모르겠다. (외워)

- cnt_letter() 함수를 print 한다. 
- help 함수는 python 내장 함수인데, 


>>class NoneType(object) <br>
 |  Methods defined here: <br>
 |  <br>
 |  __bool__(self, /) <br>
 |      self != 0 <br>
 |   <br>
 |  __repr__(self, /) <br>
 |      Return repr(self). <br>
 |   <br>
 |  ----------------------------------------------------------- <br>
 |  Static methods defined here: <br>
 |  <br>
 |  __new__(*args, **kwargs) from builtins.type <br>
 |      Create and return a new object.  See help(type) for accurate signature. <br>
>>None


실행창에 위와 같은 도움을 준다. 

실행이 성공하면 아래와 같은 결과값이 나온다. 

>Process finished with exit code 0



오늘 들은 강의보다 100배 나은 tistory가 있어서 공유 해본다. .. 아... 내 인생 

[사용자 정의 함수](https://daisy-on.tistory.com/64)
