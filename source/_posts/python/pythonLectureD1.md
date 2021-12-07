---
title: "Python Class and function"
date: 2021-12-06 12:21:20
categories:
- python
tags:
- Study
- python
- pythonFuction
---

## day 1 Lecture

<br><br><hr>


### 1. 새로운 Project 시작

- python project를 새로 시작 해 보자. 

<br>

![create_NewProject](/../../imeges/python/create_NewProject.png)

<br>

![create_NewProject_file](/../../imeges/python/create_NewProject_file.png)

pycham을 실행하여 project file을 만들어도 되지만, 원하는 directory에 file 을 만들고, 그 file에서 pycharm을 실행 해도 된다. 



### 2. main.py : 

#### stop point

![main.py](/../../imeges/python/main_py.png)


```python
# This is a sample Python script.
# Press Shift+F10 to execute it or replace it with your code.
# Press Double Shift to search everywhere for classes, files, tool windows, actions, and settings.
# 인공지능 / 머신러닝 --> 리서치 관점 (논문리뷰, 정리, End User) / Engineer 관점


def print_hi(name):
    # Use a breakpoint in the code line below to debug your script.
    print(f'Hi, {name}')  # Press Ctrl+F8 to toggle the breakpoint.

# f string
# Press the green button in the gutter to run the script.

```

- [BreakPoint](https://www.jetbrains.com/help/pycharm/using-breakpoints.html)

#### def name(name): 함수 정의 하기

```python
def print_hi(name):
    print(f'Hi, {name}')

if __name__ == '__main__':
    print_hi('PyCharm')

print("hi", name)
```


- Ctrl + shift + F10  : Run


- def 로 함수 정의 이름(print_hi), 인수(name)을 설정 :
  - 원하는 동작(print(f'Hi, {name})) 넣기
- 함수 실행 : 왜 이렇게 해야 하는가를 공부 해 오자.
  - if __name__ == '__main__':
      print_hi('PyCharm')

- print("hi", print_hi('YH'))

>Hi, PyCharm <br>
Hi, YH <br>
hi None <br>


### 3. Import.

```python
numpy == 1.21.4
pandas ==1.3.4
seaborn==0.11.2
matplotlib==3.5.0
scipy ==1.7.3
scikit-learn == 1.0.1
```


<h4> 가상환경 설정을 하는 이유 </h4>

Python PKG : 20~30 만개 있다. 
-	라이브러리 : 종속성이 있다.
-	범용성이 좋아서 data analysis, web site development, 앱, 게임 등 개발, GUI, 개발 그 외 여러가지 가능
-	Matplotlib (기초가 되는 라이브러리, 3.5.0)를 참조하여 seaborn, plotly, …
-	버전 관리 불가. 
+ data 분석을 위한 가상 환경
같은 Local machine 위에 
Game 개발을 위한 가상 환경 조성 (다른 버전을 사용 할 수 있다. )

PKG 설정: 

https://pypi.org 에 들어가서 PKG 버전을보고 설치 하면 된다. 
