---
title: "kaggle :ScatterLine (Q11)"
date: 2021-11-09 16:54:10
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



# kaggle dictation (05)


<br><br>
<hr>


## plotly.graph_objects as go: 를 이용한 Scatter + line G


### 산점도 


- bivariate"이변수" 값을 시각화 하는 기본적인 그래프.
  -  correlation: Positive, Negative, non
- 두 개의 변수 각각의 분포과 변수간의 관계를 확인 할 수 있다. 


ref. 
- [box and scatter plot/Ko.통계](https://danbi-ncsoft.github.io/study/2018/07/23/study_eda2.html)
- [그리는 방법](https://chancoding.tistory.com/1180)

#### 0. data set

https://www.kaggle.com/miguelfzzz/the-typical-kaggle-data-scientist-in-2021

<br>
<hr>


### Subject : 가장 많이 이용하는 computer platform(hardware)

#### 1. data 읽어오기 +  data Frame 만들어 주기 


```python
platform = (
    df['Q11']
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'Platform', 'Q11':'Count'})
    .sort_values(by=['Count'], ascending=False)   
    .replace(['A deep learning workstation (NVIDIA GTX, LambdaLabs, etc)',
              'A cloud computing platform (AWS, Azure, GCP, hosted notebooks, etc)'], 
              ['A deep learning workstation', 'A cloud computing platform'])
          )  
```


ide를 dataframe화 완료.

Q11의 column이름 까지 재설정 완료.

![Q11](/imeges/kgg/Q11_sct.png)

<br><br>

##### 2.표 설정. 

```python
ide = (
    ide
    .count()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'IDE', 0:'Count'})
    .sort_values(by=['Count'], ascending=False)
    )


ide['percent'] = ((ide['Count'] / len(df))*100).round(2).astype(str) + '%'
```


<br>
<br>

#### 3. percent 추가

```python
platform['percent'] = ((platform['Count'] / platform['Count'].sum())*100).round(2).astype(str) + '%'
```

<br>

![Q11_percent](/imeges/kgg/Q11_percent.png)

<br>
<br>

#### 3. 색 지정

```python
colors = ['#033351',] * 6
colors[5] = '#5abbf9'
colors[4] = '#0779c3'
colors[3] = '#0779c3'
```

<br>
<br>

#### 4. 표 재 설정


```python
platform = (platform
           .sort_values(by = ['Count'])
           .iloc[0:15]
           .reset_index())
platform
```
<br>

![platformRe](/imeges/kgg/platformRe.png)

 1. .sort_values(by = ['Count']) : [Count]로 정렬, 
 2. .iloc[0:15] platform의 column 선택: 0~15까지 data 가져오기
 3. .reset_index() : data와 상관 없는 새 index 가져오기

<br>
<br>

#### 5.plotly.graph_objects.Scatter()

> 본격적으로 Scatter G 만들기.

<div style="margin: red 10px; padding: lightgray ">
 
    ## 산점도 점 찍기

```python

fig = go.Figure(go.Scatter(x = platform['Count'], 
                           y = platform["Platform"],
                           text = platform['percent'],
                           mode = 'markers',
                           marker_color =colors,
                           marker_size  = 12))
fig
```

<br>

![Q11_graph](/imeges/kgg/Q11_graph.png)

<br>

1. x = platform['Count'], y = platform["Platform"],
   1.  x축, y축 설정
2. text = platform['percent'], 
   1. text를 넣는다고 하는데 안보이네
3. mode = 'markers',
   1.  Text, lines+markers, makers, line 이 가능 한거 같다. 
   2. [Scatter.mod](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Scatter.html)
4. marker_color =colors, marker_size  = 12)
   1. 산점도 안에 있는 점의 색과 크기

<br>
<br>
</div>


  
<div style="margin: red 10px; padding: lightgray">
<br>

    ## 산점도에 for문을 이용하여 line 연결하기 

```python
for i in range(0, len(platform)):
               fig.add_shape(type='line',
                              x0 = 0, y0 = i,
                              x1 = platform["Count"][i],
                              y1 = i,
                              line=dict(color=colors[i], width = 4))
```

![Q11_Scatter+line](/imeges/kgg/Q11_Scatter+line.png)


> for i in range(0~platform의 길이만큼)
>> fig. add_shape()
>>> type = 'line'  <br> - line모양의 grape shape add <br>
>>> x0 = 0, y0 = i, <br> - 초기값 <br>
>>> x1 = platform["Count"][i], <br> x축 Index 
>>> y1 = i, <br> y축 Index, 마지막 값 <br>
>>>  line=dict(color=colors[i], width = 4) <br>
>>> line의 세부 설정, 색과 두께 

<br>
<br>


- flatform은 .iloc[0:15] 로 뽑아진 list 형식 
따라서 platform["Count"][i]값을 뽑아 낼 수 있다.


<br>
<br>

</div>

<br>
<br>



#### 6. update_traces()

```python
fig.update_traces(hovertemplate='<b>Platform</b>: %{y}<br><extra></extra>'+
                                '<b>Count</b>: %{x}<br>'+
                                '<b>Proportion</b>: %{text}')
```

<br>
<br>

#### 7. Design

```python
fig.update_xaxes(showgrid=True, gridwidth=1, gridcolor='#9f9f9f', ticklabelmode='period')
fig.update_yaxes(showgrid=False)
 
fig.update_layout(showlegend=False, 
                  plot_bgcolor='#F7F7F7', 
                  margin=dict(pad=20),
                  paper_bgcolor='#F7F7F7',
                  yaxis_title=None,
                  xaxis_title=None,
                  title_text="Most Commonly Used <b>Computing Platforms</b>",
                  title_x=0.5,
                  font=dict(family="Hiragino Kaku Gothic Pro, sans-serif", size=17, color='#000000'),
                  title_font_size=35)
```


![Q11_layout](/imeges/kgg/Q11_layout.png)


<br>
<br>

#### 8. Annotation

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
![Q11_final](/imeges/kgg/Q11_final.png)


<br>
<br>
