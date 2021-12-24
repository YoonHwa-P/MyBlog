---
title: "DTS: PipeLine 만들고 활용하기"
date: 2021-12-22 12:15:10
categories:
- python
- machineLeaning
tags:
- python
- machineLeaning
- sklearn
---

§ 다음 posting

☞ [PipeLine](https://yoonhwa-p.github.io/2021/12/24/python/ML_ValidationCurveG(01)/)

☞ [Learning curve](https://yoonhwa-p.github.io/2021/12/24/python/ML_LearningCurveG(01)/)

---

## sklearn.pipeline.Pipeline

- class sklearn.pipeline.Pipeline(steps, *, memory=None, verbose=False)

- data : [ref](https://archive.ics.uci.edu/ml/index.php)

---

Model을 바로 확인 하기 어렵다. 

과대적합 하는지 확인 하기 위해 pipeLine을 이용하여 쉽게 파악 할 수 있다.

mlops? 때문이다. 

---

[sklearn.pipeline](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)

- pipeLine :  최종 추정을 위한 변환 파이프라인
- 매개변수를 바꿔가며 교차 검증 할 수 있는 여러 단계를 묶어 놓아 하나의 함수로 만들어 사용하기 쉽게 한 것.
- 해당 이름의 매개 변수를
[chaining estimators](https://scikit-learn.org/stable/modules/compose.html#pipeline) 을 위해 설정하거나, 
제거 할 수 있다. 
  - convenience and encapsulation
  - joint parameter selection
  - safety


### 뭘 한건지 모르겠지만, 오늘 할 것 정리 해 보자 .

```python
import pandas as pd
from sklearn.preprocessing import LabelEncoder
from sklearn.model_selection import train_test_split
from sklearn.model_selection import StratifiedKFold
import numpy as np
```

- 일단 sklearn을 이용한 ML을 하기 위해 library를 import 해 보자.


--- 

### data 불러오기

```python
data_url = 'https://archive.ics.uci.edu/ml/machine-learning-databases/breast-cancer-wisconsin/wdbc.data'
column_name = ['id', 'diagnosis', 'radius_mean', 'texture_mean', 'perimeter_mean', 'area_mean', 'smoothness_mean', 'compactness_mean', 'concavity_mean',
               'concave points_mean', 'symmetry_mean', 'fractal_dimension_mean', 'radius_se', 'texture_se', 'perimeter_se', 'area_se', 'smoothness_se',
               'compactness_se', 'concavity_se', 'concave points_se', 'symmetry_se', 'fractal_dimension_se', 'radius_worst', 'texture_worst', 'perimeter_worst',
               'area_worst', 'smoothness_worst', 'compactness_worst', 'concavity_worst', 'concave points_worst', 'symmetry_worst', 'fractal_dimension_worst']

df = pd.read_csv(data_url, names=column_name)
print(df.info())
```

---

### test, Train 나누기

```python
X = df.loc[:, "radius_mean":].values
y = df.loc[:, "diagnosis"].values

le = LabelEncoder()
y = le.fit_transform(y)
print("종속변수 클래스:", le.classes_)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.20, stratify = y, random_state=1)
```
<br><br><br>
___

### 이 코드 하나가 pipe Line

- LogisticRegression

```python
from sklearn.linear_model import LogisticRegression
pipe_lr = make_pipeline(StandardScaler(),
                        PCA(n_components=2),
                        LogisticRegression(solver="liblinear", random_state=1))
```

![PipeLine_LR](/../../imeges/python/PipeLine_LR.png)

<br><br>

- DecisionTreeClassifier

```python
from sklearn.tree import DecisionTreeClassifier
pipe_lr = make_pipeline(StandardScaler(),
                        PCA(n_components=2),
                        DecisionTreeClassifier(random_state=0))
```

![PipeLine_DTC](/../../imeges/python/PipeLine_DTC.png)

<br><br>
- LGBM


```python
from lightgbm import LGBMClassifier
pipe_lr = make_pipeline(StandardScaler(),
                        PCA(n_components=2),
                        LGBMClassifier(objective='multiclass', random_state=5))

```

- LGBMC :  이거 아닌거같은데 못봤다. 안됨 여튼 

- 이런식으로 바꿔 끼워가며 확인 할 수 있다. 

<br><br>


---

### pipeLine만들기

```python
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

pipe_lr = make_pipeline(StandardScaler(),
                        PCA(n_components=2),
                        LogisticRegression(solver="liblinear", random_state=1))

kfold = StratifiedKFold(n_splits = 10, random_state=1, shuffle=True).split(X_train, y_train)
scores = []
for k, (train, test) in enumerate(kfold):
  pipe_lr.fit(X_train[train], y_train[train])
  score = pipe_lr.score(X_train[test], y_train[test])
  scores.append(score)
  print("폴드: %2d, 클래스 분포: %s, 정확도: %.3f" % (k+1, np.bincount(y_train[train]), score))

print("\nCV 정확도: %.3f +/- %.3f" % (np.mean(scores), np.std(scores)))

from sklearn.model_selection import cross_val_score
scores = cross_val_score(estimator=pipe_lr,
                         X = X_train,
                         y = y_train,
                         cv = 10,
                         n_jobs = 1)

print("CV 정확도 점수 : %s" % scores)
print("CV 정확도 : %.3f +/- %.3f" % (np.mean(scores), np.std(scores)))
```


- Kaggle data랑 뭐가 다른지 확인 해 보라고 하는데
- Kaggle에서 어디 있는지 잘 모르겠다. 
- 자바는 어느정도 감이 왔는데 python은 당최 아얘 감조차 안온다. 
- 그냥 python 강의나 들어야 하나 고민중 ... 
