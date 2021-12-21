---
title: "DTS: Outlier detection"
date: 2021-12-21 15:54:49
categories:
- python
- machineLeaning
tags:
- python
- machineLeaning
- EMD
---

§ [변수 1개를 이용하여 이상값 찾기 ]()

§ [변수 2개를 이용하여 이상값 찾기 ]()

---

## 이상값 찾기 

- 주관적이며 연구자 마다 다르고, 산업에 따라 차이가 있다. 
- 통계에서의 이상값
  + 정규 분포를 이루고 있지 않음 : 이상값이 존재
  + 왜도, 첨도가 발생.
- 균등분포(Uniform distribution)


    1. 변수 1개를 이용하여 이상값 찾기 


```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import statsmodels.api as sm # 검정 확인을 위한 그래프 
import scipy.stats as scistat #샤피로 검정을 위한 Library

covidtotals = pd.read_csv("../input/covid-data/covidtotals.csv")
covidtotals.set_index("iso_code", inplace = True)

case_vars = ["location", "total_cases", "total_deaths", "total_cases_pm", "total_deaths_pm"]
demo_vars = ["population", "pop_density", "median_age", "gdp_per_capita", "hosp_beds"]

covidtotals.head()
```
![covidtotals_Kg](/../../imeges/python/covidtotals_Kg.png) <br><br>


- 결측치와 마찬가지로 covidtotals data를 kaggle note에 불러와서 실행 

<br><br><br>

### 백분위수(quantile)로 데이터 표시  

- 판다스 내부의 함수를 이용하여 확인한다. 

```python
covid_case_df = covidtotals.loc[:, case_vars]
covid_case_df.describe

covid_case_df.quantile(np.arange(0.0, 1.1, 0.1))
#Index이기 때문에 1.1로 표시 
```
![outlier_quantile](/../../imeges/python/outlier_quantile.png)

<br><br>

### 왜도(대칭 정도), 첨도(뾰족한 정도) 구하기 

- 역시 pandas 함수를 이용.

<br> <div style="border: 2px wave salmon">
- 들어가기 전에


![Futrue_warring](/../../imeges/python/Futrue_warring.png)

[pandas.DataFrame.skew](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.skew.html)

</div><br><br>

```python
covid_case_df.skew(axis=0, numeric_only = True)
```

> total_cases        10.804275 < --- -1~1사이에 있어야 대칭이다. <br>
total_deaths        8.929816 <br>
total_cases_pm      4.396091 <br>
total_deaths_pm     4.674417 <br>
dtype: float64 <br>


---

```python
#첨도 구하기 
covid_case_df.kurtosis(axis=0, numeric_only = True)
```

>total_cases        134.979577 <br>
total_deaths        95.737841 <br> 
total_cases_pm      25.242790 <br>
total_deaths_pm     27.238232 <br>
dtype: float64 <br>


5~10 정도 사이에 첨도가 있어야 하는데 보면 괭장히 정규분포를 이루고 있지 않다. 


- 정규 분포를 이루고 있지 않기 때문에 이산값이 있을 확률이 높다는 것을 알 수 있다. 


<br><br>

### 정규성 검정 테스트 

- 샤피로 검정 (샤피로 윌크 검정)
  - 귀무가설 : 표본의 모집단이 정규 분포를 이루고 있다. 
  - 대립가설 : 표본의 모집단이 정규 분포를 이루고 있지 않다.
  - p value > 0.05 이하 : 귀무가설을 충족하지 않아 대립가설로 


```python
# 샤피로 검정
scistat.shapiro(covid_case_df['total_cases'])
```


- 우리는 p value 를 가지고 유의성을 확인한다. 

```python
case_vars = ["location", "total_cases", "total_deaths", "total_cases_pm", "total_deaths_pm"]
demo_vars = ["population", "pop_density", "median_age", "gdp_per_capita", "hosp_beds"]
```
<br><br>

### 이상값 범위 나타내기 
- 통계적 이상값 범위 : 1사분위 (25%), 3사분위(75%) 사이의 거리 
  + 그 거리가 상하좌우 1.5배를 넘으면 이상값으로 여김

