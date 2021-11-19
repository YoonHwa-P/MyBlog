---
title: "Column 뽑아오기"
date: 2021-11-19 18:10:27
categories:
- python
    - plotly
tags:
- kaggle
- Plotly
- plot
- kaggle_Competition
---



-python: for문으로 뽑아오기 


```python
for column in df17_Ea:
...   print(column)

df.coulumns
#이건 column 이름 나옴
```
![Q17_column](/imeges/kgg/Q17_column.png)


경력은  Tenure 인거 같다. 



```python
df18.loc[:0]
```
Question을 다 삭제 해 버렸는데

질문 정히 해 놓은 것이 사라지면 어쩔 수 없이 다시 불러와야지 

![Q17_Question](/imeges/kgg/Q17_Question.png)



## 판다스 옵션 

- pandas 옵션을 조정하여 생략없이 긴 data를 볼 수 있다. 

``` python
pd.describe_option()
```


![PandasOPtion](/imeges/kgg/PandasOPtion.png)


```python
pd.set_option('display.max_seq_items', None)
```


```python
# row 생략 없이 출력
pd.set_option('display.max_rows', None)
# col 생략 없이 출력
pd.set_option('display.max_columns', None)
```


경력 : [Tenure] in df17, [Q8] in df18, [Q15] in 19, [Q6] in 20, [Q6]in 21
연봉 : [Q9] in df18, [Q10] in df19, [Q24] in 20, [Q25]in 21
*2017연봉의 경우 'CompensationAmount' 컬럼에 있지만, 통화가 다르므로 하지 말자. 


