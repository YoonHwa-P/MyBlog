---
title: "kaggle HeatMap"
date: 2021-11-18 18:10:27
categories:
- python
    - plotly
tags:
- kaggle
- Plotly
- plot
- kaggle_Competition
---



# Kaggle 을 쓰는 East Asia의 사람들

- Kaggle 정의
- Kaggle 설문조사_개요
- 그런데 우리는 EA에살아서 궁긍한 걸 찾아보자.



## data import


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


<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-2.4.2.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>



    /kaggle/input/kaggle-survey-2018/SurveySchema.csv
    /kaggle/input/kaggle-survey-2018/freeFormResponses.csv
    /kaggle/input/kaggle-survey-2018/multipleChoiceResponses.csv
    /kaggle/input/kaggle-survey-2020/kaggle_survey_2020_responses.csv
    /kaggle/input/kaggle-survey-2020/supplementary_data/kaggle_survey_2020_methodology.pdf
    /kaggle/input/kaggle-survey-2020/supplementary_data/kaggle_survey_2020_answer_choices.pdf
    /kaggle/input/kaggle-survey-2021/kaggle_survey_2021_responses.csv
    /kaggle/input/kaggle-survey-2021/supplementary_data/kaggle_survey_2021_methodology.pdf
    /kaggle/input/kaggle-survey-2021/supplementary_data/kaggle_survey_2021_answer_choices.pdf
    /kaggle/input/kaggle-survey-2019/survey_schema.csv
    /kaggle/input/kaggle-survey-2019/multiple_choice_responses.csv
    /kaggle/input/kaggle-survey-2019/other_text_responses.csv
    /kaggle/input/kaggle-survey-2019/questions_only.csv
    /kaggle/input/kaggle-survey-2017/freeformResponses.csv
    /kaggle/input/kaggle-survey-2017/schema.csv
    /kaggle/input/kaggle-survey-2017/RespondentTypeREADME.txt
    /kaggle/input/kaggle-survey-2017/multipleChoiceResponses.csv
    /kaggle/input/kaggle-survey-2017/conversionRates.csv
    


```python
df17= pd.read_csv("/kaggle/input/kaggle-survey-2017/multipleChoiceResponses.csv", encoding="ISO-8859-1")
df18= pd.read_csv("/kaggle/input/kaggle-survey-2018/multipleChoiceResponses.csv", )
df19= pd.read_csv("/kaggle/input/kaggle-survey-2019/multiple_choice_responses.csv", )
df20= pd.read_csv("/kaggle/input/kaggle-survey-2020/kaggle_survey_2020_responses.csv", )
df21= pd.read_csv("/kaggle/input/kaggle-survey-2021/kaggle_survey_2021_responses.csv", )

```

## EastAsia VS World
### 1. data 전처리
### 1.1 EastAsia / World 나누기


```python
#질문 제거하기, replace
df17= df17.iloc[1:, :].replace("People 's Republic of China",'China')
df18= df18.iloc[1:, :].replace('Republic of Korea','South Korea')
df19= df19.iloc[1:, :].replace('Republic of Korea','South Korea')
df20= df20.iloc[1:, :].replace('Republic of Korea','South Korea')
df21= df21.iloc[1:, :]


## East Asia에는 대한민국, 일본, 중국, 타이완, 몽골, 북조선 총 6개의 국가가 속해 있다. 
## 알 수 없지만, 18년도엔 타이완이 없다. 

EastAsia17 = ['China',"People 's Republic of China", 'Taiwan', 'South Korea', 'Japan']
EastAsia18= ['China', 'South Korea', 'Japan', 'Republic of Korea'] 
EastAsia19 = ['China','Taiwan', 'South Korea', 'Japan', 'Republic of Korea']
EastAsia20 = ['China','Taiwan', 'South Korea','Republic of Korea', 'Japan']
EastAsia21 = ['China','Taiwan', 'South Korea', 'Japan']

EastAsia = ['Republic of Korea','China','Taiwan', 'South Korea', 'Japan', "People 's Republic of China" ]

years = ['2017', '2018', '2019', '2020', '2021']

#East Asia 뽑기
df21_Ea = df21[df21['Q3'].isin(EastAsia)]
df21_Wo = df21[~df21['Q3'].isin(EastAsia)]
df21['region']=["EastAsia" if x in EastAsia else "World" for x in df21['Q3']]


df20_Ea = df20[df20['Q3'].isin(EastAsia)]
df20_Wo = df20[~df20['Q3'].isin(EastAsia)]
df20['region']=["EastAsia" if x in EastAsia else "World" for x in df20['Q3']]

df19_Ea = df19[df19['Q3'].isin(EastAsia)]
df19_Wo = df19[~df19['Q3'].isin(EastAsia)]
df19['region']=["EastAsia" if x in EastAsia else "World" for x in df19['Q3']]

df18_Ea = df18[df18['Q3'].isin(EastAsia)]
df18_Wo = df18[~df18['Q3'].isin(EastAsia)]
df18['region']=["EastAsia" if x in EastAsia else "World" for x in df18['Q3']]

df17_Ea = df17[df17['Country'].isin(EastAsia)]
df17_Wo = df17[~df17['Country'].isin(EastAsia)]
df17['region']=["EastAsia" if x in EastAsia else "World" for x in df17['Country']]

#df21['region'].to_frame().value_counts().to_frame().rename(columns={'region': '21y', '' : 'count'})
print('OK')
```

    OK
    


```python
# 나라별 data 뽑기
df21_Ch= df21_Ea[df21_Ea['Q3'] == 'China']
df21_Tw= df21_Ea[df21_Ea['Q3'] == 'South Korea']
df21_Ko= df21_Ea[df21_Ea['Q3'] == 'Taiwan']
df21_Jp= df21_Ea[df21_Ea['Q3'] == 'Japan']

df20_Ch= df20_Ea[df20_Ea['Q3'] == 'China']
df20_Tw= df20_Ea[df20_Ea['Q3'] == 'South Korea']
df20_Ko= df20_Ea[df20_Ea['Q3'] == 'Taiwan']
df20_Jp= df20_Ea[df20_Ea['Q3'] == 'Japan']

df19_Ch= df19_Ea[df19_Ea['Q3'] == 'China']
df19_Tw= df19_Ea[df19_Ea['Q3'] == 'South Korea']
df19_Ko= df19_Ea[df19_Ea['Q3'] == 'Taiwan']
df19_Jp= df19_Ea[df19_Ea['Q3'] == 'Japan']

df18_Ch= df18_Ea[df18_Ea['Q3'] == 'China']
df18_Tw= df18_Ea[df18_Ea['Q3'] == 'South Korea']
df18_Jp= df18_Ea[df18_Ea['Q3'] == 'Japan']

df17_Ch= df17_Ea[df17_Ea['Country'] == 'China']
df17_Tw= df17_Ea[df17_Ea['Country'] == 'South Korea']
df17_Ko= df17_Ea[df17_Ea['Country'] == 'Taiwan']
df17_Jp= df17_Ea[df17_Ea['Country'] == 'Japan']
```

### 2. Kaggle 사용자수 (W/Ea)
### 2.1 data 전처리
### 2.2 그래프 그리기 


```python
def world_map(locations,counts,title):
    data = [ dict(
            type = 'choropleth',
            locations = locations,
            z = counts,
            colorscale = 'Blues',
            locationmode = 'country names',
            autocolorscale = False,
            reversescale = True,
            marker = dict(
                line = dict(color = '#F7F7F7', width = 1.5)),
                colorbar = dict(autotick = True, legth = 3, len=0.75, title = 'respodents',
                               max = 1000, min = 0)
                )
           ]
    layout = dict(
        title = title,
        titlefont={'size': 28, 'family': 'san serif'},
        width=750, 
        height=475,
        paper_bgcolor='#F7F7F7',
        geo = dict(
            showframe = True,
            showcoastlines = True,
            fitbounds="locations",
            )
    )
    
    fig = dict(data=data, layout=layout)
    iplot(fig, validate=False, filename='world-map')
    
z = df21_Ea['Q3'].value_counts()
 
## 메서드 호출
world_map(locations=z.index, counts=z.values, title= '<b> EastAsia Countries (2021 survey) <b>')
```


<div>                            <div id="a69f534a-954d-4141-815a-47392b896824" class="plotly-graph-div" style="height:475px; width:750px;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("a69f534a-954d-4141-815a-47392b896824")) {                    Plotly.newPlot(                        "a69f534a-954d-4141-815a-47392b896824",                        [{"autocolorscale":false,"colorbar":{"autotick":true,"legth":3,"len":0.75,"max":1000,"min":0,"title":"respodents"},"colorscale":"Blues","locationmode":"country names","locations":["Japan","China","South Korea","Taiwan"],"marker":{"line":{"color":"#F7F7F7","width":1.5}},"reversescale":true,"type":"choropleth","z":[921,814,359,334]}],                        {"geo":{"fitbounds":"locations","showcoastlines":true,"showframe":true},"height":475,"paper_bgcolor":"#F7F7F7","title":"<b> EastAsia Countries (2021 survey) <b>","titlefont":{"family":"san serif","size":28},"width":750},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('a69f534a-954d-4141-815a-47392b896824');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
# 수치 bar g = 사용자 수 비교.

Ea21 = len(df21_Ea)
Wo21 = len(df21) - len(df21_Ea)

Ea20 = len(df20_Ea)
Wo20 = len(df20) - len(df20_Ea)

Ea19 = len(df19_Ea)
Wo19 = len(df19) - len(df19_Ea)

Ea18 = len(df18_Ea)
Wo18 = len(df18) -  len(df18_Ea)

Ea17 = len(df17_Ea)
Wo17 = len(df17) - len(df17_Ea)

years = ['2017','2018','2019','2020', '2021']

def percent (a, b):
    result =a/(a+b)*100
    result = np.round(result)
    return result

def percentR (b, a):
    result =a/(a+b)*100
    result = np.round(result)
    return result

percent = [percent(Ea17, Wo17), percent(Ea18, Wo18), percent(Ea19, Wo19), 
                                                 percent(Ea20, Wo20), percent(Ea21, Wo21)]

# percentR = [percentR(Ea17, Wo17), percentR(Ea18, Wo18), percentR(Ea19, Wo19), 
#                                                  percentR(Ea20, Wo20), percentR(Ea21, Wo21)]
fig = go.Figure()
fig.add_trace(go.Bar(x=years, y=[len(df17), len(df18), len(df19), len(df20), len(df21)],
                base=[-len(df17), -len(df18), -len(df19), -len(df20), -len(df21)],
                marker_color='lightslategrey',
                name='World',
                textposition='outside',
                hovertemplate='<b>KaggleUser</b>: %{x}<br>'+
                                '<b>Count</b>: %{y}'
                
                ))
fig.add_trace(go.Bar(x=years, y=[Ea17, Ea18, Ea19, Ea20, Ea21],
                base=0,
                marker_color='pink',
                name='East Asia',
                text= percent,
                texttemplate='%{text}  %', 
                textposition='outside',
                hovertemplate='<b>KaggleUser</b>: %{x}<br>'+
                                '<b>Count</b>: %{y}',
                textfont_size=12                
                ))
fig.update_layout(width=600, height=700)
fig.show()
```


<div>                            <div id="e350533a-af9e-4687-8b6d-4e7b30cc5fc8" class="plotly-graph-div" style="height:700px; width:600px;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("e350533a-af9e-4687-8b6d-4e7b30cc5fc8")) {                    Plotly.newPlot(                        "e350533a-af9e-4687-8b6d-4e7b30cc5fc8",                        [{"base":[-16715,-23859,-19717,-20036,-25973],"hovertemplate":"<b>KaggleUser</b>: %{x}<br><b>Count</b>: %{y}","marker":{"color":"lightslategrey"},"name":"World","textposition":"outside","type":"bar","x":["2017","2018","2019","2020","2021"],"y":[16715,23859,19717,20036,25973]},{"base":0,"hovertemplate":"<b>KaggleUser</b>: %{x}<br><b>Count</b>: %{y}","marker":{"color":"pink"},"name":"East Asia","text":["7.0","10.0","9.0","8.0","9.0"],"textfont":{"size":12},"textposition":"outside","texttemplate":"%{text}  %","type":"bar","x":["2017","2018","2019","2020","2021"],"y":[1196,2500,1803,1645,2428]}],                        {"height":700,"template":{"data":{"scatter":[{"type":"scatter"}]}},"width":600},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('e350533a-af9e-4687-8b6d-4e7b30cc5fc8');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
from plotly.subplots import make_subplots
import plotly.graph_objects as go

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


<div>                            <div id="53077062-a4fd-4d0e-bd9e-b868ae7ca445" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("53077062-a4fd-4d0e-bd9e-b868ae7ca445")) {                    Plotly.newPlot(                        "53077062-a4fd-4d0e-bd9e-b868ae7ca445",                        [{"domain":{"x":[0.0,0.16799999999999998],"y":[0.0,1.0]},"hole":0.2,"hoverinfo":"label+percent+name","labels":["EastAsia","World"],"name":"2021","scalegroup":"one","type":"pie","values":[2428,23545]},{"domain":{"x":[0.208,0.376],"y":[0.0,1.0]},"hole":0.2,"hoverinfo":"label+percent+name","labels":["EastAsia","World"],"name":"2020","scalegroup":"one","type":"pie","values":[1645,18391]},{"domain":{"x":[0.416,0.584],"y":[0.0,1.0]},"hole":0.2,"hoverinfo":"label+percent+name","labels":["EastAsia","World"],"name":"2019","scalegroup":"one","type":"pie","values":[1803,17914]},{"domain":{"x":[0.624,0.792],"y":[0.0,1.0]},"hole":0.2,"hoverinfo":"label+percent+name","labels":["EastAsia","World"],"name":"2018","scalegroup":"one","type":"pie","values":[2500,21359]},{"domain":{"x":[0.832,1.0],"y":[0.0,1.0]},"hole":0.2,"hoverinfo":"label+percent+name","labels":["EastAsia","World"],"name":"2017","scalegroup":"one","type":"pie","values":[1196,15519]}],                        {"annotations":[{"font":{"size":16},"showarrow":false,"text":"2017","x":0.08399999999999999,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"2018","x":0.292,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"2019","x":0.5,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"2020","x":0.708,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"2021","x":0.9159999999999999,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"}],"template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"<b>World vs EastAsia</b>"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('53077062-a4fd-4d0e-bd9e-b868ae7ca445');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


가설은 시간에따라서 응답자수가 증가 할 줄 알았는데 결과를 보니 오히려 감소하는 경향을 볼 수 있다. 

East Asia 응답자는 2020년도(10.5%)에 가장 많았고 2121년도(7.15%)로 가장 적엇다. 

### 0. Kaggle Gender (in Ea)
### 0.1 data 전처리
### 0.2 그래프 그리기 


```python


