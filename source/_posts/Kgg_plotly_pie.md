---
title: "kaggle :Donut Chart Graph (Q2)"
date: 2021-11-09 20:50:41
tags:
- kaggle
- Plotly
- Donut_Chart
- kaggle_dictation
---
<p style= font-family:Hiragino Kaku Gothic Pro, sans-serif">

# kaggle dictation (07)


<br><br>
<hr>

## plotly.graph_objects as go: 를 이용한 Donut Chart(pie graph)

###pie plot / 파이차트, 도넛차트 

파이차트는 원그래프를 일컫는 말

전체에 대한 각 부분의 비율을 백분율로 나타내어 전체적인 비율을 쉽게 파악 할 수있다.

>fig = go.Figure(data=[go.Pie(labels=labels, values=values, hole=.3)])

원그래프에 'hole'을 추가하여 생성 할 수 있다. 

그 hole 안에 domain(make_subplots())을 추가하여 차트 제목을 추가 할 수도 있다.

같은 원리로 
>fig = go.Figure(data=[go.Pie(labels=labels, values=values, pull=[0, 0, 0.2, 0])])

조각이 떼어진 pie chart 를 생성 할 수 있다.

[pieChart/En](https://plotly.com/python/pie-charts/) : pie chart <br>
[Sunburst Chart/En](https://plotly.com/python/sunburst-charts/) : 다중 pie 차트

<br>
<br>
<hr>


## plotly.graph_objects as go: 를 이용한 Donut_Chart
### Donut_Chart /도넛 차트 (pie chart의 일종)


#### 0. data set

https://www.kaggle.com/miguelfzzz/the-typical-kaggle-data-scientist-in-2021

<br>
<hr>

### Subject : 성별의 분포_

#### 1. data 읽어오기 , data Frame 생성

</p>

```python
gender = (
    df['Q2']
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'Gender', 'Q2':'Count'})
    .replace(['Prefer not to say','Nonbinary','Prefer to self-describe'], 'Other')  
    .replace(['Man','Woman'], ['Male', 'Female']) 
    .groupby('Gender')
    .sum()
    .reset_index()    
          )  
gender
```

<br>

![Q2_gender](/imeges/kgg/Q2_gender.png)

<br>
<br>

##### 2. 색 지정

```python
colors = ['#5abbf9','#033351', 'b9e2fc']
```
<br>
<br>

#### 3. Figure 그리기

```python
fig = go.Figure(data=[go.Pie(labels=gender['Gender'], 
                             values=gender['Count'], 
                             hole=.4)])
```
<br>

- labels=gender['Gender']
  - 컬럼 이름
- values=gender['Count']
  - 컬럼값
- hole=.4
  - 도넛형태로 만들어 주기 위한 hole

![Q2_dounuts](/imeges/kgg/Q2_dounuts.png)

<br>

default 값으로 legend 가 들어 있는 것을 볼 수 있다.

<br>
<br>

#### 4. update_traces()

```python
fig.update_traces(hoverinfo='percent', 
                  textinfo='label', 
                  textfont_size=20,
                  marker=dict(colors=colors, 
                              line=dict(color='#000000', width=1)))
fig
```

![Q2_update_traces](/imeges/kgg/Q2_update_traces.png)

<br>
<br>



#### 5. update_layout()
표의 layout 바꾸기

```python
fig.update_layout(showlegend=False, 
                  plot_bgcolor='#F7F7F7', 
                  paper_bgcolor='#F7F7F7',
                  title_text="<b>Gender</b> Distribution",
                  title_x=0.5,
                  font=dict(family="Hiragino Kaku Gothic Pro, sans-serif", size=25, color='#000000'))

fig
```
<br><br>

1. showlegend=False,
   - Legend 없앰
2. plot_bgcolor='#F7F7F7',
   - BG 회색, 하지만 바꿔도 안보임
3. paper_bgcolor='#F7F7F7',
   - plot이 놓여있는 paper bg
4. title_text="<b>Gender</b> Distribution",
   - 제목 설정
5. title_x=0.5,
   - 제목의 위치
6. font=dict(family="Hiragino Kaku Gothic Pro, sans-serif", size=25, color='#000000')
   - font 종류, 제목의 size, 색


<br>

![Q2Gender_dn](/imeges/kgg/Q2Gender_dn.png)

<hr>
<br>
<br>



#### 6. Annotation

```python
fig.add_annotation(dict(font=dict(size=14),
                                    x=1.1,
                                    y=-0.16,
                                    showarrow=False,
                                    text="@miguelfzzz",
                                    xanchor='left',
                                    xref="paper",
                                    yref="paper"))

fig.add_annotation(dict(font=dict(size=12),
                                    x=-0.28,
                                    y=-0.16,
                                    showarrow=False,
                                    text="Source: 2021 Kaggle Machine Learning & Data Science Survey",
                                    xanchor='left',
                                    xref="paper",
                                    yref="paper"))
fig.show()
```
<br>

![Gender_Final_dn](/imeges/kgg/Gender_Final_dn.png)

<br>


