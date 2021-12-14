---
title: "python_basic_Bank"
date: 2021-12-13 12:01:53
categories:
- python
tags:
- Study
- python
- kaggle
- coding
---

## python


### Bank _ 계좌 만들기

```python
# /c/Users/brill/Desktop/PyThon_Function/venv/Scripts/python
# -*- coding : UTF-8

class Human:

    def __init__(self, name):
        self.name = name


if __name__ == "__main__":
    human01 = Human(name="A")
    human02 = Human(name="A")

    print(human01 == human02)
    print("human 01 : ", human01)
    print("human 02 : ", human02)
```

>False
human 01 :  <__main__.Human object at 0x000001686E41CC10>
human 02 :  <__main__.Human object at 0x000001686E41CE50>
> 

저장되는 장소가 다르기 때문에 다르다. 

### Bank _ customer ID 확인하기

```python
# /c/Users/brill/Desktop/PyThon_Function/venv/Scripts/python
# -*- coding : UTF-8

class Bank:

    #instance attribute
    def __init__(self, cust_id, balance=0):
        self.balance = balance
        self.cust_id = cust_id

    #instance methode
    def withdraw(self, amount):
        self.balance -= amount

    def __eq__(self, other):
        print("__eq()__ is called")
        return self.cust_id == other.cust_id

if __name__ == "__main__":
    account01 = Bank(123, 1000)
    account02 = Bank(123, 1000)
    account03 = Bank(456, 1000)
    print(account01 == account02)
    print(account02 == account03)
    print(account01 == account03)

```


> __eq()__ is called
True
__eq()__ is called
False
__eq()__ is called
False
> 
<br><br>

- 부등호 연산자 
  - != : __ne__()
  - \>= : __ge__()
  - <= : __le__()
  - \> : __gt__()
  - < : __lt__()


### __eq()__ 함수 사용하기

```python
# /c/Users/brill/Desktop/PyThon_Function/venv/Scripts/python
# -*- coding : UTF-8

class Bank:

    #instance attribute
    def __init__(self, cust_id, balance=0):
        self.balance, self.cust_id = balance, cust_id


    #instance methode
    def withdraw(self, amount):
        self.balance -= amount

    def __eq__(self, other):
        print("__eq()__ is called")
        return (self.cust_id == other.cust_id) and (type(self) == type(other))

class Phone:

    def __init__(self, cust_id):
        self.cust_id = cust_id

    def __eq__(self, other):
        return self.cust_id == other.cust_id


if __name__ == "__main__":
    account01 = Bank(1234)
    phone01 = Phone(1234)

    print(account01 == phone01)

```
> __eq()__ is called
False
> 

eq를 불러와서 같은지 확인 할 수 있다. 


### 접근기록, log 기록 확인하기


```python
# /c/Users/brill/Desktop/PyThon_Function/venv/Scripts/python
# -*- coding : UTF-8


class Bank:
    def __init__(self, cust_id, name, balance = 0):
        self.cust_id, self.name, self.balance = cust_id, name, balance

    def __str__(self):
        cust_str = """
        customer:
            cust_id : {cust_id}
            name : {name}
            balance : {balance}
        """.format(cust_id = self.cust_id, name = self.name, balance= self.balance)

        return cust_str

if __name__ == "__main__":
    bank_cust = Bank(123, "YH")
    print(bank_cust)

```

- DB에 저장 되지 않지만, 로그 기록을 확인 할 수 있다. 



### str() and repr() 비교


```python
# /c/Users/brill/Desktop/PyThon_Function/venv/Scripts/python
# -*- coding : UTF-8


class Bank:
    def __init__(self, cust_id, name, balance = 0):
        self.cust_id, self.name, self.balance = cust_id, name, balance

    def __str__(self):
        cust_str = """
        customer:
            cust_id : {cust_id}
            name : {name}
            balance : {balance}
        """.format(cust_id = self.cust_id, name = self.name, balance= self.balance)

        return cust_str

    def __repr__(self):
        cust_str = "Bank({cust_id}, '{name}', {balance})".format(cust_id = self.cust_id, name = self.name, balance= self.balance)
        return cust_str

if __name__ == "__main__":
    bank_cust = Bank(123, "YH")
    print(str(bank_cust))
    print(repr(bank_cust))
```

[difference of str() and repr()](https://www.codingem.com/str-vs-repr-in-python/)