df21_Ea=df21[df21['Q3'].isin(EastAsia21)]
Ea21= (
    df21_Ea['Q3'].value_counts().to_frame()
    .reset_index().rename(columns={'index':'Country', 'Q3':'21'}))


df20_Ea=df20[df20['Q3'].isin(EastAsia)]
Ea20= (
    df20_Ea['Q3'].replace('Republic of Korea','South Korea')
    .value_counts().to_frame().reset_index()
    .rename(columns={'index':'Country', 'Q3':'20'}))


df19_Ea=df19[df19['Q3'].isin(EastAsia)]
Ea19= (df19_Ea['Q3'].replace('Republic of Korea','South Korea')
       .value_counts().to_frame().reset_index()
       .rename(columns={'index':'Country', 'Q3':'19'}))


df18_Ea=df18[df18['Q3'].isin(EastAsia)]
Ea18= (df18_Ea['Q3'].replace('Republic of Korea','South Korea')
       .value_counts().to_frame().reset_index()
       .rename(columns={'index':'Country', 'Q3':'18'}))
Ea18.value_counts()
#df18 열에 taiwan = 0을 추가 해야 합니다. 


df17_Ea = df17[df17['Country'].isin(EastAsia)]
Ea17= (df17_Ea['Country'].replace("People 's Republic of China",'China')
       .value_counts().to_frame().reset_index()
       .rename(columns={'index':'Country', 'Country':'17'}))

#data를 합쳐서 하나의 dataframe으로 만들어 줌.

df5years = pd.merge(Ea17, Ea18, on='Country', how='outer')
df5year =pd.merge(Ea19,Ea20, on='Country', how='outer')
df5year=pd.merge(df5year, Ea21, on='Country', how='outer')

df5years = pd.merge(df5years, df5year, on='Country', how='outer')





fig = go.Figure(data=[
    go.Bar(name='2017', x=df5years['Country'], y=df5years['17']),
    go.Bar(name='2018', x=df5years['Country'], y=df5years['18']),
    go.Bar(name='2019', x=df5years['Country'], y=df5years['19']),
    go.Bar(name='2020', x=df5years['Country'], y=df5years['20']),
    go.Bar(name='2021', x=df5years['Country'], y=df5years['21'])
])

#Change the bar mode
fig.update_layout(barmode='stack', title='연도별 동아시아 Kaggle 사용자수'
                  )
fig.show()



# Text : percent

```


<div>                            <div id="5f1fa333-b3c1-45f7-affc-367ba7e0ec7e" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("5f1fa333-b3c1-45f7-affc-367ba7e0ec7e")) {                    Plotly.newPlot(                        "5f1fa333-b3c1-45f7-affc-367ba7e0ec7e",                        [{"name":"2017","type":"bar","x":["China","Japan","Taiwan","South Korea"],"y":[471,277,254,194]},{"name":"2018","type":"bar","x":["China","Japan","Taiwan","South Korea"],"y":[1644.0,597.0,null,259.0]},{"name":"2019","type":"bar","x":["China","Japan","Taiwan","South Korea"],"y":[574,673,301,255]},{"name":"2020","type":"bar","x":["China","Japan","Taiwan","South Korea"],"y":[474,638,267,266]},{"name":"2021","type":"bar","x":["China","Japan","Taiwan","South Korea"],"y":[814,921,334,359]}],                        {"barmode":"stack","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"연도별 동아시아 Kaggle 사용자수"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('5f1fa333-b3c1-45f7-affc-367ba7e0ec7e');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


5년간 china의 Kaggle 응답자 수가 가장 많았다. taiwan이 가장 적지만, 2018년도에 어떤이유인지 모르겠지만, china로 결과 값이 흡수 된 것 같다. 


south korea나 Taiwan의 응답자수는 china의 절반에 미치지 못한다.

+ 인구수 대비 몇 % 가 많다. 



### 3. Kaggle Gender (W/Ea)
### 3.1 data 전처리
### 3.2 그래프 그리기 


```python
Gender_17 = (
    df17['GenderSelect']
    .replace(['A different identity', 'Prefer to self-describe', 'Non-binary, genderqueer, or gender non-conforming'], 'Others')
    .fillna('Others')
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'type', 'GenderSelect':'Gender'})
    .groupby('type')
    .sum()
    .reset_index()
)
Gender_18 = (
    df18['Q1']
    .replace(['Prefer not to say', 'Prefer to self-describe'], 'Others')
    .fillna('Others')
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'type', 'Q1':'Gender'})
    .groupby('type')
    .sum()
    .reset_index()
)
Gender_19 = (
    df19['Q2']
    .replace(['Prefer not to say','Prefer to self-describe'],'Others')
    .fillna('Others')
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'type', 'Q2':'Gender'})
    .groupby('type')
    .sum()
    .reset_index()
)
Gender_20 = (
    df20['Q2']
    .replace(['Prefer not to say', 'Prefer to self-describe', 'Nonbinary'], 'Others')
    .replace(['Man', 'Woman'], ['Male', 'Female'])
    .fillna('Others')
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'type', 'Q2':'Gender'})
    .groupby('type')
    .sum()
    .reset_index()
)
Gender_21 = (
    df21['Q2']
    .replace(['Prefer not to say', 'Prefer to self-describe', 'Nonbinary'], 'Others')
    .replace(['Man', 'Woman'], ['Male', 'Female'])
    .fillna('Others')
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'type', 'Q2':'Gender'})
    .groupby('type')
    .sum()
    .reset_index()
)

# Create subplots: use 'domain' type for Pie subplot
fig = make_subplots(rows=1, cols=5, specs=[[{'type':'domain'}, {'type':'domain'}, {'type':'domain'}, {'type':'domain'}, {'type':'domain'}]],
                   subplot_titles=("2017", "2018", "2019", "2020", "2021"))
fig.add_trace(go.Pie(labels=Gender_21['type'], values=Gender_21['Gender'], name="2021", scalegroup='one'),
              1, 5)
fig.add_trace(go.Pie(labels=Gender_20['type'], values=Gender_20['Gender'], name="2020", scalegroup='one'),
              1, 4)
fig.add_trace(go.Pie(labels=Gender_19['type'], values=Gender_19['Gender'], name="2019", scalegroup='one'),
              1, 3)
fig.add_trace(go.Pie(labels=Gender_18['type'], values=Gender_18['Gender'], name="2018", scalegroup='one'),
              1, 2)
fig.add_trace(go.Pie(labels=Gender_17['type'], values=Gender_17['Gender'], name="2017", scalegroup='one'),
              1, 1)

# Use `hole` to create a donut-like pie chart
fig.update_traces(hole=.2, hoverinfo="label+percent+name")
fig.update_layout(
    title_text="<b>World_Gender</b>",
    # Add annotations in the center of the donut pies.
   )
fig.show()

```


<div>                            <div id="464f6d7b-6f36-45ea-ba9e-00c1272bb9c2" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("464f6d7b-6f36-45ea-ba9e-00c1272bb9c2")) {                    Plotly.newPlot(                        "464f6d7b-6f36-45ea-ba9e-00c1272bb9c2",                        [{"domain":{"x":[0.832,1.0],"y":[0.0,1.0]},"hole":0.2,"hoverinfo":"label+percent+name","labels":["Female","Male","Others"],"name":"2021","scalegroup":"one","type":"pie","values":[4890,20598,485]},{"domain":{"x":[0.624,0.792],"y":[0.0,1.0]},"hole":0.2,"hoverinfo":"label+percent+name","labels":["Female","Male","Others"],"name":"2020","scalegroup":"one","type":"pie","values":[3878,15789,369]},{"domain":{"x":[0.416,0.584],"y":[0.0,1.0]},"hole":0.2,"hoverinfo":"label+percent+name","labels":["Female","Male","Others"],"name":"2019","scalegroup":"one","type":"pie","values":[3212,16138,367]},{"domain":{"x":[0.208,0.376],"y":[0.0,1.0]},"hole":0.2,"hoverinfo":"label+percent+name","labels":["Female","Male","Others"],"name":"2018","scalegroup":"one","type":"pie","values":[4010,19430,419]},{"domain":{"x":[0.0,0.16799999999999998],"y":[0.0,1.0]},"hole":0.2,"hoverinfo":"label+percent+name","labels":["Female","Male","Others"],"name":"2017","scalegroup":"one","type":"pie","values":[2778,13610,327]}],                        {"annotations":[{"font":{"size":16},"showarrow":false,"text":"2017","x":0.08399999999999999,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"2018","x":0.292,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"2019","x":0.5,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"2020","x":0.708,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{"size":16},"showarrow":false,"text":"2021","x":0.9159999999999999,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"}],"template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"<b>World_Gender</b>"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('464f6d7b-6f36-45ea-ba9e-00c1272bb9c2');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


연도별, 지역별로 보았을때, 여성의 비율이 20% 미만으로 적은 것을 알 수 있다. 

2019년 이전보다 2020년 이후가 증가 했다. (16%-> 19%)


### 0. Kaggle job (W/Ea)
### 0.1 data 전처리
### 0.2 그래프 그리기 


```python
#data 확인
Data_Analyst =['Data Analyst','Data Miner,Information technology','Data Miner',
               'Predictive Modeler','Information technology, networking, or system administration', 
              'A business discipline (accounting, economics, finance, etc.)', 'Business Analyst', 'Humanities',
                'Statistician', 'Mathematics or statistics', 'Medical or life sciences (biology, chemistry, medicine, etc.)', 
                'Physics or astronomy', 'Research Scientist', 'Researcher', 'Social sciences (anthropology, psychology, sociology, etc.)', 
                'Humanities (history, literature, philosophy, etc.)']
Data_Scientist =['Data Scientist', 'Environmental science or geology', 
                'Machine Learning Engineer', 'Scientist/Researcher']
Developer=['Developer Relations/Advocacy','Data Engineer','Engineer','Engineering (non-computer focused)',
           'Programmer','Software Engineer', 'Computer Scientist','Computer science (software engineering, etc.)', 
           'Fine arts or performing arts','Product Manager', 'Software Developer/Software Engineer',
           'Product/Project Manager','Program/Project Manager','DBA/Database Engineer']
Not_Employeed =['Currently not employed', 'Not employed', 'Student']
Others = ['I never declared a major', 'Other']


#연도별로 뽑은 나라별 직업.
df21job_Ea = df21_Ea.loc[:,['Q3','Q5']].reset_index().rename(columns={'index':'job', 'Q5':'2021'}).fillna('Other')
df20job_Ea = df20_Ea.loc[:,['Q3','Q5']].reset_index().rename(columns={'index':'job', 'Q5':'2020'}).fillna('Other')
df19job_Ea = df19_Ea.loc[:,['Q3','Q5']].reset_index().rename(columns={'index':'job', 'Q5':'2019'}).fillna('Other')
df18job_Ea = df18_Ea.loc[:,['Q3','Q5']].reset_index().rename(columns={'index':'job', 'Q5':'2018'}).fillna('Other')
df17job_Ea = df17_Ea.loc[:,['Country','CurrentJobTitleSelect']].reset_index().rename(columns={'index':'job', 'CurrentJobTitleSelect':'2017'}).fillna('Other')

# 연도별 job Grouping in east asia

df21job_Ea.value_counts('2021')
df21job_Ea['JOB']=["Data Analyst" if x in Data_Analyst
                   else "Data Scientist" if x in Data_Scientist # Data Scientist
                   else "Data Engineer" if x in Developer
                    else "NotEmployeed" if x in Not_Employeed
                   else "Others" 
                   for x in df21job_Ea['2021']]
df21job_Ea.value_counts('JOB')

df20job_Ea.value_counts('2020')
df20job_Ea['JOB']=["Data Analyst" if x in Data_Analyst
                   else "Data Scientist" if x in Data_Scientist 
                   else "Data Engineer" if x in Developer
                    else "NotEmployeed" if x in Not_Employeed
                   else "Other"
                   for x in df20job_Ea['2020']]
df20job_Ea[['2020','JOB']]

