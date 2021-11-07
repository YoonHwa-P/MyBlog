---
title: "Pandas_DataFrame"
date: 2021-11-06 09:11:02
tags: 
- Study 
- python
- Pandas
- DataFrame
---

#pandas에서 dataFrame 자료구조.


dataFrame은 표와 같은 스프레드 시트 형식의 자료 구조이다.

2차원 배열 또는 리스트, data table 전체를 포함하는 object라고 볼수 있음.

여러개의 column이 있고, 각 컬럼은 숫자, 문자열, boolean type을 담을 수 있다.

dataFrame은 Rew, column에대한 Index 이렇게 2가지 변수를 담고 있는데 matrix라고 할 수 있다.



<hr>
<br>

## pd.dataframe() 

```python
import pandas as pd
values = [['rose', 'tulip', 'Liry'], [4, 5, 6], ['red', 'blue', 'green']]
index = ['flower', 'Number', 'color']
columns = [1, 2, 3]

df = pd.DataFrame(values, index=index, columns=columns)
print(df)
```

df 는  data frame의 준말.




           1     2     3
    flower rose  tulip Liry
    Number 4     5     6
    color  red   blue  green


Index와 column 의 dtype은 object이다. 



데이터프레임은 리스트(List), 시리즈(Series), 딕셔너리(dict), Numpy의 ndarrays,
또 다른 데이터프레임으로 생성할 수 있습니다.



### Ref.
[DataFrame](https://eunguru.tistory.com/221?category=817455)