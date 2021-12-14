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