df19job_Ea.value_counts('2019')
df19job_Ea['JOB']=["Data Analyst" if x in Data_Analyst
                   else "Data Scientist" if x in Data_Scientist 
                   else "Data Engineer" if x in Developer
                    else "NotEmployeed" if x in Not_Employeed
                    else "Other"
                   for x in df19job_Ea['2019']]


#2019 data에   "Other" grouping이 제대로 이루어 졌는지 확인 
df19jobTest = df19job_Ea.loc[df19job_Ea.JOB == 'Other']
df19jobTest['2019'].value_counts()


df18job_Ea.value_counts('2018')
df18job_Ea['JOB']=["Data Analyst" if x in Data_Analyst
                   else "Data Scientist" if x in Data_Scientist 
                   else "Data Engineer" if x in Developer
                    else "NotEmployeed" if x in Not_Employeed
                    else "Other"
                   for x in df18job_Ea['2018']]

df18jobTest = df18job_Ea.loc[df18job_Ea.JOB == 'Other']
df18jobTest['2018'].value_counts()


df17job_Ea.value_counts('2017')
df17job_Ea['JOB']=["Data Analyst" if x in Data_Analyst
                   else "Data Scientist" if x in Data_Scientist 
                   else "Data Engineer" if x in Developer
                    else "NotEmployeed" if x in Not_Employeed
                    else "Other"
                   for x in df17job_Ea['2017']]

df17jobTest = df17job_Ea.loc[df17job_Ea.JOB == 'Other']
df17jobTest['2017'].value_counts()


df21jobTest = df21job_Ea.loc[df21job_Ea.JOB == 'Other']
df21jobTest['2021'].head()
df21job_Ea.value_counts('JOB')

#data frame 정리
dfjob21 =df21job_Ea.groupby(['Q3','JOB']).size().reset_index().rename(columns = {0:"Count"}).rename(columns={'Q3':'country', 'JOB':'2021'})
dfjob20 =df20job_Ea.groupby(['Q3','JOB']).size().reset_index().rename(columns = {0:"Count"}).rename(columns={'Q3':'country', 'JOB':'2020'})
dfjob19 =df19job_Ea.groupby(['Q3','JOB']).size().reset_index().rename(columns = {0:"Count"}).rename(columns={'Q3':'country', 'JOB':'2019'})
dfjob18 =df18job_Ea.groupby(['Q3','JOB']).size().reset_index().rename(columns = {0:"Count"}).rename(columns={'Q3':'country', 'JOB':'2018'})
dfjob17 =df17job_Ea.groupby(['Country','JOB']).size().reset_index().rename(columns = {0:"Count"}).rename(columns={'Country':'country', 'JOB':'2017'})

