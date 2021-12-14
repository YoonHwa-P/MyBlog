---
title: "python_basic_Exeption"
date: 2021-12-14 12:01:53
categories:
- python
tags:
- Study
- python
- kaggle
- coding
---

## python

### Exception

```python
# /c/Users/brill/Desktop/PyThon_Function/venv/Scripts/python
# -*- coding : UTF-8

def error01():
    a=10
    a/0
    #ZeroDivisionError: division by zero

def error02():
    a= [1, 2, 3, 4, 5]
    a[10]
    #IndexError: list index out of range

def error03():
    a = 1000
    a + "Hello"
    #TypeError: unsupported operand type(s) for +: 'int' and 'str'

def error04():
    a=10
    a+b
    #NameError: name 'b' is not defined

if __name__ == "__main__":
    error01()
    error02()
    error03()
    error04()
    print("program is done")

```

- 크롤링 코드를 작성 했을때 
> "https://sports.news.naver.com/news?oid=109&aid=0004526080" : 페이지있음,  
> "https://sports.news.naver.com/news?oid=109&aid=0004526081" : 페이지가 없다면,   
> "https://sports.news.naver.com/news?oid=109&aid=0005526080" : 페이지 있음,   
> 
- 크롤링 코드 멈춤
- 프로그램이 멈춰서 안됨 


[Exeption의 종류](https://docs.python.org/3/library/exceptions.html)



### java 의 try catch 구문과 같음

```python
# /c/Users/brill/Desktop/PyThon_Function/venv/Scripts/python
# -*- coding : UTF-8

def try_func(x, idx):
    try:
        return 100/x[idx]
    except ZeroDivisionError:
        print("did't divide zero")
    except IndexError:
        print("not in range of Index")
    except TypeError:
        print("there is type Error")
    except NameError:
        print("it is not definated parameter")
    finally:
        print("무조건 실행됨")


def main():
    a = [50, 60, 0, 70]
    print(try_func(a,1))

    # Zero Division Error
    print(try_func(a,0))

    # Index Error
    print(try_func(a,5))

    # type Error
    print(try_func(a, "hi"))


if __name__ == "__main__":
    main()

```

어떻게던 프로그램이 돌아 갈 수 있도록
만들어 주는 것이 중요하다. 

<br><br>

---

- class 정리 

1. \_\_init__ : set_name, set_id 해 주지 않고, 통합시켜주는 역할
2. \_\_eq__, \_\_ne__ : 부등호 연산자 
3. 상속, 다형성(서로다른 클래스에서 공통으로 쓰는 함수)
4. Exception
5. class attribute / instance attribute / instance method 차이
6. 추상 class (안배웠음)
7. data incapsulation

---
<br><br>



```python
# /c/Users/brill/Desktop/PyThon_Function/venv/Scripts/python
# -*- coding : UTF-8

class SalaryExcept(ValueError): pass # 상속
class TipExept(SalaryExcept): pass # 상속

class Employee:

    MIN_SALARY = 30000
    MAX_Bonus = 20000

    def __init__(self, name, salary = 30000):
        self.name = name
        if salary< Employee.MIN_SALARY:
            raise SalaryExcept("급여가 너무 낮아요!")
        self.salary = salary

    def give_bonus(self, amount):
        if amount > Employee.MAX_Bonus:
            print("보너스가 너무 많아 ")
        elif self.salary + amount < Employee.MIN_SALARY :
            print("보너스 지급 후의 급여도 매우 낮다. ")
        else:
            self.salary += amount

if __name__ == "__main__":
    emp = Employee("YH", salary= 10000)

    try:
        emp.give_bonus(70000)
    except SalaryExcept:
        print("Error Salary")

    try:
        emp.give_bonus(-10000)
    except tipExcept:
        print("Error Tip")
```
