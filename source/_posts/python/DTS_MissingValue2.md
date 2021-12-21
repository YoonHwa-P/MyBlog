---
title: "DTS: Missing Value detection(02)"
date: 2021-12-22 08:11:53
categories:
- python
- machineLeaning
tags:
- python
- machineLeaning
- EDA
---

§ [결측치 찾기 이론](https://yoonhwa-p.github.io/2021/12/21/python/DTS_MissingValue)

§ [결측치 찾기 실습](https://yoonhwa-p.github.io/2021/12/21/python/DTS_MissingValue2)

§ [변수 1개를 이용하여 이상값 찾기 ](https://yoonhwa-p.github.io/2021/12/21/python/DTS_Outlier_detection)

§ [변수 2개를 이용하여 이상값 찾기 ]()

♠ [Ref.01](https://wooono.tistory.com/103)

---

[data in Kaggle](https://www.kaggle.com/yoonhwayam/ml-covid-data-covidtotals-class-211220/data?scriptVersionId=82892462)

note를 public으로 올려는 놨는데 검색이 될까 모르겠네요.

## Missing Value : 결측치 확인

### data Loading

```python
import pandas as pd
covidtotals = pd.read_csv("../input/covid-data/covidtotals.csv")
covidtotals.head()
```

![MissingValue_covidtotals](/../../imeges/python/MissingValue_covidtotals.png)

### data info

```python
covidtotals.info()
```

![MissingValue_covid_info](/../../imeges/python/MissingValue_covid_info.png)

### data division

- 인구통계 관련 column
- Covid 관련 column

```python
case_vars = ["location", "total_cases", "total_deaths", "total_cases_pm", "total_deaths_pm"]
demo_vars = ["population", "pop_density", "median_age", "gdp_per_capita", "hosp_beds"]
```

### demo_vars column별로 결측치를 측정

```python
covidtotals[demo_vars].isnull().sum(axis = 0) # column별로 결측치를 측정
```

![MissingValue_covid_isnullsum](/../../imeges/python/MissingValue_covid_isnullsum.png)


### case_vars column별로 결측치를 측정

```python
covidtotals[case_vars].isnull().sum(axis = 0) # column별로 결측치를 측정
```

![MissingValue_covid_nullSum](/../../imeges/python/MissingValue_covid_nullSum.png)


- case_vars 에는 결측치가 없지만, demo_vars에는 결측치가 있는 것을 확인 할 수 있다. 

| || || |
|: :||: :||: :|
|pop_density ||      12|
|median_age  ||    24|
|gdp_per_capita||    28|
|hosp_beds  ||   46|

위의 column들에 각각 수만큼의 결측치를 확인 할 수 있다. 

### 행 방향으로 발생한 결측치 확인

```python
demovars_misscnt = covidtotals[demo_vars].isnull().sum(axis = 1)
demovars_misscnt.value_counts()
```
>0    156  <br>
1     24  
2     12  
3     10  
4      8  
dtype: int64

```python
covidtotals[case_vars].isnull().sum(axis = 1).value_counts()
```
>0    210
dtype: int64


### 인구통계 데이터가 3가지 이상 누락된 국가를 나열하기

```python
["location"] + demo_vars
covidtotals.loc[demovars_misscnt >= 3, ["location"] + demo_vars].T

```
![MissingValue_covid_Location](/../../imeges/python/MissingValue_covid_Location.png)



### case에는 누락국가가 없지만, 그냥 한번 확인 

```python
casevars_misscnt = covidtotals[case_vars].isnull().sum(axis = 1)
casevars_misscnt.value_counts()
```
>0    210
dtype: int64

```python
covidtotals[covidtotals['location'] == "Hong Kong"]
```


```python
temp = covidtotals.copy()
temp[case_vars].isnull().sum(axis = 0)
temp.total_cases_pm.fillna(0, inplace = True)
temp.total_deaths_pm.fillna(0, inplace = True)
temp[case_vars].isnull().sum(axis = 0)

```

![MissingValue_covid_Del](/../../imeges/python/MissingValue_covid_Del.png)

---

이건 잘 모르겠다. 그냥 삭제 할 수 있다. 