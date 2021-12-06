---
title: "Pandas_Series"
date: 2021-11-06 15:21:01
tags: 
- Study 
- python
- Pandas
- Series
---


#Pandas에서 series 자료구조

series는 1차원 배열같은 자료 구조를 말한다.

아래 code는 python pandas의 parameter 값이다. 

```python

def __init__(data=None, index=None, dtype=None, name=None, 
             copy=False, fastpath=False)
```


series의 parameter는 data, index, dtype, name, copy, fastpath로 나뉘어져 있는데
name의 경우는 이름 인 것 같고 기본적으로 Index와 value라는 parameter를 많이 이용 하는 듯 하다.


- Index : 배열의 이름
- value : 값


python의 dictionalry와 거의 유사 한 것 같다.  <br>
<sapn style="font-size: 50%; font-color: orange;" >(다음에 찾아보자 오늘은 벅참.) </span>


series의 dtype에는 str, numpy.dtype, or ExtensionDtype, optional Data type 을 
담을 수 있는데 이는 자동으로 값이 입력 되는 것같다.


series 객체를 생성 할 때 value와 Index를 직접 지정 해 줄 수 있다.

```python
import pandas as pd
sr = pd.Series([24000, 20000, 1000, 5000], 
               index=["피자", "치킨", "콜라", "생맥"])
print(sr)
```





구글 코랩에서 작업 하고 있는데, 
아래에 보면 series 객체의 parameter에대한 팝업이 나와 공부 하기 참 편하게 해 준다.


>>def __init__(data=None, index=None, dtype=None, name=None, copy=False, fastpath=False)
One-dimensional ndarray with axis labels (including time series).
> 
>>Labels need not be unique but must be a hashable type. The object supports both integer- and label-based indexing and provides a host of methods for performing operations involving the index. Statistical methods from ndarray have been overridden to automatically exclude missing data
(currently represented as NaN).
Operations between Series (+, -, /, *, **) align values based on their associated index values-- they need not be the same length. The result index will be the sorted union of the two indexes.

<br><br>


>Parameters

>>data : array-like, Iterable, dict, or scalar value  <br>
>>Contains data stored in Series. <br> <br>
>>index : array-like or Index (1d)  <br>
>>Values must be hashable and have the same length as data.  <br>
Non-unique index values are allowed. Will default to RangeIndex (0, 1, 2, ..., n) 
if not provided. If both a dict and index sequence are used, the index will 
override the keys found in the dict. <br><br>
>>dtype : str, numpy.dtype, or ExtensionDtype, optional <br>
Data type for the output Series.  <br>
If not specified, this will be inferred from data.  <br>
See the user guide <basics.dtypes> for more usages. <br><br>
>> name : str, optional  <br><br>
>> The name to give to the Series.  <br><br>
>> copy : bool, default False  <br>
>> Copy input data.  <br>


<h4>type</h4>

- list []
- tuple ()
- set {}
- dict {Key:value}



[series](https://eunguru.tistory.com/220) <br>
[pandas](https://wikidocs.net/32829)  <br>