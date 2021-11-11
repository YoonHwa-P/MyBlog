---
title: "to prepare kaggle Competition"
date: 2021-11-11 22:12:20
categories:
- python
    - plotly
tags:
- kaggle
- summary
- kaggle_dictation
---


#Kaggle Competition 준비하기
### Kaggle Note 에서 작성됨.
1. files and Library import


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

result

`/kaggle/input/kaggle-survey-2018/SurveySchema.csv
/kaggle/input/kaggle-survey-2018/freeFormResponses.csv
/kaggle/input/kaggle-survey-2018/multipleChoiceResponses.csv
/kaggle/input/kaggle-survey-2017/freeformResponses.csv
/kaggle/input/kaggle-survey-2017/schema.csv
/kaggle/input/kaggle-survey-2017/RespondentTypeREADME.txt
/kaggle/input/kaggle-survey-2017/multipleChoiceResponses.csv
/kaggle/input/kaggle-survey-2017/conversionRates.csv
/kaggle/input/kaggle-survey-2020/kaggle_survey_2020_responses.csv
/kaggle/input/kaggle-survey-2020/supplementary_data/kaggle_survey_2020_methodology.pdf
/kaggle/input/kaggle-survey-2020/supplementary_data/kaggle_survey_2020_answer_choices.pdf
/kaggle/input/kaggle-survey-2021/kaggle_survey_2021_responses.csv
/kaggle/input/kaggle-survey-2021/supplementary_data/kaggle_survey_2021_methodology.pdf
/kaggle/input/kaggle-survey-2021/supplementary_data/kaggle_survey_2021_answer_choices.pdf
/kaggle/input/kaggle-survey-2019/survey_schema.csv
/kaggle/input/kaggle-survey-2019/multiple_choice_responses.csv
/kaggle/input/kaggle-survey-2019/other_text_responses.csv
/kaggle/input/kaggle-survey-2019/questions_only.csv`


2. dataframe create

```python
df17= pd.read_csv("/kaggle/input/kaggle-survey-2017/multipleChoiceResponses.csv", encoding="ISO-8859-1")
df18= pd.read_csv("/kaggle/input/kaggle-survey-2018/multipleChoiceResponses.csv", )
df19= pd.read_csv("/kaggle/input/kaggle-survey-2019/multiple_choice_responses.csv", )
df20= pd.read_csv("/kaggle/input/kaggle-survey-2020/kaggle_survey_2020_responses.csv", )
df21= pd.read_csv("/kaggle/input/kaggle-survey-2021/kaggle_survey_2021_responses.csv", )
```

3. data 확인하기

```python
df21 = df21.iloc[1:, :]
#df21.value_counts()
#df21.count

df21.head()
```

![df21.head](/imeges/kgg/df21.head.png)


4. data 1개씩 표로 만들어서 불러오기

```python
country = (
    df21['Q3']
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'country', 'Q3':'Count'})
    .sort_values(by=['country'], ascending=False)
     )  

Wage = (
    df21['Q25']
    .value_counts()
    .to_frame()
    .reset_index()
    .rename(columns={'index':'Wage', 'Q25':'Count'})
    .sort_values(by=['Wage'], ascending=True)
     )  

Wage
```


### Problem 

나라별 임금을 확인 하기 위해 
temp에 나라이름과 임금에 대해 넣었다. 
C_ls : country list라는 List를 만들어 나라 별로 임금을 뽑을 수 있었다.

```python
# India, Russia, China
temp = df21[['Q3','Q25']]

print(temp['Q3'].unique())

C_Ls = temp['Q3'].unique() #나라이름 뽑아주기
```

![Unique함수](/imeges/kgg/Unique함수.png)


```python

tempR = temp[temp['Q3'] == str(C_Ls[0])].value_counts()


# 이 경우에는 temp가 가공 할 수 없는 templet이기 때문에 plot을 그릴 수가 없다. 
```

![temp_noncalculable](/imeges/kgg/temp_noncalculable.png)


<br><br><br>

### trouble shooting

```python
#선생님
temp= df21[['Q3','Q25']]
temp = temp.groupby(['Q3','Q25'])
        .size()
        .reset_index()
        .rename(columns = {0:"Count"})

C_Ls = temp['Q3'].unique()

for i in range(1,len(df21['Q3'])):
    temp2 = temp[temp['Q3'] == str(C_Ls[i])]
    fig = px.bar(temp2, x='Q25', y='Count',title=C_Ls[i])
    fig.show()
```

선생님의 도움을 받아 for문 안에 나라 이름을 넣어서 

모든 나라의 임금을 확인 할 수 있었다. 

모든 나라의 임금을 확인 했는데, 차별이 심한 나라를 찾기 힘들었다. 

뭐때문에 이 graph를 그리려고 했는지 잃어버렸다. 

1. 한국에서 돈 잘 벌려면 어떤 직업, 어떤 ... 을 해야 하는가 
2. 한국보다 평균임금이 더 많은 나라는?
3. 한국 보다 평균임금이 더 많은 나라는 뭐가 다를까?
4. 임금에 대한 것을 버려야 할까...

이런 재미있는 Issue에 대해 Note를 만들어 보려고 했는데 ㅎㅎ

코딩 실력이 안된당 흐흐 대회에서 잘 하려는 마음보다는 코드를 더 잘 읽고, 쓰는데 집중하자. ㅠㅠ

D-17 (대회 종료까지)

