---
title: "kaggle :HorizontalBar (Q9)"
date: 2021-11-09 14:00:00
categories:
- python
    - plotly
tags:
- kaggle
- Plotly
- BarGraph
- kaggle_dictation
- HorizontalBar
---



# kaggle dictation (04)


<br><br>
<hr>


## plotly.graph_objects as go: 를 이용한 bar graph
### HorizontalBar plot /가로 막대 차트


#### 0. data set

https://www.kaggle.com/miguelfzzz/the-typical-kaggle-data-scientist-in-2021

<br>
<hr>


### Subject : 어떤알고리즘을 선호 하는 지 알아보는 Horizontal bar

#### 1. data 읽어오기 


```python
# Features that start with Q7
ide_cols = [col for col in df if col.startswith('Q9')]

ide = df[ide_cols]

print(ide)
```

![print(ide)](/imeges/kgg/print(ide).png)



#### 2. data Frame 만들어 주기 

Q9로 시작하는 col 이 True이면, col을 긁어와라 df로 ?? 

ide를 dataframe화 완료.

```python
ide.columns = ['JupyterLab', 'RStudio', 'Visual Studio', 'VSCode', 
               'PyCharm', 'Spyder', 'Notepad++', 'Sublime Text', 'Vim, Emacs, or similar', 
               'MATLAB', 'Jupyter Notebook', 'None', 'Other']
```

Q9의 column이름 재 설정.


<br><br>

##### 3.표 설정. 

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
ide['percent'] = ((ide['Count'] / len(df))*100).round(2).astype(str) + '%'
```

<br>
<br>

#### 4. 색 지정

```python
colors = ['#033351',] * 13
colors[0] = '#5abbf9'
colors[1] = '#5abbf9'
colors[2] = '#0779c3'
colors[3] = '#0779c3'
colors[4] = '#0779c3'
colors[5] = '#0779c3'
colors[6] = '#0779c3'
colors[7] = '#0779c3'
```
<br>
<br>

#### 5. bar Graph 만들기


```python
fig = go.Figure(go.Bar(
            x=ide['Count'],
            y=ide['IDE'],
            text=ide['percent'],
            orientation='h',
            marker_color=colors
                        ))
```

<br>
<br>


#### 6. update_traces()

```python
fig.update_traces(texttemplate='%{text}', 
                  textposition='outside',
                  cliponaxis = False,
                  hovertemplate='<b>IDE</b>: %{y}<br><extra></extra>'+
                                '<b>Count</b>: %{x}',
                  textfont_size=12)
```

<br>
<br>

#### 7. Design

```python
fig.update_xaxes(showgrid=False)
fig.update_yaxes(showgrid=False)
 
fig.update_layout(showlegend=False, 
                  plot_bgcolor='#F7F7F7', 
                  margin=dict(pad=20),
                  paper_bgcolor='#F7F7F7',
                  xaxis={'showticklabels': False},
                  yaxis_title=None,
                  height = 600,
                  xaxis_title=None,
                  yaxis={'categoryorder':'total ascending'},
                  title_text="Most Commonly Used <b>IDE's</b>",
                  title_x=0.5,
                  font=dict(family="Hiragino Kaku Gothic Pro, sans-serif", size=15, color='#000000'),
                  title_font_size=35)
```

<br>
<br>

#### 8. Annotation

```python
fig.add_annotation(dict(font=dict(size=14),
                                    x=0.98,
                                    y=-0.17,
                                    showarrow=False,
                                    text="@miguelfzzz",
                                    xanchor='left',
                                    xref="paper",
                                    yref="paper"))

fig.add_annotation(dict(font=dict(size=12),
                                    x=0,
                                    y=-0.17,
                                    showarrow=False,
                                    text="Source: 2021 Kaggle Machine Learning & Data Science Survey",
                                    xanchor='left',
                                    xref="paper",
                                    yref="paper"))


fig.show()
```

<br>
<br>
