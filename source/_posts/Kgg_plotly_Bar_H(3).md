---
title: "kaggle :HorizontalBar (Q7)"
date: 2021-11-09 20:01:03
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


# kaggle dictation (06)


<br><br>
<hr>


## plotly.graph_objects as go: 를 이용한 bar graph
### HorizontalBar plot /가로 막대 차트


#### 0. data set

https://www.kaggle.com/miguelfzzz/the-typical-kaggle-data-scientist-in-2021

<br>
<hr>

### Subject : 가장 많이쓰는 programming 언어_Horizontal bar

#### 1. data 읽어오기 
<a style="border:3px;">

<br>

Q7에는 sub가 많기 때문에 python 구문을 이용하여
'Q7' 이 붙어있는 컬럼 불러오기.

<br>

languages_cols = [col for col in df if col.startswith(‘Q7’)]

>>col 1부터 df 끝까지

Q7로 시작하는지 확인하여 **true** 일 때만 데이터 가져오기

</a>

```python
languages_cols = [col for col in df if col.startswith('Q7')]
```

<br><br>

#### 2. data Frame 만들어 주기 
algorithms 에 data frame을 씌워서 표를 만들고, 이름을 다음과같이 바꿔줌.

```python
languages = df[languages_cols]

languages.columns = ['Python', 'R', 'SQL', 'C', 'C++', 'Java', 
                     'Javascript', 'Julia', 'Swift', 'Bash',
                     'MATLAB', 'None', 'Other']
```

<br>

![languages.columns](/imeges/kgg/languages.columns.png)

<br>
<br>

##### 3.표 설정. 

```python
languages = (
    languages
    .count()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'Languages', 0:'Count'})
    .sort_values(by=['Count'], ascending=False)
    )
```

<br>

![languages](/imeges/kgg/languages.png)

<br>

 
  1. .count()  :coulumn 수 세기 
  2. .to_frame() : frame 생성
  3. .reset_index() : 원본과 상관없는 Index 생성
  4. .rename()
     1. columns의 이름을 지정 : 'index':'Languages', 0:'Count'
  5. .sort_values()
     1. by=['Count'], ascending=False
     2. Count 기준으로 내림차순으로 정렬


<br>
<br>

#### 3. percent 추가

```python
languages['percent'] = ((languages['Count'] / len(df))*100).round(2).astype(str) + '%'
```

<br>
<br>

표에 'percent'를 추가
algorithms의 count에 df의 length로 나누고 *100을 하는 전형적인 % 나타내기 <br>
값자체에 %를 입력하여 나중에 %를 추가 입력하지 않아도 됨  <br>
소숫점 자리 2까지 반영(반올림).
type 자체를 String으로 하여 추가 계산은 불가능. <br>


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
colors[7] = '#05568a'
colors[8] = '#05568a'
colors[9] = '#05568a'
```
<br>

<br>



#### 5. bar Graph 만들기

```python
fig = go.Figure(go.Bar(
            x=algorithms['Count'],
            y=algorithms['Algorithms'],
            text=algorithms['percent'],
            orientation='h',
            marker_color=colors
                        ))
```

horizontal과 vertical Graph의 차이는  x, y axis를 바꾸어 주는 것과  <br>
orientation='h' 을 넣어 주는 것의 차이.

![Leng_go.Bar](/imeges/kgg/Leng_go.Bar.png)

<hr>
<br>
<br>


#### 6. update_traces()

traces() 수정 :  Trace에 대한 설정

```python
fig.update_traces(texttemplate='%{text}', 
                  textposition='outside',
                  cliponaxis = False,
                  hovertemplate='<b>Lenguage</b>: %{y}<br><extra></extra>'+
                                '<b>Count</b>: %{x}',
                  textfont_size=12)
```

  1. texttemplate : text type
  2. textposition : 'outside' _ 설정 해 주지 않은 경우 칸에 따라 적당히 들어감.
  3. cliponaxis = False : text가 칸이 작아서 짤리는 경우를 막아주는 기능 (off)
  4. hovertemplate :
     1. 마우스 On하면 (커서를 위에 대면) 나오는 Hovert에대한 설정.
  5. textfont_size : 폰트 size

<br><br>

#### 7. Design

```python

fig.update_xaxes(showgrid=False)
fig.update_yaxes(showgrid=False)

fig.update_layout(showlegend=False, 
                  plot_bgcolor='#F7F7F7', 
                  margin=dict(pad=20),
                  paper_bgcolor='#F7F7F7',
                  height=700,
                  xaxis={'showticklabels': False},
                  yaxis_title=None,
                  xaxis_title=None,
                  yaxis={'categoryorder':'total ascending'},
                  title_text="Most Commonly Used <b>Programming Languages</b>",
                  title_x=0.5,
                  font=dict(family="Hiragino Kaku Gothic Pro, sans-serif", size=17, color='#000000'),
                  title_font_size=35)
```

1) Grid Delete

<br>

2) update_layout
   1) showlegend=False,
   2) plot_bgcolor='#F7F7F7'
   3) margin=dict(pad=20),
      1) ![padding20](/imeges/kgg/padding20.png)
   4) paper_bgcolor='#F7F7F7',
   5) xaxis={'showticklabels': False},
      1) x 축 labels을 삭제.
   6) yaxis_title=None,
   7) xaxis_title=None, yaxis={'categoryorder':'total ascending'},
      1) y 축 title을 categoryorder : 정렬
   8) title_text="Most Commonly Used <b>Algorithms</b>",
   9) title_x=0.5,
   10) font=dict(family="Hiragino Kaku Gothic Pro, sans-serif", size=15, color='#000000'),
                  title_font_size=35)

       
<br>
<br>

#### 8. Annotation

```python
fig.add_annotation(dict(font=dict(size=14),
                                    x=0.98,
                                    y=-0.13,
                                    showarrow=False,
                                    text="@miguelfzzz",
                                    xanchor='left',
                                    xref="paper",
                                    yref="paper"))

fig.add_annotation(dict(font=dict(size=12),
                                    x=0,
                                    y=-0.13,
                                    showarrow=False,
                                    text="Source: 2021 Kaggle Machine Learning & Data Science Survey",
                                    xanchor='left',
                                    xref="paper",
                                    yref="paper"))

fig.show()
```


![Leng_Final_HB](/imeges/kgg/Leng_Final_HB.png)


