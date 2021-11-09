---
title: "kaggle :ScatterLine (Q42)"
date: 2021-11-09 22:24:19
categories:
- python
    - plotly
tags:
- kaggle
- Plotly
- Scatter
- kaggle_dictation
- ScatterLine
---



# kaggle dictation (08)


<br><br>
<hr>


## plotly.graph_objects as go_ Scatter + line plot


### 산점도 


- bivariate"이변수" 값을 시각화 하는 기본적인 그래프.
  -  correlation: Positive, Negative, non
- 두 개의 변수 각각의 분포과 변수간의 관계를 확인 할 수 있다. 


ref. 
- [line-and-scatter/En.](https://plotly.com/python/line-and-scatter/)
- [Q11_Scatter](https://yoonhwa-p.github.io/2021/11/09/Kgg_plotly_ScatterLine/)

<br>
<br>

#### 0. data set

https://www.kaggle.com/miguelfzzz/the-typical-kaggle-data-scientist-in-2021

<br>
<hr>


### Subject : 가장 많이 이용하는 Media source

#### 1. data 읽어오기 

Q42로 시작하는 col을 읽어오기.
python의 for문을 이용.

```python
media_cols = [col for col in df if col.startswith('Q42')]
```

![media_cols](/imeges/kgg/media_cols.png)

<br><br>

#### 2. data Frame 만들어 주기 


```python
media = df[media_cols]

media.columns = ['Twitter', 'Email newsletters', 
                'Reddit', 'Kaggle', 'Course Forums', 
                'YouTube', 'Podcasts', 'Blogs',
                'Journal Publications', 'Slack Communities', 'None', 'Other']
```

![df_media_cols](/imeges/kgg/df_media_cols.png)

<br><br>



#### 3.표 설정. 

```python
media = (
    media
    .count()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'Medias', 0:'Count'})
    .sort_values(by=['Count'], ascending=False)
    )

media
```
<br>

![media](/imeges/kgg/media.png)


<br>
<br>


#### 4. 색 지정

```python

colors = ['#033351',] * 12
colors[11] = '#5abbf9'
colors[10] = '#5abbf9'
colors[9] = '#5abbf9'
colors[8] = '#0779c3'
colors[7] = '#0779c3'
colors[6] = '#0779c3'
colors[5] = '#0779c3'
colors[4] = '#0779c3'
```

<br>
<br>

#### 5. percent로 계산한 column 추가

i. add percent column

```python
media['percent'] = ((media['Count'] / len(df))*100).round(2).astype(str) + '%'

media
```

![Q42_percent](/imeges/kgg/Q42_percent.png)

<br><br>

ii. Count값 (column값으로 ) 정렬

```python
media = (media
        .sort_values(by = ['Count'])
        .iloc[0:15]
        .reset_index())

media
```

![Q42_percent_sort](/imeges/kgg/Q42_percent_sort.png)

<br>

    1. Default는 내림차순
    2. iloc으로 0번부터 15까지 List로 긁어오기
    3. reset index()


<br><br>


#### 6.plotly.graph_objects.Scatter()

##### Scatter G 그리기


<hr>
i. 산점도 점 찍기


```python

fig = go.Figure(go.Scatter(x = media['Count'], 
                           y = media["Medias"],
                           text = media['percent'],
                           mode = 'markers',
                           marker_color =colors,
                           marker_size  = 12))
fig
```

<br>

![Q42_graph](/imeges/kgg/Q42_graph.png)

<br>
<br>

<hr>

ii. 산점도에 for문을 이용하여 line 연결하기 

```python
for i in range(0, len(media)):
               fig.add_shape(type='line',
                              x0 = 0, y0 = i,
                              x1 = media["Count"][i],
                              y1 = i,
                              line=dict(color=colors[i], width = 4))  
fig
```

![Q42_Scatter+line](/imeges/kgg/Q42_Scatter+line.png)

<br>

 - for i in range(0~platform의 길이만큼)
 - fig. add_shape()
   - type = 'line'  
     - line모양의 grape shape add <br>
   - x0 = 0, y0 = i, 
     - 초기값 (0, i)에서 시작
     - (0, 0) = other Line Start
   - x1 = platform["Count"][i],
     - x축 Index : count의 값만큼 x축방향으로 Line이 그어진다.
   - y1 = i, 
     - y축 Index, 마지막 값 <br>
   - line=dict(color=colors[i], width = 4) <br>
     - line의 세부 설정, 색과 두께 


<br>
<br>

<hr>



#### 7. update_traces(hovertemplate)

```python
fig.update_traces(hovertemplate='<b>Media Source</b>: %{y}<br><extra></extra>'+
                                '<b>Count</b>: %{x}<br>'+
                                '<b>Proportion</b>: %{text}')
```

<br>
<br>

#### 8. Design
i. 축 grid

```python
fig.update_xaxes(showgrid=True, gridwidth=1, gridcolor='#9f9f9f', ticklabelmode='period')
fig.update_yaxes(showgrid=False)
```

x 축의 grid만 보여줌. tick labe lmode : period

![Q42_grid](/imeges/kgg/Q42_grid.png)

<br>
<br>

[plotly의 axes/En](https://plotly.com/python/axes/)

<br>
ii. update_layout()

```python

fig.update_layout(showlegend=False, 
                  plot_bgcolor='#F7F7F7', 
                  margin=dict(pad=20),
                  paper_bgcolor='#F7F7F7',
                  yaxis_title=None,
                  xaxis_title=None,
                  title_text="Most Commonly Used <b>Media Sources</b>",
                  title_x=0.5,
                  height=700,
                  font=dict(family="Hiragino Kaku Gothic Pro, sans-serif", size=17, color='#000000'),
                  title_font_size=35)
```

![Q42_layout](/imeges/kgg/Q42_layout.png)

<br>
<br>

#### 9. Annotation

```python
fig.add_annotation(dict(font=dict(size=14),
                                    x=0.98,
                                    y=-0.22,
                                    showarrow=False,
                                    text="@miguelfzzz",
                                    xanchor='left',
                                    xref="paper",
                                    yref="paper"))

fig.add_annotation(dict(font=dict(size=12),
                                    x=0.04,
                                    y=-0.22,
                                    showarrow=False,
                                    text="Source: 2021 Kaggle Machine Learning & Data Science Survey",
                                    xanchor='left',
                                    xref="paper",
                                    yref="paper"))

fig.show()
```

![Q42_final](/imeges/kgg/Q42_final.png)

<br>
<br>
