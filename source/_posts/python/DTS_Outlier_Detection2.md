---
title: "DTS: Outlier detection02"
date: 2021-12-22 10:54:49
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

§ [data 출처](https://ourworldindata.org/coronavirus-source-data)
---

## 이상값 찾기 

- 서로 겹치는 값이 있거나, 한 변수의 범주거나 연속일 경우 

- 수치형 데이터에 대한 상관행렬
```python
# 상관관계 확인
covidtotals.corr(method = "pearson")
```
- corr <|0.2| : 약한 상관관계
- corr < |0.3~0.6| : 중간정도의 상관관계
- 상관관계를 확인 할 수 있다. 


### crosstab
- 총 사망자 분위수별 총 확진자 분위수의 크로스 탭 표시 
    - case: 확진자수
    - deaths: 사망자 수 

```python
pd.crosstab(covidtotalsonly["total_cases_q"], 
            covidtotalsonly["total_deaths_q"])
```


![Outlier_crosstab](/../../imeges/python/Outlier_crosstab.png)

- 매우 낮은 수로 사망 했지만, 확진이 중간 = 이상치
- 

```python
covidtotals.loc[(covidtotalsonly["total_cases_q"]== "very high")
                & (covidtotalsonly["total_deaths_q"]== "medium")].T
```



```python

fig, ax = plt.subplots()
sns.regplot(x = "total_cases_pm", y = "total_deaths_pm", data = covidtotals, ax = ax)
ax.set(xlabel = "Cases Per Million", ylabel = "Deaths Per Million", title = "Total Covid Cases and Deaths per Million by Country")
ax.ticklabel_format(axis = "x", useOffset=False, style = "plain")
plt.xticks(rotation=90)
plt.show()
```

![Outlier_regplot](/../../imeges/python/Outlier_regplot.png)


