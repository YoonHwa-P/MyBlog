---
title: "NLP_text_classification"
date: 2021-11-11 23:17:56
categories:
- python
    - plotly
tags:
- kaggle
- summary
- kaggle_dictation
---

##Kaggle _ API
1. !pip Install Kaggle : Kaggle 설치
2. google.colab에 kaggle.json files upload
   1. Saving kaggle.json to kaggle.json 
   2. User uploaded file "kaggle.json" with length 66 bytes
   3. kaggle.json file에는 뭐가 들어있을까 너무 궁금하다. 
3. !kaggle competitions download -c nlp-getting-started
   1. 케글 대회 자료를 다운받기. (-c nlp-getting-started 이게 뭘까)
4. data path 설정하기


##data 둘러보기
1. data frame을 만들기 위해 Pandas와 numpy를  import후 각 file을 data set을 Load해 준다. 
2. data set 확인
   1. .head()로 대략적인 data set 확인
   2. .shape로 각 data set의 크기 확인
   3. .info()로 각 data frame의 정보 확인

##EDA
[EDA](https://eda-ai-lab.tistory.com/13)

: 수집한 data를 다양한 각도에서 관찰하고 이해하는 과정

: 통계적 방법으로 자료를 직관적으로 바라보는 과정

- data의 분포 및 값을 검토함으로써 데이터가 표현하는 현상을 더 잘 이해하고, 잠재적 문제를 발견 할 수 있다. 
- 문제를 발견하여 기존의 가설을 수정하거나 새로운 가설을 세울 수 있다. 
- 

1. data visualiztion을 위해 matplotlib.pyplot과 seaborn 설치
   1. missing_colunms = ["keyword", "location"]
   2. 각 data set에서 null인 columns를 가져온다. 
2. matplotlib.pyplot으로 bar plot 그리기
![MP_EDA](/imeges/kgg/MP_EDA.png)
3. 

##Feature Engineering
##Mideling
##algorithm logistic regression