---
title: "kaggle HeatMap"
date: 2021-11-16 15:10:27
categories:
- python
    - plotly
tags:
- kaggle
- Plotly
- plot
- kaggle_dictation
---

## HeatMap
### python문법의 plotly Library를 이용하여 Heatmap을 알아보자

+ HeatMap의 Gradation 색을 바꾸고 싶다면, [colorscales](https://plotly.com/python/builtin-colorscales/) 을 참고 해 보자. 
+ 

### HeatMap 1

```python
import plotly.figure_factory as ff

z=[[1, 90, 30, 50, 1], [20, 1, 60, 80, 30], [30, 60, 1, 50, 20]]
x=['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
y=['Morning', 'Afternoon', 'Evening']

fig = ff.create_annotated_heatmap(z, x = x, y = y, colorscale = "Viridis")
fig.show()
```


Input data를 맞춰 줘야한다. 

- x, y = List
- z= 배열

![heatmap](/imeges/kgg/heatmap.png)


### HeatMap 2

```python
import plotly.graph_objects as go
from functools import reduce
from itertools import product

z=[[1, 90, 30, 50, 1], [20, 1, 60, 80, 30], [30, 60, 1, 50, 20]]
x=['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
y=['Morning', 'Afternoon', 'Evening']

def get_anno_text(z_value):
    annotations=[]
    a, b = len(z_value), len(z_value[0])
    flat_z = reduce(lambda x,y: x+y, z_value) # z_value.flat if you deal with numpy
    coords = product(range(a), range(b))
    for pos, elem in zip(coords, flat_z):
        annotations.append({'font': {'color': '#FFFFFF'},
                    'showarrow': False,
                    'text': str(elem),
                    'x': pos[1],
                    'y': pos[0]})
    return annotations

fig = go.Figure(data=go.Heatmap(
                   z=z,
                   x=x,
                   y=y,
                   hoverongaps = True))

fig.update_layout(annotations = get_anno_text(z))
fig.show()
```


![HeatMap2](/imeges/kgg/HeatMap2.png)


### HeatMap 3

```python
import plotly.graph_objects as go

z = df.groupby(['Q4', 'Q1']).size().unstack().fillna(0).astype('int16')
# convert to correlation matrix
z2 = z.apply(lambda x:x/x.sum(), axis = 1)

x = z2.columns
y = z2.index

fig = go.Figure(data=go.Heatmap(
                   z=z2.to_numpy(),  #dataframe을 넘파이(배열)로 바꿔줌: List형태
                   x=x,
                   y=y,
                   type="heatmap",
                   colorscale = "Viridis",
                   hoverongaps = False))

fig.update_layout(
    title='Degree ~ Gender',
    xaxis_nticks=36)

fig.show()

```

![HeatMap3](/imeges/kgg/HeatMap3.png)

- z2 = z.apply(lambda x:x/x.sum(), axis = 1)
- 여기서 이 한줄로 인해 dataFrame이 Heatmap에 들어 갈 수 있는 상관관계G를 만들 수 있는 상태로 변환.
- data 자료형을 맞춰줘야 한다. (x, y, z)




### HeatMap Ref.
[Ref.](https://www.kaggle.com/j2hoon85/plotly-tutorial-for-kaggle-survey-competitions)