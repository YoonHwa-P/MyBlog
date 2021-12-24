---
title: "DTS: ML_Learning CurveG(01)"
date: 2021-12-24 10:44:36
categories:
- python
- machineLeaning
tags:
- python
- machineLeaning
- DTS
---

§ 이전 posting

☞ [PipeLine](https://yoonhwa-p.github.io/2021/12/22/python/DTS_PipeLine/)

§ 다음 posting

☞ [PipeLine](https://yoonhwa-p.github.io/2021/12/22/python/DTS_ValidationCurbeG(01)/)

---

## Learning curve 그리기 

- pipeLine 이용하여 ML 돌림
- 이후 ML 을 확인 하기 위해 Learning, validation curve를 그려 확인
- 일반적으로 두 curve 를 함께 그린다. 


<br><br><br>

---

### data 불러오기, 훈련 세트 분리, 교차검증 정의 

```python
import pandas as pd 
from sklearn.preprocessing import LabelEncoder
from sklearn.model_selection import train_test_split

data_url = 'https://archive.ics.uci.edu/ml/machine-learning-databases/breast-cancer-wisconsin/wdbc.data'
column_name = ['id', 'diagnosis', 'radius_mean', 'texture_mean', 'perimeter_mean', 'area_mean', 'smoothness_mean', 'compactness_mean', 'concavity_mean', 
               'concave points_mean', 'symmetry_mean', 'fractal_dimension_mean', 'radius_se', 'texture_se', 'perimeter_se', 'area_se', 'smoothness_se', 
               'compactness_se', 'concavity_se', 'concave points_se', 'symmetry_se', 'fractal_dimension_se', 'radius_worst', 'texture_worst', 'perimeter_worst', 
               'area_worst', 'smoothness_worst', 'compactness_worst', 'concavity_worst', 'concave points_worst', 'symmetry_worst', 'fractal_dimension_worst']

df = pd.read_csv(data_url, names=column_name)
print(df.info())

X = df.loc[:, "radius_mean":].values
y = df.loc[:, "diagnosis"].values

le = LabelEncoder()
y = le.fit_transform(y)
print("종속변수 클래스:", le.classes_)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.20, stratify = y, random_state=1)

from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

pipe_lr = make_pipeline(StandardScaler(), 
                        PCA(n_components=2), 
                        LogisticRegression(solver="liblinear", random_state=1))

```

<br><br><br>

---

### Learning curve 결과 값 구하기 

```python
from sklearn.model_selection import learning_curve
import numpy as np

train_sizes, train_scores, test_scores = learning_curve(
    estimator = pipe_lr, 
    X = X_train, 
    y = y_train, 
    train_sizes = np.linspace(0.1, 1.0, 10), 
    cv = 10
)

train_mean = np.mean(train_scores, axis = 1)
train_std = np.std(train_scores, axis = 1)
test_mean = np.mean(test_scores, axis = 1)
test_std = np.std(test_scores, axis = 1)

print("mean(test)-----------------\n", train_mean,"\n mean(train)-----------------\n",test_mean )

print("STD(test)-----------------\n", train_std,"\n STD(train)-----------------\n",test_std )
```

>mean(test)-----------------  
 [0.9525     0.96049383 0.93032787 0.92822086 0.93382353 0.93469388  
 0.94090909 0.94740061 0.94945652 0.95378973]   
 mean(train)-----------------  
 [0.92763285 0.92763285 0.93415459 0.93415459 0.93855072 0.94516908  
 0.94956522 0.947343   0.94516908 0.94956522]  
STD(test)-----------------  
 [0.0075     0.00493827 0.00839914 0.01132895 0.00395209 0.00730145  
 0.00862865 0.0072109  0.00656687 0.00632397]   
 STD(train)-----------------  
 [0.0350718  0.02911549 0.02165313 0.02743013 0.02529372 0.02426857  
 0.0238436  0.02421442 0.02789264 0.02919026]  
> 



### Learning Curve Graph

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize = (8,5))
ax.plot(train_sizes, 
        train_mean, 
        color = "blue", 
        marker = "o", 
        markersize = 10, 
        label = "training acc.")
ax.fill_between(train_sizes, 
                train_mean + train_std, 
                train_mean - train_std, 
                alpha = 0.15, color = "darkblue")

ax.plot(train_sizes,
        test_mean, color = "green",
        marker = "s",
        linestyle = "--", # 점선으로 표시
        markersize = 10,
        label = "testing acc.")

ax.fill_between(train_sizes, 
                test_mean + test_std, 
                test_mean - test_std, 
                alpha = 0.15, color = "salmon")
plt.grid()
plt.xlabel("Number of training samples")
plt.ylabel("Accuracy")
plt.legend(loc = "lower right")
plt.ylim([0.8, 1.03])
plt.tight_layout()
plt.show

# sample 수가 많아지면, 점점 가까워 진다. 
```


![ML_Learning_Curve](/../../imeges/python/ML_Learning_Curve.png)


---

분야 좋은데 인거 알겠고, 재미있는데 참 ㅎㅎ 

<br><br><br>


---
