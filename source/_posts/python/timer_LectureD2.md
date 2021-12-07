---
title: "Make Timer function in python"
date: 2021-12-07 14:37:20
categories:
- python
- machineLeaning
tags:
- Study
- python
- pythonFuction
- timer
- machineLeaning
---

##  <p style="color:#E84D4D;font-size:120%;">시간 측정 decorator 함수 만들기 </p>

<br>
<hr>




### import time

```python
import time
```

### 실행 시간을 확인 하는 함수 만들기 

```python
def timer(func):
    """ 함수 실행 시간 확인
    :param func: check할 함수 넣을거임
    :return: 걸린 시간
    """
    def wrapper(*args, **kwargs):
        #현재 시간
        time_start = time.time()

        #decorated function 불러오기
        result = func(*args, **kwargs)
        time_total = time.time() - time_start
#
        print("{}, Total time is {: .2f} sec.".format(func.__name__, time_total))

        return result
    return wrapper
```


<div style="border: 1px solid gold">

- <p style="color:#E84D4D;font-size:120%;">가변 매개 변수 args(*) </p>
  
  - 함수를 정의할때 앞에 *가 붙어 있으면, 정해지지 않은 수의 매개변수를 받겠다는 의미
  - 가변 매개변수는 입력 받은 인수를 튜플형식으로 packing 한다. 
  - 일반적으로 *args (arguments의 약자로 관례적으로 사용) 를 사용한다. 
  - 다른 매개 변수와 혼용가능 
<br>

- <p style="color:#E84D4D;font-size:120%;">키워드 매개변수 kwargs(**) </p>
  
  - 함수에서 정의되지 않은 매개변수를 받을 때 사용되며, 딕셔너리 형식으로 전달.
  - 일반 매개변수, 가변 매개변수와 함께 일는 경우 순서를 지켜야함 (일반>가변>키워드 순)
  - **kwargs (Keyword arguments의 약자로 관례적으로 사용 )
  

</div> 

<br><br>

### decorator 함수를 이용하여 시간 확인 함수 설정, 실행

```python

@timer
def check_time(num):
    time.sleep(num)

if __name__ == "__main__":
    check_time(1.5)

```

>out
>> check_time, Total time is  1.50 sec.


<br><br><hr style="border: dashed 5px gold;">
<br><br>
### 관련 이론들을 아래에 적어 놓았다. 

- `timestamp` :python에서 time은 1970년 1월 1일 0시 0분 0초를 기준으로 경과한 시간을 나타냄
- `time_struct` class
  - timestamp가 주어 졌을때 날짜와 시간을 알아내기 위한 API 제공


| name       | value       | Ex.       |
|:----------:|:-----------:|:--------------:|
| `tm_year`  | year        | 1993, 2021  |
| `tm_mon`   | month       | 1~12  |
| `tm_mday`  | day         | 1~31  |
| `tm_hour`  | hour        | 0~23  |
| `tm_min`   | minute      | 0~59  |
| `tm_sec`   | second      | 0~61   |
| `tm_wday`  | 요일      | 0~6 (0 : MON)   |
| `tm_yday`  | 연중 경과일   | 1~366      |
| `tm_isdst` | summertime  | 0: unapply 1: apply |


<br><br>

### time() 함수

<hr>

- 현재 timestamp 얻기

>in
```python
secs = time.time()
print(secs)

```
> out <br>
> 1638870356.8049076

- unix timestamp는 소수로 값을 return, 정수 부분이 초 단위.


 #### 부가적인 time 함수들

  1. gmtime() : GMT 기준의 `time_struct` type으로 변환
>in
```python
tm = time.gmtime(secs)
print(tm)
```
>out
>>time.struct_time(tm_year=2021, tm_mon=12, tm_mday=7, tm_hour=9, tm_min=53, tm_sec=5, tm_wday=1, tm_yday=341, tm_isdst=0)


  2. localtime() : 현지 시간대 기준의 `time_struct` type으로 변환
>in
```python
tm = time.localtime(secs)
print("year:", tm.tm_year)
print("month:", tm.tm_mon)
print("day:", tm.tm_mday)
print("hour:", tm.tm_hour)
print("minute:", tm.tm_min)
print("second:", tm.tm_sec)
```
>out
>>year: 2021  
month: 12  
day: 7  
hour: 18  
minute: 53  
second: 5  
> 

 3. ctime() : `요일 월 일 시:분:초 연도` 형식으로 출력
>in
```python
string = time.ctime(secs)
```
>out
>>Tue Dec  7 18:56:03 2021
> 


 4. strftime() : [strftime](https://docs.python.org/3.8/library/time.html#time.strftime) 과 같은 특정 형식으로 변환가능
    - parameter로 `time_struct` type data를 받기 때문에 위의 함수들을 사용해서 data를 `strftime()`으로 넘겨야 함.
>in
```python
tmt = time.localtime(secs)
string = time.strftime('%Y-%m-%d %I:%M:%S %p', tmt)
print(string)
```
>out
>> 2021-12-07 07:00:54 PM
> 

_인자를 쓰는동안 secs로 계속 썻더니 시간이 계속 올라가고있다 하하하 ^0^_

 5. strptime() : [strftime](https://docs.python.org/3.8/library/time.html#time.strftime) 
과 같은 특정 포멧의 시간을 `time_struct` type 으로 변경. 
>in
```python
string = '2021-12-07 07:00:54 PM'
tmm = time.strptime(string, '%Y-%m-%d %I:%M:%S %p')
print(tmm)
```
>out
>> time.struct_time(tm_year=2021, tm_mon=12, tm_mday=7, tm_hour=19, tm_min=0, 
>> tm_sec=54, tm_wday=1, tm_yday=341, tm_isdst=-1)
> 

 6. sleep() : 일정 시간 동안 시간 지연 시키기 
>in
```python
print("Start-->")
time.sleep(1.5)
print("<--End")
```
>out
>> Start-->  
<--End

- time.sleep(sec) : 초 단위로 시간을 지연 시킨다. 
