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



```python
fig = go.Figure()
y=[len(df17_Ea),len(df18_Ea), len(df19_Ea),len(df20_Ea),len(df21_Ea)]
fig.add_trace(go.Bar(x=years, y=y,
                base=0,
                marker_color='#D9946C',
                      yaxis = "y1",
                name='East Asia',
                text= percent,
                texttemplate='%{text}  %', 
                  textposition='outside',
                  hovertemplate='<b>KaggleUser</b>: %{x}<br>'+
                                '<b>Count</b>: %{y}',
                  textfont_size=14
                ))
fig.add_trace(
    go.Scatter(name = "World",
           x=years, 
            y=[len(df17), len(df18), len(df19), len(df20), len(df21)],
               marker_color='#88BFBA',
           mode = 'lines+markers', # please check option here
           yaxis = "y2"))

fig.update_layout(yaxis  = dict(title = "KaggleUser in World", showgrid = False, range=[0, len(df21_Ea)*1.2]),
                  yaxis2 = dict(title = "KaggleUser in East Asia", overlaying = "y1", side = "right", showgrid = False, 
                                zeroline = False, 
                                range=[0, len(df21)*1.2]), # This code solves the different zero set but with same zero values.
                  template = "plotly_white", height=500, width=700,)

fig.show()
```

![vbar_scatters](../imeges/kgg/vbar_scatters.png)

