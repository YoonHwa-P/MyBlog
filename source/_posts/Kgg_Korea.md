---
title: "kaggle in Korea(data둘러보기)"
date: 2021-11-12 16:10:27
categories:
- python
    - plotly
tags:
- kaggle
- Plotly
- plot
- kaggle_dictation
---
#How popular is kaggle in South Korea?
<br><br>
<hr>

## data 정제하기 

```python
df21_Ko = df21[df21['Q3'] == 'South Korea']
df21_Wo = df21[~(df21['Q3'] == 'South Korea')]
df21['region']=["Korea" if x == 'South Korea' else "World" for x in df21['Q3']]
df21['region'].value_counts()

df20_Ko = df20[df20['Q3'] == 'South Korea']
df20_Wo = df20[~(df20['Q3'] == 'South Korea')]
df20['region']=["Korea" if x == 'South Korea' else "World" for x in df20['Q3']]
df20['region'].value_counts()

df19_Ko = df19[df19['Q3'] == 'South Korea']
df19_Wo = df19[~(df19['Q3'] == 'South Korea')]
df19['region']=["Korea" if x == 'South Korea' else "World" for x in df19['Q3']]
df19['region'].value_counts()

df18_Ko = df18[df18['Q3'] == 'South Korea']
df18_Wo = df18[~(df18['Q3'] == 'South Korea')]
df18['region']=["Korea" if x == 'South Korea' else "World" for x in df18['Q3']]
df18['region'].value_counts()

df17_Ko = df17[df17['Country'] == 'South Korea']
df17_Wo = df17[~(df17['Country'] == 'South Korea')]
df17['region']=["Korea" if x == 'South Korea' else "World" for x in df17['Country']]
df17['region'].value_counts()
```


World    25615
Korea      359
Name: region, dtype: int64

World    19847
Korea      190
Name: region, dtype: int64


World    19536
Korea      182
Name: region, dtype: int64


World    23672
Korea      188
Name: region, dtype: int64


World    16522
Korea      194
Name: region, dtype: int64