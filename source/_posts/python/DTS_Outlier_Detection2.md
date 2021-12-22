---
title: "DTS: Outlier detection02"
date: 2021-12-21 15:54:49
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

---

## 이상값 찾기 

- 서로 겹치는 값이 있거나, 한 변수의 범주거나 연속일 경우 

- 수치형 데이터에 대한 상관행렬
```python
# 상관관계 확인
covidtotals.corr(method = "pearson")
```
- 