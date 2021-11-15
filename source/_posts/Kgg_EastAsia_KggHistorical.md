---
title: "Subplots in python (kaggle in East-Asia)"
date: 2021-11-15 12:10:27
categories:
- python
    - plotly
tags:
- kaggle
- Plotly
- plot
- Subplots
---

# World Vs East Asia

<br><br>
<hr>




python을 이용한 plotly Library로 plot 그리기

[subplots](https://plotly.com/python/subplots/) 를 이용하여 다중 그래프를 그려 보자.


```python
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pylab as plt

import plotly.io as pio
import plotly.express as px
import plotly.graph_objects as go
import plotly.figure_factory as ff
from plotly.subplots import make_subplots
from plotly.offline import init_notebook_mode, iplot
init_notebook_mode(connected=True)
pio.templates.default = "none"
# import plotly.offline as py
# py.offline.init_notebook_mode()

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

import warnings
warnings.filterwarnings("ignore")
```


```python
df17= pd.read_csv("/kaggle/input/kaggle-survey-2017/multipleChoiceResponses.csv", encoding="ISO-8859-1")
df18= pd.read_csv("/kaggle/input/kaggle-survey-2018/multipleChoiceResponses.csv", )
df19= pd.read_csv("/kaggle/input/kaggle-survey-2019/multiple_choice_responses.csv", )
df20= pd.read_csv("/kaggle/input/kaggle-survey-2020/kaggle_survey_2020_responses.csv", )
df21= pd.read_csv("/kaggle/input/kaggle-survey-2021/kaggle_survey_2021_responses.csv", )

```


```python
total17 = (
    df17['region']
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'type', 'region':'respodents'})
    .groupby('type')
    .sum()
    .reset_index() 
)

total18 = (
    df18['region']
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'type', 'region':'respodents'})
    .groupby('type')
    .sum()
    .reset_index() 
)

total19 = (
    df19['region']
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'type', 'region':'respodents'})
    .groupby('type')
    .sum()
    .reset_index() 
)

total20 = (
    df20['region']
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'type', 'region':'respodents'})
    .groupby('type')
    .sum()
    .reset_index() 
)

total21 = (
    df21['region']
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'type', 'region':'respodents'})
    .groupby('type')
    .sum()
    .reset_index() 
)
```


```python
# Create subplots: use 'domain' type for Pie subplot
fig = make_subplots(rows=1, cols=5, specs=[[{'type':'domain'}, {'type':'domain'}, {'type':'domain'}, {'type':'domain'}, {'type':'domain'}]],
                   subplot_titles=("2017", "2018", "2019", "2020", "2021"))
fig.add_trace(go.Pie(labels=total21['type'], values=total21['respodents'], name="2021", scalegroup='one'),
              1, 1)
fig.add_trace(go.Pie(labels=total20['type'], values=total20['respodents'], name="2020", scalegroup='one'),
              1, 2)
fig.add_trace(go.Pie(labels=total19['type'], values=total19['respodents'], name="2019", scalegroup='one'),
              1, 3)
fig.add_trace(go.Pie(labels=total18['type'], values=total18['respodents'], name="2018", scalegroup='one'),
              1, 4)
fig.add_trace(go.Pie(labels=total17['type'], values=total17['respodents'], name="2017", scalegroup='one'),
              1, 5)

# Use `hole` to create a donut-like pie chart
fig.update_traces(hole=.2, hoverinfo="label+percent+name")
fig.update_layout(
    title_text="<b>World vs EastAsia</b>",
    # Add annotations in the center of the donut pies.
   )
fig.show()
```


![다중 파이 그래프](/imeges/kgg/Kgg_Multi_piePlot.png)




