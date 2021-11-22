---
title: "kaggle in East-Asia(data둘러보기)"
date: 2021-11-15 12:10:27
categories:
- python
    - plotly
tags:
- kaggle
- Plotly
- plot
- kaggle_dictation
---


# Kaggle in East Asia


<hr>

## 0. introduction

## 1. World Vs East Asia

1. Kgg 응답자 수 
2. Kgg 응답자 중 직업
   1. 
   
## 2. East Asia
## 3. interestings



- Tenure: 17y 경력? Q6(20y) Q8(18y) Q15(19y) Q6(20, 21y)
- FormalEducation: 17y 학력, Q4 in 18y, 19y, 20y
- 전공 Q5 in18y
- compensation for year: Q10 in 18y,19y, Q24 in 20,

```python
# 연도별 나이 
df21Age_Ea = df21_Ea.loc[:,['Q3','Q1']].reset_index().rename(columns={'Q3':'East_Asia', 'Q1':'2021'}).fillna('etc')
df20Age_Ea = df20_Ea.loc[:,['Q3','Q1']].reset_index().rename(columns={'Q3':'East_Asia', 'Q1':'2020'}).fillna('etc')
df19Age_Ea = df19_Ea.loc[:,['Q3','Q1']].reset_index().rename(columns={'Q3':'East_Asia', 'Q1':'2019'}).fillna('etc')
df18Age_Ea = df18_Ea.loc[:,['Q3','Q2']].reset_index().rename(columns={'Q3':'East_Asia', 'Q2':'2018'}).fillna('etc')
df17Age_Ea = df17_Ea.loc[:,['Country','Age']].reset_index().rename(columns={'Country':'East_Asia', 'Age':'2017'}).fillna('etc')


#data frame 정리

dfAge21_percent =df21Age_Ea.groupby(['East_Asia','2021']).size().reset_index().rename(columns = {0:"Count"})
dfAge21_percent['percent'] =((dfAge21_percent['Count'] / len(df21Age_Ea))*100).round(2)
dfAge21_percent['percent_str'] =((dfAge21_percent['Count'] / len(df21Age_Ea))*100).round(2).astype(str) + '%'
# dfAge21['percent'] = ((media['Count'] / len(df))*100).round(2).astype(str) + '%'
# dfAge_percent21=dfAge21.value_counts('East_Asia',normalize=True).mul(100).round(1).astype(str)
dfAge21_percent


# 나라별 연령대 비율 in East Asia
fig = go.Figure()
for country, group  in dfAge21_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2021'], y = group['percent'], name = country, text=group['percent_str']
   ))
fig.update_layout(barmode="stack",
                 plot_bgcolor = "white",
                 title='2021_ 연령별 나라 비율 in East Asia')
fig.show()

```


![Q1_EastAsia](/imeges/kgg/Q1_EastAsia-1.png)



```python

# 연령별 나라 비율 in East Asia // 여기 name 어떻게 넣는거야 !!!! 
fig = go.Figure()

for country, group  in dfAge21_percent.groupby('2021'):
   fig.add_trace(go.Bar(
       x = group['East_Asia'], y =group['percent'], text=dfAge21_percent['percent_str']
   ))
fig.update_layout(barmode="stack",
                 plot_bgcolor = "white",
                 title='2021_ 나라별 연령 비율 in East Asia')
fig.show()
```


![img_1.Q1_EastAsia](/imeges/kgg/Q1_EastAsia-2.png)


- 못해먹겠다. 

[Metaplotly](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=cjh226&logNo=221266463090)