```


```python
#21년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfjob21.groupby('country'):
   fig.add_trace(go.Bar(
       x = group['2021'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2021_나라별 직업 수',
                 width=700,
                 height=450
                 )
fig.show()

#20년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfjob20.groupby('country'):
   fig.add_trace(go.Bar(
       x = group['2020'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2020_나라별 직업 수',
                 width=700,
                 height=450)

fig.show()

#19년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfjob19.groupby('country'):
   fig.add_trace(go.Bar(
       x = group['2019'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2019_나라별 직업 수',
                 width=700,
                 height=450
                 )

fig.show()

#18년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfjob18.groupby('country'):
   fig.add_trace(go.Bar(
       x = group['2018'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2018_나라별 직업 수', width=700,
                 height=450
                 )
fig.show()


#17년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfjob17.groupby('country'):
   fig.add_trace(go.Bar(
       x = group['2017'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2017_나라별 직업 수',
                 width=700,
                 height=450
                 )
fig.show()
```


<div>                            <div id="da79c984-e07e-459a-aa47-b950bc5fd4d5" class="plotly-graph-div" style="height:450px; width:700px;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("da79c984-e07e-459a-aa47-b950bc5fd4d5")) {                    Plotly.newPlot(                        "da79c984-e07e-459a-aa47-b950bc5fd4d5",                        [{"name":"China","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Others"],"y":[145,113,136,408,12]},{"name":"Japan","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Others"],"y":[185,284,123,211,118]},{"name":"South Korea","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Others"],"y":[68,68,78,120,25]},{"name":"Taiwan","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Others"],"y":[72,70,57,116,19]}],                        {"barmode":"group","height":450,"plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2021_나라별 직업 수"},"width":700},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('da79c984-e07e-459a-aa47-b950bc5fd4d5');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="6b1e4072-1057-4bf3-8d6f-aa80ce2dc4db" class="plotly-graph-div" style="height:450px; width:700px;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("6b1e4072-1057-4bf3-8d6f-aa80ce2dc4db")) {                    Plotly.newPlot(                        "6b1e4072-1057-4bf3-8d6f-aa80ce2dc4db",                        [{"name":"China","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Other"],"y":[73,54,60,226,61]},{"name":"Japan","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Other"],"y":[107,157,111,130,133]},{"name":"South Korea","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Other"],"y":[53,37,49,84,43]},{"name":"Taiwan","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Other"],"y":[43,62,36,98,28]}],                        {"barmode":"group","height":450,"plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2020_나라별 직업 수"},"width":700},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('6b1e4072-1057-4bf3-8d6f-aa80ce2dc4db');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="832d6f83-55e4-4e0a-bab6-ee2e0cca7ded" class="plotly-graph-div" style="height:450px; width:700px;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("832d6f83-55e4-4e0a-bab6-ee2e0cca7ded")) {                    Plotly.newPlot(                        "832d6f83-55e4-4e0a-bab6-ee2e0cca7ded",                        [{"name":"China","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Other"],"y":[91,129,41,256,57]},{"name":"Japan","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Other"],"y":[131,204,125,93,120]},{"name":"South Korea","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Other"],"y":[59,69,44,55,28]},{"name":"Taiwan","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","NotEmployeed","Other"],"y":[56,90,44,86,25]}],                        {"barmode":"group","height":450,"plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2019_나라별 직업 수"},"width":700},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('832d6f83-55e4-4e0a-bab6-ee2e0cca7ded');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="2fffa68b-6c52-464b-9cfa-1e2295b606b3" class="plotly-graph-div" style="height:450px; width:700px;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("2fffa68b-6c52-464b-9cfa-1e2295b606b3")) {                    Plotly.newPlot(                        "2fffa68b-6c52-464b-9cfa-1e2295b606b3",                        [{"name":"China","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","Other"],"y":[509,948,34,153]},{"name":"Japan","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","Other"],"y":[265,254,9,69]},{"name":"South Korea","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","Other"],"y":[96,130,1,32]}],                        {"barmode":"group","height":450,"plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2018_나라별 직업 수"},"width":700},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('2fffa68b-6c52-464b-9cfa-1e2295b606b3');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="2b71b654-dbc1-4d99-a0a8-c87d4bba1492" class="plotly-graph-div" style="height:450px; width:700px;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("2b71b654-dbc1-4d99-a0a8-c87d4bba1492")) {                    Plotly.newPlot(                        "2b71b654-dbc1-4d99-a0a8-c87d4bba1492",                        [{"name":"China","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","Other"],"y":[83,72,84,232]},{"name":"Japan","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","Other"],"y":[50,85,64,78]},{"name":"South Korea","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","Other"],"y":[46,30,35,83]},{"name":"Taiwan","type":"bar","x":["Data Analyst","Data Engineer","Data Scientist","Other"],"y":[41,62,39,112]}],                        {"barmode":"group","height":450,"plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2017_나라별 직업 수"},"width":700},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('2b71b654-dbc1-4d99-a0a8-c87d4bba1492');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### 0. Kaggle age & Edu (W/Ea)
### 0.1 data 전처리
### 0.2 그래프 그리기 


```python
# 연도별 나이 
df21Age_Ea = df21_Ea.loc[:,['Q3','Q1']].reset_index().rename(columns={'Q3':'East_Asia', 'Q1':'2021'}).fillna('etc')
df20Age_Ea = df20_Ea.loc[:,['Q3','Q1']].reset_index().rename(columns={'Q3':'East_Asia', 'Q1':'2020'}).fillna('etc')
df19Age_Ea = df19_Ea.loc[:,['Q3','Q1']].reset_index().rename(columns={'Q3':'East_Asia', 'Q1':'2019'}).fillna('etc')
df18Age_Ea = df18_Ea.loc[:,['Q3','Q2']].reset_index().rename(columns={'Q3':'East_Asia', 'Q2':'2018'}).fillna('etc')
df17Age_Ea = df17_Ea.loc[:,['Country','Age']].reset_index().rename(columns={'Country':'East_Asia', 'Age':'2017'}).fillna('etc')

#data frame 정리
dfAge21 =df21Age_Ea.groupby(['East_Asia','2021']).size().reset_index().rename(columns = {0:"Count"})
dfAge20 =df20Age_Ea.groupby(['East_Asia','2020']).size().reset_index().rename(columns = {0:"Count"})
dfAge19 =df19Age_Ea.groupby(['East_Asia','2019']).size().reset_index().rename(columns = {0:"Count"})
dfAge18 =df18Age_Ea.groupby(['East_Asia','2018']).size().reset_index().rename(columns = {0:"Count"})
dfAge17 =(df17Age_Ea.groupby(['East_Asia','2017'])
          .size().reset_index().rename(columns = {0:"Count"}))

#2017data
# array([16.0, 18.0, 19.0, 20.0, 21.0, 22.0, 23.0, 24.0, 25.0, 26.0, 27.0,
#        28.0, 29.0, 30.0, 31.0, 32.0, 33.0, 34.0, 35.0, 36.0, 37.0, 38.0,
#        39.0, 40.0, 41.0, 42.0, 43.0, 44.0, 45.0, 47.0, 50.0, 54.0, 100.0,
#        'etc', 46.0, 48.0, 49.0, 51.0, 52.0, 53.0, 55.0, 57.0, 58.0, 59.0,
#        62.0, 64.0, 65.0, 67.0, 68.0, 70.0, 17.0, 56.0, 60.0], dtype=object)


#21년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfAge21.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2021'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2021_나라별 연령')
fig.show()

#20년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfAge20.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2020'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2020_나라별 연령')

fig.show()

#19년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfAge19.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2019'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2019_나라별 연령')

fig.show()


#18년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfAge18.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2018'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2018_나라별 연령')

fig.show()


#17년도 Bar graph 그리기_ Scatter 로 변경
fig = go.Figure()

for country, group  in dfAge17.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2017'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2017_나라별 연령')

fig.show()


```


<div>                            <div id="ed15ab1b-e373-4c00-8486-32fef3d7535a" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("ed15ab1b-e373-4c00-8486-32fef3d7535a")) {                    Plotly.newPlot(                        "ed15ab1b-e373-4c00-8486-32fef3d7535a",                        [{"name":"China","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","70+"],"y":[206,275,159,109,39,14,8,1,2,1]},{"name":"Japan","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70+"],"y":[64,93,169,140,99,95,92,61,50,49,9]},{"name":"South Korea","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69"],"y":[17,46,106,53,32,26,35,30,10,4]},{"name":"Taiwan","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69"],"y":[32,69,87,38,28,23,24,20,6,7]}],                        {"barmode":"group","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2021_나라별 연령"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('ed15ab1b-e373-4c00-8486-32fef3d7535a');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="4a28ccfe-fdf5-4489-864f-14e40c5e1877" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("4a28ccfe-fdf5-4489-864f-14e40c5e1877")) {                    Plotly.newPlot(                        "4a28ccfe-fdf5-4489-864f-14e40c5e1877",                        [{"name":"China","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","55-59"],"y":[113,159,116,56,14,11,2,3]},{"name":"Japan","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70+"],"y":[43,71,114,92,74,66,69,45,30,30,4]},{"name":"South Korea","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69"],"y":[19,35,74,41,29,27,21,10,6,4]},{"name":"Taiwan","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69"],"y":[30,59,62,28,27,24,12,15,7,3]}],                        {"barmode":"group","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2020_나라별 연령"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('4a28ccfe-fdf5-4489-864f-14e40c5e1877');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="dbceff57-9ac3-43fb-98a1-764cb3fb45d0" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("dbceff57-9ac3-43fb-98a1-764cb3fb45d0")) {                    Plotly.newPlot(                        "dbceff57-9ac3-43fb-98a1-764cb3fb45d0",                        [{"name":"China","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","70+"],"y":[93,201,161,63,28,18,5,4,1]},{"name":"Japan","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70+"],"y":[30,82,154,125,87,66,56,35,20,17,1]},{"name":"South Korea","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69"],"y":[12,23,76,39,29,27,32,10,5,2]},{"name":"Taiwan","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70+"],"y":[18,55,71,38,40,30,21,16,7,4,1]}],                        {"barmode":"group","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2019_나라별 연령"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('dbceff57-9ac3-43fb-98a1-764cb3fb45d0');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="3ff8513a-6fc7-49f5-b0c2-d2445d793b29" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("3ff8513a-6fc7-49f5-b0c2-d2445d793b29")) {                    Plotly.newPlot(                        "3ff8513a-6fc7-49f5-b0c2-d2445d793b29",                        [{"name":"China","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70-79","80+"],"y":[246,611,525,147,58,32,13,3,3,1,1,4]},{"name":"Japan","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70-79"],"y":[39,87,143,99,82,65,38,17,9,12,6]},{"name":"South Korea","type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69"],"y":[9,36,90,44,24,20,25,3,3,5]}],                        {"barmode":"group","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2018_나라별 연령"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('3ff8513a-6fc7-49f5-b0c2-d2445d793b29');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="eb4b6a56-fe64-48c7-96e7-c730ea4f66a5" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("eb4b6a56-fe64-48c7-96e7-c730ea4f66a5")) {                    Plotly.newPlot(                        "eb4b6a56-fe64-48c7-96e7-c730ea4f66a5",                        [{"name":"China","type":"bar","x":[16.0,18.0,19.0,20.0,21.0,22.0,23.0,24.0,25.0,26.0,27.0,28.0,29.0,30.0,31.0,32.0,33.0,34.0,35.0,36.0,37.0,38.0,39.0,40.0,41.0,42.0,43.0,44.0,45.0,47.0,50.0,54.0,100.0,"etc"],"y":[1,3,3,14,34,40,44,55,45,32,31,25,21,32,8,8,7,10,12,2,8,3,4,3,4,2,2,1,3,2,2,1,1,8]},{"name":"Japan","type":"bar","x":[19.0,20.0,21.0,22.0,23.0,24.0,25.0,26.0,27.0,28.0,29.0,30.0,31.0,32.0,33.0,34.0,35.0,36.0,37.0,38.0,39.0,40.0,41.0,42.0,43.0,44.0,45.0,46.0,47.0,48.0,49.0,50.0,51.0,52.0,53.0,54.0,55.0,57.0,58.0,59.0,62.0,64.0,65.0,67.0,68.0,70.0,"etc"],"y":[1,4,3,7,11,6,13,9,20,18,13,13,7,10,10,13,12,3,13,5,5,8,8,4,7,6,4,4,1,6,5,4,2,3,1,2,2,1,1,1,1,1,1,2,1,1,4]},{"name":"South Korea","type":"bar","x":[17.0,19.0,20.0,21.0,22.0,23.0,24.0,25.0,26.0,27.0,28.0,29.0,30.0,31.0,32.0,33.0,34.0,35.0,36.0,37.0,38.0,39.0,40.0,41.0,42.0,43.0,44.0,45.0,46.0,47.0,48.0,49.0,53.0,56.0,57.0,59.0,62.0,65.0,"etc"],"y":[1,2,1,3,3,7,9,19,20,15,6,11,9,7,7,2,5,6,4,1,9,3,5,4,4,4,3,4,5,1,3,2,1,1,1,1,1,1,3]},{"name":"Taiwan","type":"bar","x":[18.0,19.0,20.0,21.0,22.0,23.0,24.0,25.0,26.0,27.0,28.0,29.0,30.0,31.0,32.0,33.0,34.0,35.0,36.0,37.0,38.0,39.0,40.0,41.0,42.0,43.0,44.0,45.0,46.0,47.0,48.0,50.0,51.0,52.0,53.0,54.0,56.0,60.0],"y":[1,2,14,16,14,20,22,15,15,7,10,8,5,4,5,7,13,12,2,4,10,2,10,2,3,1,2,9,3,2,4,2,2,1,2,1,1,1]}],                        {"barmode":"group","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2017_나라별 연령"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('eb4b6a56-fe64-48c7-96e7-c730ea4f66a5');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### 0.2 Kaggle age (Ea)
### 0.1.1 data 전처리
### 0.2.1 그래프 그리기 


```python
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


# 연령별 나라 비율 in East Asia // 다중 pie 그래프로 바꾸기 
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


<div>                            <div id="78c8214c-2956-44e2-a035-7e2c1d299547" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("78c8214c-2956-44e2-a035-7e2c1d299547")) {                    Plotly.newPlot(                        "78c8214c-2956-44e2-a035-7e2c1d299547",                        [{"name":"China","text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%"],"type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","70+"],"y":[8.48,11.33,6.55,4.49,1.61,0.58,0.33,0.04,0.08,0.04]},{"name":"Japan","text":["2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%"],"type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70+"],"y":[2.64,3.83,6.96,5.77,4.08,3.91,3.79,2.51,2.06,2.02,0.37]},{"name":"South Korea","text":["0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%"],"type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69"],"y":[0.7,1.89,4.37,2.18,1.32,1.07,1.44,1.24,0.41,0.16]},{"name":"Taiwan","text":["1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69"],"y":[1.32,2.84,3.58,1.57,1.15,0.95,0.99,0.82,0.25,0.29]}],                        {"barmode":"stack","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2021_ 연령별 나라 비율 in East Asia"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('78c8214c-2956-44e2-a035-7e2c1d299547');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="e6b02bd9-8ef3-48d5-9751-958989ed3b27" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("e6b02bd9-8ef3-48d5-9751-958989ed3b27")) {                    Plotly.newPlot(                        "e6b02bd9-8ef3-48d5-9751-958989ed3b27",                        [{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["China","Japan","South Korea","Taiwan"],"y":[8.48,2.64,0.7,1.32]},{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["China","Japan","South Korea","Taiwan"],"y":[11.33,3.83,1.89,2.84]},{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["China","Japan","South Korea","Taiwan"],"y":[6.55,6.96,4.37,3.58]},{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["China","Japan","South Korea","Taiwan"],"y":[4.49,5.77,2.18,1.57]},{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["China","Japan","South Korea","Taiwan"],"y":[1.61,4.08,1.32,1.15]},{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["China","Japan","South Korea","Taiwan"],"y":[0.58,3.91,1.07,0.95]},{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["China","Japan","South Korea","Taiwan"],"y":[0.33,3.79,1.44,0.99]},{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["China","Japan","South Korea","Taiwan"],"y":[0.04,2.51,1.24,0.82]},{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["China","Japan","South Korea","Taiwan"],"y":[0.08,2.06,0.41,0.25]},{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["Japan","South Korea","Taiwan"],"y":[2.02,0.16,0.29]},{"text":["8.48%","11.33%","6.55%","4.49%","1.61%","0.58%","0.33%","0.04%","0.08%","0.04%","2.64%","3.83%","6.96%","5.77%","4.08%","3.91%","3.79%","2.51%","2.06%","2.02%","0.37%","0.7%","1.89%","4.37%","2.18%","1.32%","1.07%","1.44%","1.24%","0.41%","0.16%","1.32%","2.84%","3.58%","1.57%","1.15%","0.95%","0.99%","0.82%","0.25%","0.29%"],"type":"bar","x":["China","Japan"],"y":[0.04,0.37]}],                        {"barmode":"stack","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2021_ 나라별 연령 비율 in East Asia"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('e6b02bd9-8ef3-48d5-9751-958989ed3b27');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### 연도별 연령


```python
# 나라별로 연도가 나왓으면 좋겠어요 .

z = df21Age_Ea.groupby(['East_Asia', '2021']).size().unstack().fillna(0).astype('int64')
z_data = z.apply(lambda x:np.round(x/x.sum(), 2), axis = 1).to_numpy() # convert to correlation matrix
x = z.columns.tolist()
y = z.index.tolist()

fig21 = ff.create_annotated_heatmap(z_data, x = x, y = y, colorscale = "sunset")

z = df20Age_Ea.groupby(['East_Asia', '2020']).size().unstack().fillna(0).astype('int64')
z_data = z.apply(lambda x:np.round(x/x.sum(), 2), axis = 1).to_numpy() # convert to correlation matrix
x = z.columns.tolist()
y = z.index.tolist()

fig20 = ff.create_annotated_heatmap(z_data, x = x, y = y, colorscale = "sunset")


z = df19Age_Ea.groupby(['East_Asia', '2019']).size().unstack().fillna(0).astype('int64')
z_data = z.apply(lambda x:np.round(x/x.sum(), 2), axis = 1).to_numpy() # convert to correlation matrix
x = z.columns.tolist()
y = z.index.tolist()

fig19 = ff.create_annotated_heatmap(z_data, x = x, y = y, colorscale = "sunset")


z = df18Age_Ea.groupby(['East_Asia', '2018']).size().unstack().fillna(0).astype('int64')
z_data = z.apply(lambda x:np.round(x/x.sum(), 2), axis = 1).to_numpy() # convert to correlation matrix
x = z.columns.tolist()
y = z.index.tolist()

fig18 = ff.create_annotated_heatmap(z_data, x = x, y = y, colorscale = "sunset")


z = df17Age_Ea.groupby(['East_Asia', '2017']).size().unstack().fillna(0).astype('int64')
z_data = z.apply(lambda x:np.round(x/x.sum(), 2), axis = 1).to_numpy() # convert to correlation matrix
x = z.columns.tolist()
y = z.index.tolist()

fig17 = ff.create_annotated_heatmap(z_data, x = x, y = y, colorscale = "sunset")



fig21.show()
fig20.show()
fig19.show()
fig18.show()

```


<div>                            <div id="c0f0e5e2-f134-43f6-a5aa-a23571c2160a" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c0f0e5e2-f134-43f6-a5aa-a23571c2160a")) {                    Plotly.newPlot(                        "c0f0e5e2-f134-43f6-a5aa-a23571c2160a",                        [{"colorscale":[[0.0,"rgb(243, 231, 155)"],[0.16666666666666666,"rgb(250, 196, 132)"],[0.3333333333333333,"rgb(248, 160, 126)"],[0.5,"rgb(235, 127, 134)"],[0.6666666666666666,"rgb(206, 102, 147)"],[0.8333333333333334,"rgb(160, 89, 160)"],[1.0,"rgb(92, 83, 165)"]],"reversescale":false,"showscale":false,"type":"heatmap","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70+"],"y":["China","Japan","South Korea","Taiwan"],"z":[[0.25,0.34,0.2,0.13,0.05,0.02,0.01,0.0,0.0,0.0,0.0],[0.07,0.1,0.18,0.15,0.11,0.1,0.1,0.07,0.05,0.05,0.01],[0.05,0.13,0.3,0.15,0.09,0.07,0.1,0.08,0.03,0.01,0.0],[0.1,0.21,0.26,0.11,0.08,0.07,0.07,0.06,0.02,0.02,0.0]]}],                        {"annotations":[{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.25","x":"18-21","xref":"x","y":"China","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.34","x":"22-24","xref":"x","y":"China","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.2","x":"25-29","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.13","x":"30-34","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"35-39","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"40-44","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"45-49","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"50-54","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"55-59","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"60-69","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"18-21","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"22-24","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.18","x":"25-29","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.15","x":"30-34","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"35-39","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"40-44","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"45-49","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"50-54","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"55-59","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"60-69","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"70+","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"18-21","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.13","x":"22-24","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.3","x":"25-29","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.15","x":"30-34","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"35-39","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"40-44","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"45-49","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.08","x":"50-54","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"55-59","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"60-69","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"18-21","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.21","x":"22-24","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.26","x":"25-29","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"30-34","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.08","x":"35-39","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"40-44","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"45-49","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"50-54","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"55-59","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"60-69","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"Taiwan","yref":"y"}],"template":{"data":{"scatter":[{"type":"scatter"}]}},"xaxis":{"dtick":1,"gridcolor":"rgb(0, 0, 0)","side":"top","ticks":""},"yaxis":{"dtick":1,"ticks":"","ticksuffix":"  "}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('c0f0e5e2-f134-43f6-a5aa-a23571c2160a');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="2e2c52e8-5176-4b82-a5b8-e7a9a443f5f9" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("2e2c52e8-5176-4b82-a5b8-e7a9a443f5f9")) {                    Plotly.newPlot(                        "2e2c52e8-5176-4b82-a5b8-e7a9a443f5f9",                        [{"colorscale":[[0.0,"rgb(243, 231, 155)"],[0.16666666666666666,"rgb(250, 196, 132)"],[0.3333333333333333,"rgb(248, 160, 126)"],[0.5,"rgb(235, 127, 134)"],[0.6666666666666666,"rgb(206, 102, 147)"],[0.8333333333333334,"rgb(160, 89, 160)"],[1.0,"rgb(92, 83, 165)"]],"reversescale":false,"showscale":false,"type":"heatmap","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70+"],"y":["China","Japan","South Korea","Taiwan"],"z":[[0.24,0.34,0.24,0.12,0.03,0.02,0.0,0.0,0.01,0.0,0.0],[0.07,0.11,0.18,0.14,0.12,0.1,0.11,0.07,0.05,0.05,0.01],[0.07,0.13,0.28,0.15,0.11,0.1,0.08,0.04,0.02,0.02,0.0],[0.11,0.22,0.23,0.1,0.1,0.09,0.04,0.06,0.03,0.01,0.0]]}],                        {"annotations":[{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.24","x":"18-21","xref":"x","y":"China","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.34","x":"22-24","xref":"x","y":"China","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.24","x":"25-29","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.12","x":"30-34","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"35-39","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"40-44","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"45-49","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"50-54","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"55-59","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"60-69","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"18-21","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"22-24","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.18","x":"25-29","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.14","x":"30-34","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.12","x":"35-39","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"40-44","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"45-49","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"50-54","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"55-59","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"60-69","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"70+","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"18-21","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.13","x":"22-24","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.28","x":"25-29","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.15","x":"30-34","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"35-39","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"40-44","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.08","x":"45-49","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.04","x":"50-54","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"55-59","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"60-69","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"18-21","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.22","x":"22-24","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.23","x":"25-29","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"30-34","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"35-39","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"40-44","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.04","x":"45-49","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"50-54","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"55-59","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"60-69","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"Taiwan","yref":"y"}],"template":{"data":{"scatter":[{"type":"scatter"}]}},"xaxis":{"dtick":1,"gridcolor":"rgb(0, 0, 0)","side":"top","ticks":""},"yaxis":{"dtick":1,"ticks":"","ticksuffix":"  "}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('2e2c52e8-5176-4b82-a5b8-e7a9a443f5f9');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="371893a2-4aab-4b40-b7d7-9e1485ca08c7" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("371893a2-4aab-4b40-b7d7-9e1485ca08c7")) {                    Plotly.newPlot(                        "371893a2-4aab-4b40-b7d7-9e1485ca08c7",                        [{"colorscale":[[0.0,"rgb(243, 231, 155)"],[0.16666666666666666,"rgb(250, 196, 132)"],[0.3333333333333333,"rgb(248, 160, 126)"],[0.5,"rgb(235, 127, 134)"],[0.6666666666666666,"rgb(206, 102, 147)"],[0.8333333333333334,"rgb(160, 89, 160)"],[1.0,"rgb(92, 83, 165)"]],"reversescale":false,"showscale":false,"type":"heatmap","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70+"],"y":["China","Japan","South Korea","Taiwan"],"z":[[0.16,0.35,0.28,0.11,0.05,0.03,0.01,0.01,0.0,0.0,0.0],[0.04,0.12,0.23,0.19,0.13,0.1,0.08,0.05,0.03,0.03,0.0],[0.05,0.09,0.3,0.15,0.11,0.11,0.13,0.04,0.02,0.01,0.0],[0.06,0.18,0.24,0.13,0.13,0.1,0.07,0.05,0.02,0.01,0.0]]}],                        {"annotations":[{"font":{"color":"#000000"},"showarrow":false,"text":"0.16","x":"18-21","xref":"x","y":"China","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.35","x":"22-24","xref":"x","y":"China","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.28","x":"25-29","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"30-34","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"35-39","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"40-44","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"45-49","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"50-54","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"55-59","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"60-69","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.04","x":"18-21","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.12","x":"22-24","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.23","x":"25-29","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.19","x":"30-34","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.13","x":"35-39","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"40-44","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.08","x":"45-49","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"50-54","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"55-59","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"60-69","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"18-21","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"22-24","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.3","x":"25-29","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.15","x":"30-34","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"35-39","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"40-44","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.13","x":"45-49","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.04","x":"50-54","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"55-59","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"60-69","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"18-21","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.18","x":"22-24","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.24","x":"25-29","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.13","x":"30-34","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.13","x":"35-39","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"40-44","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"45-49","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"50-54","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"55-59","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"60-69","xref":"x","y":"Taiwan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"Taiwan","yref":"y"}],"template":{"data":{"scatter":[{"type":"scatter"}]}},"xaxis":{"dtick":1,"gridcolor":"rgb(0, 0, 0)","side":"top","ticks":""},"yaxis":{"dtick":1,"ticks":"","ticksuffix":"  "}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('371893a2-4aab-4b40-b7d7-9e1485ca08c7');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="84a63d82-0628-44aa-8037-f5411e50e701" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("84a63d82-0628-44aa-8037-f5411e50e701")) {                    Plotly.newPlot(                        "84a63d82-0628-44aa-8037-f5411e50e701",                        [{"colorscale":[[0.0,"rgb(243, 231, 155)"],[0.16666666666666666,"rgb(250, 196, 132)"],[0.3333333333333333,"rgb(248, 160, 126)"],[0.5,"rgb(235, 127, 134)"],[0.6666666666666666,"rgb(206, 102, 147)"],[0.8333333333333334,"rgb(160, 89, 160)"],[1.0,"rgb(92, 83, 165)"]],"reversescale":false,"showscale":false,"type":"heatmap","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70-79","80+"],"y":["China","Japan","South Korea"],"z":[[0.15,0.37,0.32,0.09,0.04,0.02,0.01,0.0,0.0,0.0,0.0,0.0],[0.07,0.15,0.24,0.17,0.14,0.11,0.06,0.03,0.02,0.02,0.01,0.0],[0.03,0.14,0.35,0.17,0.09,0.08,0.1,0.01,0.01,0.02,0.0,0.0]]}],                        {"annotations":[{"font":{"color":"#000000"},"showarrow":false,"text":"0.15","x":"18-21","xref":"x","y":"China","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.37","x":"22-24","xref":"x","y":"China","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.32","x":"25-29","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"30-34","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.04","x":"35-39","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"40-44","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"45-49","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"50-54","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"55-59","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"60-69","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70-79","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"80+","xref":"x","y":"China","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"18-21","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.15","x":"22-24","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.24","x":"25-29","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.17","x":"30-34","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.14","x":"35-39","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"40-44","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"45-49","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"50-54","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"55-59","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"60-69","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"70-79","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"80+","xref":"x","y":"Japan","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"18-21","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.14","x":"22-24","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.35","x":"25-29","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.17","x":"30-34","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"35-39","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.08","x":"40-44","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.1","x":"45-49","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"50-54","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"55-59","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"60-69","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70-79","xref":"x","y":"South Korea","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"80+","xref":"x","y":"South Korea","yref":"y"}],"template":{"data":{"scatter":[{"type":"scatter"}]}},"xaxis":{"dtick":1,"gridcolor":"rgb(0, 0, 0)","side":"top","ticks":""},"yaxis":{"dtick":1,"ticks":"","ticksuffix":"  "}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('84a63d82-0628-44aa-8037-f5411e50e701');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### 연령별 지역 


```python
# 연령-지역 %
dfKo_Age21= df21Age_Ea[df21Age_Ea['East_Asia']=='South Korea']
dfKo_Age21_per=dfKo_Age21['2021'].value_counts().to_frame().reset_index()
dfKo_Age21_per['South Korea']=((dfKo_Age21_per['2021'] / len(dfKo_Age21))*100).round(2)

dfTw_Age21= df21Age_Ea[df21Age_Ea['East_Asia']=='Taiwan']
dfTw_Age21_per=dfTw_Age21['2021'].value_counts().to_frame().reset_index()
dfTw_Age21_per['Taiwan']=((dfTw_Age21_per['2021'] / len(dfTw_Age21))*100).round(2)
dfTw_Age21_per

dfCh_Age21= df21Age_Ea[df21Age_Ea['East_Asia']=='China']
dfCh_Age21_per=dfCh_Age21['2021'].value_counts().to_frame().reset_index()
dfCh_Age21_per['China']=((dfCh_Age21_per['2021'] / len(dfCh_Age21))*100).round(2)
dfCh_Age21_per

df21Age_Ea.head()
dfJp_Age21= df21Age_Ea[df21Age_Ea['East_Asia']=='Japan']
dfJp_Age21_per=dfJp_Age21['2021'].value_counts().to_frame().reset_index()
dfJp_Age21_per['Japan']=((dfJp_Age21_per['2021'] / len(dfJp_Age21))*100).round(2)
dfJp_Age21_per



#g 그리기(heatMap)
merge1= pd.merge(dfKo_Age21_per,dfTw_Age21_per, on='index', how='outer')
merge2= pd.merge(dfCh_Age21_per,dfJp_Age21_per, on='index', how='outer')
merge= pd.merge(merge1,merge2, on='index', how='outer').fillna(0).sort_values(by=['index'],ascending=True)

merge.iloc[:,[2,4,6,8]]
merge.iloc[:,[2,4,6,8]].to_numpy()



fig = go.Figure(data=go.Heatmap(
                   z=merge.iloc[:,[2,4,6,8]].to_numpy(),
                   x=['South Korea','Taiwan','China','Japan'],
                   y=merge.sort_values(by=['index'],ascending=True)['index'].tolist(),
                   hoverongaps = False,
                   opacity=1.0, xgap=2.5, ygap=2.5, colorscale='orrd'),
                   )
fig.show()

```


<div>                            <div id="46c6ee8d-49fa-4a3d-865c-353c3f2c6e30" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("46c6ee8d-49fa-4a3d-865c-353c3f2c6e30")) {                    Plotly.newPlot(                        "46c6ee8d-49fa-4a3d-865c-353c3f2c6e30",                        [{"colorscale":[[0.0,"rgb(255,247,236)"],[0.125,"rgb(254,232,200)"],[0.25,"rgb(253,212,158)"],[0.375,"rgb(253,187,132)"],[0.5,"rgb(252,141,89)"],[0.625,"rgb(239,101,72)"],[0.75,"rgb(215,48,31)"],[0.875,"rgb(179,0,0)"],[1.0,"rgb(127,0,0)"]],"hoverongaps":false,"opacity":1.0,"type":"heatmap","x":["South Korea","Taiwan","China","Japan"],"xgap":2.5,"y":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70+"],"ygap":2.5,"z":[[4.74,9.58,25.31,6.95],[12.81,20.66,33.78,10.1],[29.53,26.05,19.53,18.35],[14.76,11.38,13.39,15.2],[8.91,8.38,4.79,10.75],[7.24,6.89,1.72,10.31],[9.75,7.19,0.98,9.99],[8.36,5.99,0.12,6.62],[2.79,1.8,0.25,5.43],[1.11,2.1,0.0,5.32],[0.0,0.0,0.12,0.98]]}],                        {"template":{"data":{"scatter":[{"type":"scatter"}]}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('46c6ee8d-49fa-4a3d-865c-353c3f2c6e30');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### 0.2 Kaggle Edu (Ea)
### 0.1.1 data 전처리
### 0.2.1 그래프 그리기 


```python
# 연도별 학력
df21Edu_Ea = df21_Ea.loc[:,['Q3','Q4']].reset_index().rename(columns={'Q3':'East_Asia', 'Q4':'2021'}).fillna('etc')
df20Edu_Ea = df20_Ea.loc[:,['Q3','Q4']].reset_index().rename(columns={'Q3':'East_Asia', 'Q4':'2020'}).fillna('etc')
df19Edu_Ea = df19_Ea.loc[:,['Q3','Q4']].reset_index().rename(columns={'Q3':'East_Asia', 'Q4':'2019'}).fillna('etc')
df18Edu_Ea = df18_Ea.loc[:,['Q3','Q4']].reset_index().rename(columns={'Q3':'East_Asia', 'Q4':'2018'}).fillna('etc')
df17Edu_Ea = df17_Ea.loc[:,['Country','FormalEducation']].reset_index().rename(columns={'Country':'East_Asia', 'FormalEducation':'2017'}).fillna('etc')

df21Edu_Ea =df21Edu_Ea.replace({'I prefer not to answer':'etc'}).sort_values(by='2021', ascending=False)
df20Edu_Ea =df20Edu_Ea.replace({'I prefer not to answer':'etc'}).sort_values(by='2020', ascending=False)
df19Edu_Ea =df19Edu_Ea.replace({'I prefer not to answer':'etc'}).sort_values(by='2019', ascending=False)
df18Edu_Ea =df18Edu_Ea.replace({'I prefer not to answer':'etc'}).sort_values(by='2018', ascending=False)
df17Edu_Ea =df17Edu_Ea.replace({'I prefer not to answer':'etc'}).sort_values(by='2017', ascending=False)

#data frame 정리
dfEdu21 =df21Edu_Ea.groupby(['East_Asia','2021']).size().reset_index().rename(columns = {0:"Count"})
dfEdu20 =df20Edu_Ea.groupby(['East_Asia','2020']).size().reset_index().rename(columns = {0:"Count"})
dfEdu19 =df19Edu_Ea.groupby(['East_Asia','2019']).size().reset_index().rename(columns = {0:"Count"})
dfEdu18 =df18Edu_Ea.groupby(['East_Asia','2018']).size().reset_index().rename(columns = {0:"Count"})
dfEdu17 =(df17Edu_Ea.groupby(['East_Asia','2017'])
          .size().reset_index().rename(columns = {0:"Count"}))

# 비율 data 추가
##21
dfEdu21_percent =df21Edu_Ea.groupby(['East_Asia','2021']).size().reset_index().rename(columns = {0:"Count"})
dfEdu21_percent['percent'] =((dfEdu21_percent['Count'] / len(df21Edu_Ea))*100).round(2)
dfEdu21_percent['percent_str'] =((dfEdu21_percent['Count'] / len(df21Edu_Ea))*100).round(2).astype(str) + '%'
##20
dfEdu20_percent =df20Edu_Ea.groupby(['East_Asia','2020']).size().reset_index().rename(columns = {0:"Count"})
dfEdu20_percent['percent'] =((dfEdu20_percent['Count'] / len(df20Edu_Ea))*100).round(2)
dfEdu20_percent['percent_str'] =((dfEdu20_percent['Count'] / len(df20Edu_Ea))*100).round(2).astype(str) + '%'
##19
dfEdu19_percent =df19Edu_Ea.groupby(['East_Asia','2019']).size().reset_index().rename(columns = {0:"Count"})
dfEdu19_percent['percent'] =((dfEdu19_percent['Count'] / len(df19Edu_Ea))*100).round(2)
dfEdu19_percent['percent_str'] =((dfEdu19_percent['Count'] / len(df19Edu_Ea))*100).round(2).astype(str) + '%'
##18
dfEdu18_percent =df18Edu_Ea.groupby(['East_Asia','2018']).size().reset_index().rename(columns = {0:"Count"})
dfEdu18_percent['percent'] =((dfEdu18_percent['Count'] / len(df18Edu_Ea))*100).round(2)
dfEdu18_percent['percent_str'] =((dfEdu18_percent['Count'] / len(df18Edu_Ea))*100).round(2).astype(str) + '%'
##19
dfEdu17_percent =df17Edu_Ea.groupby(['East_Asia','2017']).size().reset_index().rename(columns = {0:"Count"})
dfEdu17_percent['percent'] =((dfEdu17_percent['Count'] / len(df17Edu_Ea))*100).round(2)
dfEdu17_percent['percent_str'] =((dfEdu17_percent['Count'] / len(df17Edu_Ea))*100).round(2).astype(str) + '%'


```


```python

#21년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfEdu21_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2021'], y = group['percent'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2021_나라별 학력')
fig.show()

#20년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfEdu20_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2020'], y = group['percent'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2020_나라별 학력')

fig.show()

#19년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfEdu19_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2019'], y = group['percent'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2019_나라별 학력')

fig.show()


#18년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfEdu18_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2018'], y = group['percent'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2018_나라별 학력')

fig.show()


#17년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfEdu17_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2017'], y = group['percent'], name = country
   ))
fig.update_layout(barmode="group", 
                 plot_bgcolor = "white",
                 title='2017_나라별 학력')

fig.show()

```


<div>                            <div id="606e8a00-f0e9-4a4a-8b32-205f790116b4" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("606e8a00-f0e9-4a4a-8b32-205f790116b4")) {                    Plotly.newPlot(                        "606e8a00-f0e9-4a4a-8b32-205f790116b4",                        [{"name":"China","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional doctorate","Some college/university study without earning a bachelor’s degree","etc"],"y":[8.94,2.88,16.31,0.37,0.37,3.75,0.91]},{"name":"Japan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional doctorate","Some college/university study without earning a bachelor’s degree","etc"],"y":[9.93,4.24,15.82,1.94,0.49,3.58,1.94]},{"name":"South Korea","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional doctorate","Some college/university study without earning a bachelor’s degree","etc"],"y":[5.23,2.27,3.79,0.62,0.25,2.14,0.49]},{"name":"Taiwan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional doctorate","Some college/university study without earning a bachelor’s degree","etc"],"y":[2.72,1.65,7.58,0.21,0.25,0.99,0.37]}],                        {"barmode":"group","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2021_나라별 학력"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('606e8a00-f0e9-4a4a-8b32-205f790116b4');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="336395a3-791a-4ae5-81df-56d9a89b18d2" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("336395a3-791a-4ae5-81df-56d9a89b18d2")) {                    Plotly.newPlot(                        "336395a3-791a-4ae5-81df-56d9a89b18d2",                        [{"name":"China","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[7.23,2.49,13.37,0.06,0.36,2.49,2.8]},{"name":"Japan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[9.24,4.07,16.29,1.03,0.43,3.65,4.07]},{"name":"South Korea","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[4.13,2.43,4.98,0.49,0.61,2.07,1.46]},{"name":"Taiwan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[2.61,1.7,9.3,0.3,0.24,0.91,1.16]}],                        {"barmode":"group","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2020_나라별 학력"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('336395a3-791a-4ae5-81df-56d9a89b18d2');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="c93605ee-c898-4dda-b890-493654aefd36" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c93605ee-c898-4dda-b890-493654aefd36")) {                    Plotly.newPlot(                        "c93605ee-c898-4dda-b890-493654aefd36",                        [{"name":"China","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[7.99,3.55,15.7,0.11,0.5,1.66,2.33]},{"name":"Japan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[8.43,4.44,16.75,1.05,0.33,3.0,3.33]},{"name":"South Korea","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[3.88,2.33,4.38,0.28,0.55,1.44,1.28]},{"name":"Taiwan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[2.5,2.38,9.6,0.17,0.44,0.72,0.89]}],                        {"barmode":"group","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2019_나라별 학력"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('c93605ee-c898-4dda-b890-493654aefd36');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="6b61c5e7-9b62-4e80-be93-b0bbb770b656" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("6b61c5e7-9b62-4e80-be93-b0bbb770b656")) {                    Plotly.newPlot(                        "6b61c5e7-9b62-4e80-be93-b0bbb770b656",                        [{"name":"China","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[14.32,7.0,34.04,0.68,0.92,3.8,5.0]},{"name":"Japan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[5.76,2.88,10.92,0.92,0.28,1.76,1.36]},{"name":"South Korea","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[3.44,1.32,3.68,0.12,0.36,0.6,0.84]}],                        {"barmode":"group","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2018_나라별 학력"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('6b61c5e7-9b62-4e80-be93-b0bbb770b656');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="12f3b019-a42d-48b2-b3eb-9c1d8b82e34b" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("12f3b019-a42d-48b2-b3eb-9c1d8b82e34b")) {                    Plotly.newPlot(                        "12f3b019-a42d-48b2-b3eb-9c1d8b82e34b",                        [{"name":"China","type":"bar","x":["Bachelor's degree","Doctoral degree","I did not complete any formal education past high school","Master's degree","Professional degree","Some college/university study without earning a bachelor's degree","etc"],"y":[13.88,3.93,0.75,14.21,0.33,1.59,4.68]},{"name":"Japan","type":"bar","x":["Bachelor's degree","Doctoral degree","I did not complete any formal education past high school","Master's degree","Professional degree","Some college/university study without earning a bachelor's degree","etc"],"y":[6.02,3.85,0.84,8.61,0.42,1.34,2.09]},{"name":"South Korea","type":"bar","x":["Bachelor's degree","Doctoral degree","I did not complete any formal education past high school","Master's degree","Professional degree","Some college/university study without earning a bachelor's degree","etc"],"y":[5.02,2.51,0.5,4.77,0.25,0.92,2.26]},{"name":"Taiwan","type":"bar","x":["Bachelor's degree","Doctoral degree","I did not complete any formal education past high school","Master's degree","Professional degree","Some college/university study without earning a bachelor's degree","etc"],"y":[5.43,2.51,0.17,9.03,0.25,1.17,2.68]}],                        {"barmode":"group","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2017_나라별 학력"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('12f3b019-a42d-48b2-b3eb-9c1d8b82e34b');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python

#21년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfEdu21_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2021'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="stack", 
                 plot_bgcolor = "white",
                 title='2021_나라별 학력 비율 ')
fig.show()

#20년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfEdu20_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2020'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="stack", 
                 plot_bgcolor = "white",
                 title='2020_나라별 학력 비율 ')

fig.show()

#19년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfEdu19_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2019'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="stack", 
                 plot_bgcolor = "white",
                 title='2019_나라별 학력 비율 ')

fig.show()


#18년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfEdu18_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2018'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="stack", 
                 plot_bgcolor = "white",
                 title='2018_나라별 학력 비율  ')

fig.show()


#17년도 Bar graph 그리기
fig = go.Figure()

for country, group  in dfEdu17_percent.groupby('East_Asia'):
   fig.add_trace(go.Bar(
       x = group['2017'], y = group['Count'], name = country
   ))
fig.update_layout(barmode="stack", 
                 plot_bgcolor = "white",
                 title='2017_나라별 학력 비율 ')

fig.show()

```


<div>                            <div id="c4ac4e5e-09be-4e8f-9775-aaaa2efa2f37" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c4ac4e5e-09be-4e8f-9775-aaaa2efa2f37")) {                    Plotly.newPlot(                        "c4ac4e5e-09be-4e8f-9775-aaaa2efa2f37",                        [{"name":"China","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional doctorate","Some college/university study without earning a bachelor’s degree","etc"],"y":[217,70,396,9,9,91,22]},{"name":"Japan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional doctorate","Some college/university study without earning a bachelor’s degree","etc"],"y":[241,103,384,47,12,87,47]},{"name":"South Korea","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional doctorate","Some college/university study without earning a bachelor’s degree","etc"],"y":[127,55,92,15,6,52,12]},{"name":"Taiwan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional doctorate","Some college/university study without earning a bachelor’s degree","etc"],"y":[66,40,184,5,6,24,9]}],                        {"barmode":"stack","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2021_나라별 학력 비율 "}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('c4ac4e5e-09be-4e8f-9775-aaaa2efa2f37');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="5ed0509a-5b0e-4fd2-b2b0-29f73002262a" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("5ed0509a-5b0e-4fd2-b2b0-29f73002262a")) {                    Plotly.newPlot(                        "5ed0509a-5b0e-4fd2-b2b0-29f73002262a",                        [{"name":"China","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[119,41,220,1,6,41,46]},{"name":"Japan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[152,67,268,17,7,60,67]},{"name":"South Korea","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[68,40,82,8,10,34,24]},{"name":"Taiwan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[43,28,153,5,4,15,19]}],                        {"barmode":"stack","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2020_나라별 학력 비율 "}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('5ed0509a-5b0e-4fd2-b2b0-29f73002262a');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="60631bb4-7380-4ff5-a04b-ba39675b9a37" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("60631bb4-7380-4ff5-a04b-ba39675b9a37")) {                    Plotly.newPlot(                        "60631bb4-7380-4ff5-a04b-ba39675b9a37",                        [{"name":"China","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[144,64,283,2,9,30,42]},{"name":"Japan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[152,80,302,19,6,54,60]},{"name":"South Korea","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[70,42,79,5,10,26,23]},{"name":"Taiwan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[45,43,173,3,8,13,16]}],                        {"barmode":"stack","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2019_나라별 학력 비율 "}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('60631bb4-7380-4ff5-a04b-ba39675b9a37');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="4c8a390a-e60e-4efe-bd75-b3a5e769c0d4" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("4c8a390a-e60e-4efe-bd75-b3a5e769c0d4")) {                    Plotly.newPlot(                        "4c8a390a-e60e-4efe-bd75-b3a5e769c0d4",                        [{"name":"China","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[358,175,851,17,23,95,125]},{"name":"Japan","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[144,72,273,23,7,44,34]},{"name":"South Korea","type":"bar","x":["Bachelor’s degree","Doctoral degree","Master’s degree","No formal education past high school","Professional degree","Some college/university study without earning a bachelor’s degree","etc"],"y":[86,33,92,3,9,15,21]}],                        {"barmode":"stack","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2018_나라별 학력 비율  "}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('4c8a390a-e60e-4efe-bd75-b3a5e769c0d4');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



<div>                            <div id="a736f17b-42b5-439e-bfcc-fdcc467646fd" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("a736f17b-42b5-439e-bfcc-fdcc467646fd")) {                    Plotly.newPlot(                        "a736f17b-42b5-439e-bfcc-fdcc467646fd",                        [{"name":"China","type":"bar","x":["Bachelor's degree","Doctoral degree","I did not complete any formal education past high school","Master's degree","Professional degree","Some college/university study without earning a bachelor's degree","etc"],"y":[166,47,9,170,4,19,56]},{"name":"Japan","type":"bar","x":["Bachelor's degree","Doctoral degree","I did not complete any formal education past high school","Master's degree","Professional degree","Some college/university study without earning a bachelor's degree","etc"],"y":[72,46,10,103,5,16,25]},{"name":"South Korea","type":"bar","x":["Bachelor's degree","Doctoral degree","I did not complete any formal education past high school","Master's degree","Professional degree","Some college/university study without earning a bachelor's degree","etc"],"y":[60,30,6,57,3,11,27]},{"name":"Taiwan","type":"bar","x":["Bachelor's degree","Doctoral degree","I did not complete any formal education past high school","Master's degree","Professional degree","Some college/university study without earning a bachelor's degree","etc"],"y":[65,30,2,108,3,14,32]}],                        {"barmode":"stack","plot_bgcolor":"white","template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"text":"2017_나라별 학력 비율 "}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('a736f17b-42b5-439e-bfcc-fdcc467646fd');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python

z = df21_Ea.groupby(['Q4', 'Q1']).size().unstack().fillna(0).astype('int64')
z_data = z.apply(lambda x:np.round(x/x.sum(), 2), axis = 1).to_numpy() # convert to correlation matrix
x = z.columns.tolist()
y = z.index.tolist()

fig = ff.create_annotated_heatmap(z_data, x = x, y = y, colorscale = "sunset")
fig.show()
```


<div>                            <div id="f304db3b-8835-4153-8f92-eb2f3852f1dd" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("f304db3b-8835-4153-8f92-eb2f3852f1dd")) {                    Plotly.newPlot(                        "f304db3b-8835-4153-8f92-eb2f3852f1dd",                        [{"colorscale":[[0.0,"rgb(243, 231, 155)"],[0.16666666666666666,"rgb(250, 196, 132)"],[0.3333333333333333,"rgb(248, 160, 126)"],[0.5,"rgb(235, 127, 134)"],[0.6666666666666666,"rgb(206, 102, 147)"],[0.8333333333333334,"rgb(160, 89, 160)"],[1.0,"rgb(92, 83, 165)"]],"reversescale":false,"showscale":false,"type":"heatmap","x":["18-21","22-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-69","70+"],"y":["Bachelor’s degree","Doctoral degree","I prefer not to answer","Master’s degree","No formal education past high school","Professional doctorate","Some college/university study without earning a bachelor’s degree"],"z":[[0.21,0.19,0.19,0.12,0.08,0.06,0.06,0.04,0.03,0.03,0.0],[0.01,0.05,0.18,0.22,0.11,0.08,0.11,0.08,0.06,0.08,0.01],[0.09,0.09,0.22,0.12,0.07,0.17,0.09,0.06,0.06,0.04,0.0],[0.05,0.27,0.27,0.15,0.08,0.06,0.06,0.03,0.02,0.01,0.0],[0.2,0.09,0.14,0.17,0.05,0.11,0.07,0.11,0.04,0.0,0.03],[0.03,0.03,0.12,0.09,0.12,0.15,0.09,0.15,0.06,0.09,0.06],[0.42,0.18,0.12,0.05,0.06,0.05,0.05,0.04,0.02,0.02,0.0]]}],                        {"annotations":[{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.21","x":"18-21","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.19","x":"22-24","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.19","x":"25-29","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.12","x":"30-34","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.08","x":"35-39","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"40-44","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"45-49","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.04","x":"50-54","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"55-59","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"60-69","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"Bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"18-21","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"22-24","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.18","x":"25-29","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.22","x":"30-34","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"35-39","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.08","x":"40-44","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"45-49","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.08","x":"50-54","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"55-59","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.08","x":"60-69","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"70+","xref":"x","y":"Doctoral degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"18-21","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"22-24","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.22","x":"25-29","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.12","x":"30-34","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"35-39","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.17","x":"40-44","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"45-49","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"50-54","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"55-59","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.04","x":"60-69","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"I prefer not to answer","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"18-21","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.27","x":"22-24","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.27","x":"25-29","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.15","x":"30-34","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.08","x":"35-39","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"40-44","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"45-49","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"50-54","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"55-59","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.01","x":"60-69","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"Master’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.2","x":"18-21","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"22-24","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.14","x":"25-29","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.17","x":"30-34","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"35-39","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"40-44","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.07","x":"45-49","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.11","x":"50-54","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.04","x":"55-59","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"60-69","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"70+","xref":"x","y":"No formal education past high school","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"18-21","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.03","x":"22-24","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.12","x":"25-29","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"30-34","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.12","x":"35-39","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.15","x":"40-44","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"45-49","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.15","x":"50-54","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"55-59","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.09","x":"60-69","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"70+","xref":"x","y":"Professional doctorate","yref":"y"},{"font":{"color":"#FFFFFF"},"showarrow":false,"text":"0.42","x":"18-21","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.18","x":"22-24","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.12","x":"25-29","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"30-34","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.06","x":"35-39","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"40-44","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.05","x":"45-49","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.04","x":"50-54","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"55-59","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.02","x":"60-69","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"},{"font":{"color":"#000000"},"showarrow":false,"text":"0.0","x":"70+","xref":"x","y":"Some college/university study without earning a bachelor’s degree","yref":"y"}],"template":{"data":{"scatter":[{"type":"scatter"}]}},"xaxis":{"dtick":1,"gridcolor":"rgb(0, 0, 0)","side":"top","ticks":""},"yaxis":{"dtick":1,"ticks":"","ticksuffix":"  "}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('f304db3b-8835-4153-8f92-eb2f3852f1dd');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


## 경력


```python
#전체 코드
_3year = ['I have never written code', '< 1 years', '1-3 years']
_5year = ['3-5 years ','5-10 years']
_10year = ['10-20 years','20+ years']

df21_3year = df21['Q6'][df21['Q6'].isin(_3year)]
df21_5year = df21['Q6'][df21['Q6'].isin(_5year)]
df21_10year = df21['Q6'][df21['Q6'].isin(_10year)]

df21_3year.count()
df21_5year.count()
df21_10year.count()

years =['_3year','_5year', '_10year']
values =[df21_3year.count(),
         df21_5year.count(),
        df21_10year.count()]

fig = go.Figure(data=[
    go.Bar(name='21년 World kaggler들의 경력', x=years, y=values ,orientation='v'),])

fig.update_layout(title_text="<b>21년 World kaggler들의 경력</b>",title_font_size=35)

fig.show()
```


<div>                            <div id="343ea7ed-f974-4065-8a78-78bfe20cb3fe" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("343ea7ed-f974-4065-8a78-78bfe20cb3fe")) {                    Plotly.newPlot(                        "343ea7ed-f974-4065-8a78-78bfe20cb3fe",                        [{"name":"21년 World kaggler들의 경력","orientation":"v","type":"bar","x":["_3year","_5year","_10year"],"y":[14787,3099,4026]}],                        {"template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"font":{"size":35},"text":"<b>21년 World kaggler들의 경력</b>"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('343ea7ed-f974-4065-8a78-78bfe20cb3fe');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
#최종 합친 코드
_3year = ['I have never written code', '< 1 years', '1-3 years']
_5year = ['3-5 years ','5-10 years']
_10year = ['10-20 years','20+ years']

df21_Ea_3year = df21_Ea['Q3'][df21_Ea['Q6'].isin(_3year)].value_counts().to_frame().rename(columns = {'Q3':'3year'})
df21_Ea_5year = df21_Ea['Q3'][df21_Ea['Q6'].isin(_5year)].value_counts().to_frame().rename(columns = {'Q3':'5year'})
df21_Ea_10year = df21_Ea['Q3'][df21_Ea['Q6'].isin(_10year)].value_counts().to_frame().rename(columns = {'Q3':'10year'})

career=(df21_Ea_3year.join(df21_Ea_5year).join(df21_Ea_10year))
career

career.iloc[0,0:3] #China
career.iloc[1,0:3] #Japan
career.iloc[2,0:3] #South Korea
career.iloc[3,0:3] #Taiwan

fig = go.Figure(data=[
    go.Bar(name='China', x = years, 
                           y=career.iloc[0,0:3]),
    go.Bar(name='Japan', x = years, 
                             y=career.iloc[1,0:3]),
    go.Bar(name='South Korea', x= years, 
                            y=career.iloc[2,0:3]),
    go.Bar(name='Taiwan', x= years, 
                            y=career.iloc[3,0:3])
    ])

fig.update_layout(title_text="<b>21년 EastAisa kaggler들의 경력</b>",title_font_size=35)
fig.show()
```


<div>                            <div id="e2aecdc1-cdab-40dc-8a1f-adf3c8dc3ee1" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("e2aecdc1-cdab-40dc-8a1f-adf3c8dc3ee1")) {                    Plotly.newPlot(                        "e2aecdc1-cdab-40dc-8a1f-adf3c8dc3ee1",                        [{"name":"China","type":"bar","x":["_3year","_5year","_10year"],"y":[542,65,52]},{"name":"Japan","type":"bar","x":["_3year","_5year","_10year"],"y":[445,108,228]},{"name":"South Korea","type":"bar","x":["_3year","_5year","_10year"],"y":[206,45,54]},{"name":"Taiwan","type":"bar","x":["_3year","_5year","_10year"],"y":[185,47,46]}],                        {"template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"font":{"size":35},"text":"<b>21년 EastAisa kaggler들의 경력</b>"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('e2aecdc1-cdab-40dc-8a1f-adf3c8dc3ee1');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### 연봉


```python
#전체 코드

#마지막 행 삭제해줌
df21_=(df21['Q25'].value_counts().to_frame())
#df21_=df21_.drop(df21_.index[26])
#df21_

compensation = df21_['Q25'].index
fig = go.Figure(data=[
    go.Bar(name='21년 World kaggler들의 연봉', x=compensation, y=df21_['Q25'].to_numpy() ,orientation='v')])

fig.update_layout(title_text="<b>21년 World kaggler들의 연봉</b>",title_font_size=35)

fig.show()
```


<div>                            <div id="de5a4fd6-45d2-4001-8de4-a357e70389c8" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("de5a4fd6-45d2-4001-8de4-a357e70389c8")) {                    Plotly.newPlot(                        "de5a4fd6-45d2-4001-8de4-a357e70389c8",                        [{"name":"21년 World kaggler들의 연봉","orientation":"v","type":"bar","x":["$0-999","1,000-1,999","10,000-14,999","30,000-39,999","100,000-124,999","5,000-7,499","50,000-59,999","40,000-49,999","20,000-24,999","2,000-2,999","15,000-19,999","7,500-9,999","60,000-69,999","25,000-29,999","70,000-79,999","4,000-4,999","150,000-199,999","80,000-89,999","3,000-3,999","125,000-149,999","90,000-99,999","200,000-249,999","300,000-499,999","250,000-299,999",">$1,000,000","$500,000-999,999"],"y":[3369,969,950,741,725,699,697,688,587,575,573,552,551,470,464,456,392,391,380,379,350,177,91,75,58,32]}],                        {"template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"font":{"size":35},"text":"<b>21년 World kaggler들의 연봉</b>"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('de5a4fd6-45d2-4001-8de4-a357e70389c8');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
compensation = df21_['Q25'].index

fig = go.Figure(data=[
    go.Bar(name='China', x = compensation, 
                           y = df21_Ea['Q25'][df21_Ea['Q3'] =='Japan'].value_counts()),
    
    go.Bar(name='Japan', x = compensation, 
                             y=df21_Ea['Q25'][df21_Ea['Q3'] =='Taiwan'].value_counts()),
    
    go.Bar(name='South Korea', x = compensation, 
                            y=df21_Ea['Q25'][df21_Ea['Q3'] =='South Korea'].value_counts()),
    
    go.Bar(name='Taiwan', x = compensation, 
                            y=df21_Ea['Q25'][df21_Ea['Q3'] =='China'].value_counts())
    ])

fig.update_layout(title_text="<b>21년 EastAisa kaggler들의 연봉</b>",title_font_size=35)
fig.show()
```


<div>                            <div id="94a83dbe-42e5-40aa-95e0-898c459b78d7" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("94a83dbe-42e5-40aa-95e0-898c459b78d7")) {                    Plotly.newPlot(                        "94a83dbe-42e5-40aa-95e0-898c459b78d7",                        [{"name":"China","type":"bar","x":["$0-999","1,000-1,999","10,000-14,999","30,000-39,999","100,000-124,999","5,000-7,499","50,000-59,999","40,000-49,999","20,000-24,999","2,000-2,999","15,000-19,999","7,500-9,999","60,000-69,999","25,000-29,999","70,000-79,999","4,000-4,999","150,000-199,999","80,000-89,999","3,000-3,999","125,000-149,999","90,000-99,999","200,000-249,999","300,000-499,999","250,000-299,999",">$1,000,000","$500,000-999,999"],"y":[79,74,60,53,46,44,43,36,31,28,27,27,20,18,15,10,9,8,8,6,5,2,1,1,1]},{"name":"Japan","type":"bar","x":["$0-999","1,000-1,999","10,000-14,999","30,000-39,999","100,000-124,999","5,000-7,499","50,000-59,999","40,000-49,999","20,000-24,999","2,000-2,999","15,000-19,999","7,500-9,999","60,000-69,999","25,000-29,999","70,000-79,999","4,000-4,999","150,000-199,999","80,000-89,999","3,000-3,999","125,000-149,999","90,000-99,999","200,000-249,999","300,000-499,999","250,000-299,999",">$1,000,000","$500,000-999,999"],"y":[28,23,19,14,13,12,10,10,10,10,7,6,6,5,3,3,2,2,2,1,1,1]},{"name":"South Korea","type":"bar","x":["$0-999","1,000-1,999","10,000-14,999","30,000-39,999","100,000-124,999","5,000-7,499","50,000-59,999","40,000-49,999","20,000-24,999","2,000-2,999","15,000-19,999","7,500-9,999","60,000-69,999","25,000-29,999","70,000-79,999","4,000-4,999","150,000-199,999","80,000-89,999","3,000-3,999","125,000-149,999","90,000-99,999","200,000-249,999","300,000-499,999","250,000-299,999",">$1,000,000","$500,000-999,999"],"y":[33,21,18,17,15,12,12,11,11,11,9,8,7,6,6,6,5,5,4,4,2,2]},{"name":"Taiwan","type":"bar","x":["$0-999","1,000-1,999","10,000-14,999","30,000-39,999","100,000-124,999","5,000-7,499","50,000-59,999","40,000-49,999","20,000-24,999","2,000-2,999","15,000-19,999","7,500-9,999","60,000-69,999","25,000-29,999","70,000-79,999","4,000-4,999","150,000-199,999","80,000-89,999","3,000-3,999","125,000-149,999","90,000-99,999","200,000-249,999","300,000-499,999","250,000-299,999",">$1,000,000","$500,000-999,999"],"y":[106,28,21,21,20,19,18,18,15,12,12,12,9,7,7,7,7,6,6,5,5,4,2]}],                        {"template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"font":{"size":35},"text":"<b>21년 EastAisa kaggler들의 연봉</b>"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('94a83dbe-42e5-40aa-95e0-898c459b78d7');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### 언어


```python
df21['Q7_Part_1'].value_counts()
```




    Python    21860
    Name: Q7_Part_1, dtype: int64




```python
#코드 전체

df21_p = df21['Q7_Part_1'].value_counts().to_frame() #python
df21_r = df21['Q7_Part_2'].value_counts().to_frame() #r
df21_s = df21['Q7_Part_3'].value_counts().to_frame() #sql
df21_c = df21['Q7_Part_4'].value_counts().to_frame() #c
df21_cc = df21['Q7_Part_5'].value_counts().to_frame() #c++
df21_j = df21['Q7_Part_6'].value_counts().to_frame() #java
df21_js = df21['Q7_Part_7'].value_counts().to_frame() #javascript
df21_ju = df21['Q7_Part_8'].value_counts().to_frame() #julia
df21_sw = df21['Q7_Part_9'].value_counts().to_frame() #swift
df21_b = df21['Q7_Part_10'].value_counts().to_frame() #bash
df21_ma = df21['Q7_Part_11'].value_counts().to_frame() #matlab
df21_n = df21['Q7_Part_12'].value_counts().to_frame() #none

languages = ['Python','R','SQL','C','C++','Java','Javascript','Julia','Swift','Bash','MATLAB','None']

fig = go.Figure(data=[
    go.Bar(name='21년 World kaggler들이 사용하는 언어', x = languages, 
                                                     y = [df21_p.iloc[0,0],
                                                          df21_r.iloc[0,0],
                                                          df21_s.iloc[0,0],
                                                          df21_c.iloc[0,0],
                                                          df21_cc.iloc[0,0],
                                                          df21_j.iloc[0,0],
                                                          df21_js.iloc[0,0],
                                                          df21_ju.iloc[0,0],
                                                          df21_sw.iloc[0,0],
                                                          df21_b.iloc[0,0],
                                                          df21_ma.iloc[0,0],
                                                          df21_n.iloc[0,0]],orientation='v')
                    
                    ])

fig.update_layout(title_text="<b>21년 World kaggler들이 사용하는 언어</b>",title_font_size=35)

fig.show()
```


<div>                            <div id="6a39f4dd-965c-4020-a6f2-683e7c1ecc36" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("6a39f4dd-965c-4020-a6f2-683e7c1ecc36")) {                    Plotly.newPlot(                        "6a39f4dd-965c-4020-a6f2-683e7c1ecc36",                        [{"name":"21년 World kaggler들이 사용하는 언어","orientation":"v","type":"bar","x":["Python","R","SQL","C","C++","Java","Javascript","Julia","Swift","Bash","MATLAB","None"],"y":[21860,5334,10756,4709,5535,4769,4332,305,242,2216,2935,319]}],                        {"template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"font":{"size":35},"text":"<b>21년 World kaggler들이 사용하는 언어</b>"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('6a39f4dd-965c-4020-a6f2-683e7c1ecc36');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
df21_lan_ch_p=df21_Ea['Q7_Part_1'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_1':'cnt'})
df21_lan_ch_r=df21_Ea['Q7_Part_2'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_2':'cnt'})
df21_lan_ch_s=df21_Ea['Q7_Part_3'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_3':'cnt'})
df21_lan_ch_c=df21_Ea['Q7_Part_4'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_4':'cnt'})
df21_lan_ch_cc=df21_Ea['Q7_Part_5'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_5':'cnt'})
df21_lan_ch_j=df21_Ea['Q7_Part_6'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_6':'cnt'})
df21_lan_ch_js=df21_Ea['Q7_Part_7'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_7':'cnt'})
df21_lan_ch_ju=df21_Ea['Q7_Part_8'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_8':'cnt'})
df21_lan_ch_sw=df21_Ea['Q7_Part_9'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_9':'cnt'})
df21_lan_ch_b=df21_Ea['Q7_Part_10'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_10':'cnt'})
df21_lan_ch_ma=df21_Ea['Q7_Part_11'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_11':'cnt'})
df21_lan_ch_n=df21_Ea['Q7_Part_12'][df21_Ea['Q3']=='China'].value_counts().to_frame().rename(columns = {'Q7_Part_12':'cnt'})
ch_lan = pd.concat([df21_lan_ch_p,df21_lan_ch_r,df21_lan_ch_s,df21_lan_ch_c,df21_lan_ch_cc,df21_lan_ch_j,df21_lan_ch_js,df21_lan_ch_ju,df21_lan_ch_sw,df21_lan_ch_b,df21_lan_ch_ma,df21_lan_ch_n])


df21_lan_jp_p=df21_Ea['Q7_Part_1'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_1':'cnt'})
df21_lan_jp_r=df21_Ea['Q7_Part_2'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_2':'cnt'})
df21_lan_jp_s=df21_Ea['Q7_Part_3'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_3':'cnt'})
df21_lan_jp_c=df21_Ea['Q7_Part_4'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_4':'cnt'})
df21_lan_jp_cc=df21_Ea['Q7_Part_5'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_5':'cnt'})
df21_lan_jp_j=df21_Ea['Q7_Part_6'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_6':'cnt'})
df21_lan_jp_js=df21_Ea['Q7_Part_7'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_7':'cnt'})
df21_lan_jp_ju=df21_Ea['Q7_Part_8'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_8':'cnt'})
df21_lan_jp_sw=df21_Ea['Q7_Part_9'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_9':'cnt'})
df21_lan_jp_b=df21_Ea['Q7_Part_10'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_10':'cnt'})
df21_lan_jp_ma=df21_Ea['Q7_Part_11'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_11':'cnt'})
df21_lan_jp_n=df21_Ea['Q7_Part_12'][df21_Ea['Q3']=='Japan'].value_counts().to_frame().rename(columns = {'Q7_Part_12':'cnt'})
jp_lan = pd.concat([df21_lan_jp_p,df21_lan_jp_r,df21_lan_jp_s,df21_lan_jp_c,df21_lan_jp_cc,df21_lan_jp_j,df21_lan_jp_js,df21_lan_jp_ju,df21_lan_jp_sw,df21_lan_jp_b,df21_lan_jp_ma,df21_lan_jp_n])


df21_lan_tw_p=df21_Ea['Q7_Part_1'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_1':'cnt'})
df21_lan_tw_r=df21_Ea['Q7_Part_2'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_2':'cnt'})
df21_lan_tw_s=df21_Ea['Q7_Part_3'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_3':'cnt'})
df21_lan_tw_c=df21_Ea['Q7_Part_4'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_4':'cnt'})
df21_lan_tw_cc=df21_Ea['Q7_Part_5'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_5':'cnt'})
df21_lan_tw_j=df21_Ea['Q7_Part_6'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_6':'cnt'})
df21_lan_tw_js=df21_Ea['Q7_Part_7'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_7':'cnt'})
df21_lan_tw_ju=df21_Ea['Q7_Part_8'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_8':'cnt'})
df21_lan_tw_sw=df21_Ea['Q7_Part_9'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_9':'cnt'})
df21_lan_tw_b=df21_Ea['Q7_Part_10'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_10':'cnt'})
df21_lan_tw_ma=df21_Ea['Q7_Part_11'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_11':'cnt'})
df21_lan_tw_n=df21_Ea['Q7_Part_12'][df21_Ea['Q3']=='Taiwan'].value_counts().to_frame().rename(columns = {'Q7_Part_12':'cnt'})
tw_lan = pd.concat([df21_lan_tw_p,df21_lan_tw_r,df21_lan_tw_s,df21_lan_tw_c,df21_lan_tw_cc,df21_lan_tw_j,df21_lan_tw_js,df21_lan_tw_ju,df21_lan_tw_sw,df21_lan_tw_b,df21_lan_tw_ma,df21_lan_tw_n])


df21_lan_ko_p=df21_Ea['Q7_Part_1'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_1':'cnt'})
df21_lan_ko_r=df21_Ea['Q7_Part_2'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_2':'cnt'})
df21_lan_ko_s=df21_Ea['Q7_Part_3'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_3':'cnt'})
df21_lan_ko_c=df21_Ea['Q7_Part_4'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_4':'cnt'})
df21_lan_ko_cc=df21_Ea['Q7_Part_5'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_5':'cnt'})
df21_lan_ko_j=df21_Ea['Q7_Part_6'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_6':'cnt'})
df21_lan_ko_js=df21_Ea['Q7_Part_7'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_7':'cnt'})
df21_lan_ko_ju=df21_Ea['Q7_Part_8'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_8':'cnt'})
df21_lan_ko_sw=df21_Ea['Q7_Part_9'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_9':'cnt'})
df21_lan_ko_b=df21_Ea['Q7_Part_10'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_10':'cnt'})
df21_lan_ko_ma=df21_Ea['Q7_Part_11'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_11':'cnt'})
df21_lan_ko_n=df21_Ea['Q7_Part_12'][df21_Ea['Q3']=='South Korea'].value_counts().to_frame().rename(columns = {'Q7_Part_12':'cnt'})
ko_lan = pd.concat([df21_lan_ko_p,df21_lan_ko_r,df21_lan_ko_s,df21_lan_ko_c,df21_lan_ko_cc,df21_lan_ko_j,df21_lan_ko_js,df21_lan_ko_ju,df21_lan_ko_sw,df21_lan_ko_b,df21_lan_ko_ma,df21_lan_ko_n])


ch_lan['cnt'].to_list()

languages = ['Python','R','SQL','C','C++','Java','Javascript','Julia','Swift','Bash','MATLAB','None']

fig = go.Figure(data=[
    go.Bar(name='China', x = languages, 
                         y = ch_lan['cnt'].tolist()),
    
    go.Bar(name='Japan', x = languages, 
                             y=jp_lan['cnt'].tolist()),
    
    go.Bar(name='South Korea', x = languages, 
                            y=ko_lan['cnt'].tolist()),
    
    go.Bar(name='Taiwan', x = languages, 
                            y=tw_lan['cnt'].tolist())
          ])

fig.update_layout(title_text="<b>21년 EastAisa kaggler들이 사용하는 언어</b>",title_font_size=35)
fig.show()
```


<div>                            <div id="89ffc1e9-fdab-4bd9-a8e2-44ff4437bee5" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("89ffc1e9-fdab-4bd9-a8e2-44ff4437bee5")) {                    Plotly.newPlot(                        "89ffc1e9-fdab-4bd9-a8e2-44ff4437bee5",                        [{"name":"China","type":"bar","x":["Python","R","SQL","C","C++","Java","Javascript","Julia","Swift","Bash","MATLAB","None"],"y":[738,85,215,226,268,212,86,4,5,31,169]},{"name":"Japan","type":"bar","x":["Python","R","SQL","C","C++","Java","Javascript","Julia","Swift","Bash","MATLAB","None"],"y":[787,122,233,164,164,138,148,8,20,81,53,6]},{"name":"South Korea","type":"bar","x":["Python","R","SQL","C","C++","Java","Javascript","Julia","Swift","Bash","MATLAB","None"],"y":[303,89,89,81,68,57,42,2,6,14,38,3]},{"name":"Taiwan","type":"bar","x":["Python","R","SQL","C","C++","Java","Javascript","Julia","Swift","Bash","MATLAB","None"],"y":[292,71,100,93,96,46,50,3,10,21,55,5]}],                        {"template":{"data":{"scatter":[{"type":"scatter"}]}},"title":{"font":{"size":35},"text":"<b>21년 EastAisa kaggler들이 사용하는 언어</b>"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('89ffc1e9-fdab-4bd9-a8e2-44ff4437bee5');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python

```


```python
#Stacked Area Cart

# q3_q5_q19_2019 = df_2019.loc[:, ['Q3', 'Q5', 'Q19']]

# final_df = pd.concat([q3_q5_q19_2019, q3_q5_q8_2020, q3_q5_q8_2021])

# year_val = final_df['year'].unique().tolist()

# # final_df.groupby(["Q8", "Q5"]).size().unstack().fillna(0).astype("int16")
# year_q5 = final_df.groupby(['year', 'Q5']).size().reset_index().rename(columns = {0:"Count"})
# # print(year_q5.head())

# year_q5 = year_q5[year_q5['Q5'].isin(['Student', 'Data Scientist', 'Data Analyst', 'Data Engineer', 'Software Engineer'])]

# # 2019
# q5_2019 = year_q5[year_q5['year'] == "2019"].reset_index(drop = True)
# q5_2019['percentage'] = q5_2019["Count"] / q5_2019["Count"].sum()
# q5_2019['%'] = np.round(q5_2019['percentage'] * 100, 1)

# # 2020
# q5_2020 = year_q5[year_q5['year'] == "2020"].reset_index(drop = True)
# q5_2020['percentage'] = q5_2020["Count"] / q5_2020["Count"].sum()
# q5_2020['%'] = np.round(q5_2020['percentage'] * 100, 1)

# # 2021
# q5_2021 = year_q5[year_q5['year'] == "2021"].reset_index(drop = True)
# q5_2021['percentage'] = q5_2021["Count"] / q5_2021["Count"].sum()
# q5_2021['%'] = np.round(q5_2021['percentage'] * 100, 1)



# year_q5_df = pd.concat([q5_2019, q5_2020, q5_2021], ignore_index = True)
# year_q5_final = pd.pivot(year_q5_df, index = "year", columns = "Q5", values = "%").reset_index()
# year_q5_final
```


```python
# year_val = year_q5_final['year'].unique().tolist()

# fig = go.Figure()

# fig.add_trace(go.Scatter(
#     x = year_val, 
#     y = year_q5_final["Data Analyst"].tolist(), 
#     mode = "lines", 
#     name = "Data Analyst",
#     line = dict(width = 0.5),
#     stackgroup = "one"
# ))

# fig.add_trace(go.Scatter(
#     x = year_val, 
#     y = year_q5_final["Data Engineer"].tolist(), 
#     mode = "lines", 
#     name = "Data Engineer",
#     line = dict(width = 0.5),
#     stackgroup = "one"
# ))

# fig.add_trace(go.Scatter(
#     x = year_val, 
#     y = year_q5_final["Data Scientist"].tolist(), 
#     name = "Data Scientist",
#     mode = "lines", 
#     line = dict(width = 0.5),
#     stackgroup = "one"
# ))

# fig.add_trace(go.Scatter(
#     x = year_val, 
#     y = year_q5_final["Software Engineer"].tolist(), 
#     name = "Software Engineer",
#     mode = "lines", 
#     line = dict(width = 0.5),
#     stackgroup = "one"
# ))

# fig.add_trace(go.Scatter(
#     x = year_val, 
#     y = year_q5_final["Student"].tolist(), 
#     name = "Student",
#     mode = "lines", 
#     line = dict(width = 0.5),
#     stackgroup = "one"
# ))

# fig.update_layout(yaxis_range = (0, 100))

# fig.show()
```

### Thank you for reading!
