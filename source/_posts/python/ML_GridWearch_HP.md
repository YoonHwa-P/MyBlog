---
title: "DTS: ML_Grid search(Hyper Parameter)"
date: 2021-12-25 08:40:13
categories:
- python
- machineLeaning
tags:
- python
- machineLeaning
- DTS
- HyperParameter
---

 § 이전 posting

☞ [PipeLine](https://yoonhwa-p.github.io/2021/12/22/python/DTS_PipeLine/)

☞ [Learning curve](https://yoonhwa-p.github.io/2021/12/24/python/ML_LearningCurveG(01)/)

---

## ML pipeLine 검증 곡선 그리기 

- ML 그리드 서치 
  + grid search를 이용한 파이프라인(pipeLine) 설계및 
  하이퍼 파라미터 튜닝(hyper parameter)
  - 그리드 서치와 랜덤 서치가 있다. 
    - 랜덤 서치로 먼저 뽑아 낸 후 그리드 서치를 이용하여 안정적으로 서치 ! 

- 나도 공부 하기 싫으닌까 그냥 <a style="color:#C6563B;font-size:11 0%;"> 
남 </a> 이 하는거 따라 쓰고 싶다. 

 <p style="color:#C6563B;font-size:150%;"> 
남 : Kaggle competition </p>

<br><br><br>

---



```python
import pandas as pd 
from sklearn.preprocessing import LabelEncoder
from sklearn.model_selection import train_test_split
import numpy as np
from sklearn.model_selection import StratifiedKFold
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt 
from sklearn.model_selection import learning_curve
from sklearn.model_selection import validation_curve
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import GridSearchCV 
from sklearn.svm import SVC 

data_url = 'https://archive.ics.uci.edu/ml/machine-learning-databases/breast-cancer-wisconsin/wdbc.data'
column_name = ['id', 'diagnosis', 'radius_mean', 'texture_mean', 'perimeter_mean', 'area_mean', 'smoothness_mean', 'compactness_mean', 'concavity_mean', 
               'concave points_mean', 'symmetry_mean', 'fractal_dimension_mean', 'radius_se', 'texture_se', 'perimeter_se', 'area_se', 'smoothness_se', 
               'compactness_se', 'concavity_se', 'concave points_se', 'symmetry_se', 'fractal_dimension_se', 'radius_worst', 'texture_worst', 'perimeter_worst', 
               'area_worst', 'smoothness_worst', 'compactness_worst', 'concavity_worst', 'concave points_worst', 'symmetry_worst', 'fractal_dimension_worst']

df = pd.read_csv(data_url, names=column_name)

X = df.loc[:, "radius_mean":].values
y = df.loc[:, "diagnosis"].values

le = LabelEncoder()
y = le.fit_transform(y)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.20, 
                                                    # stratify = y, 
                                                    random_state=1)
kfold = StratifiedKFold(n_splits = 10, random_state=1, shuffle=True)

pipe_tree = make_pipeline(StandardScaler(), 
                          PCA(n_components=2), 
                          DecisionTreeClassifier(random_state=1))
```



```python
# 이 Line이 핵쉼 !!

# estimator.get_params().keys()
# pipe_tree.get_params().keys() ---> 이렇게 씀. 

print(pipe_tree.get_params().keys())
param_grid = [{"decisiontreeclassifier__max_depth": [1, 2, 3, 4, 5, 6, 7, None]}]

gs = GridSearchCV(estimator = pipe_tree, 
                  param_grid = param_grid, 
                  scoring="accuracy", 
                  cv = kfold)

gs = gs.fit(X_train, y_train)
print(gs.best_score_)
print(gs.best_params_)

clf = gs.best_estimator_
# 자동으로 제일 좋은 것을 뽑아서 알려줌.
clf.fit(X_train, y_train) 
print("테스트 정확도:", clf.score(X_test, y_test))
```


>dict_keys(['memory', 'steps', 'verbose', 'standardscaler', 'pca', 'decisiontreeclassifier', 'standardscaler__copy', 'standardscaler__with_mean', 'standardscaler__with_std', 'pca__copy', 'pca__iterated_power', 'pca__n_components', 'pca__random_state', 'pca__svd_solver', 'pca__tol', 'pca__whiten', 'decisiontreeclassifier__ccp_alpha', 'decisiontreeclassifier__class_weight', 'decisiontreeclassifier__criterion', 'decisiontreeclassifier__max_depth', 'decisiontreeclassifier__max_features', 'decisiontreeclassifier__max_leaf_nodes', 'decisiontreeclassifier__min_impurity_decrease', 'decisiontreeclassifier__min_samples_leaf', 'decisiontreeclassifier__min_samples_split', 'decisiontreeclassifier__min_weight_fraction_leaf', 'decisiontreeclassifier__random_state', 'decisiontreeclassifier__splitter'])
0.927536231884058
{'decisiontreeclassifier__max_depth': 7}
테스트 정확도: 0.9210526315789473
> 


### svc를 이용한 hyperparameter tuenning

```python
import pandas as pd 
from sklearn.preprocessing import LabelEncoder
from sklearn.model_selection import train_test_split
import numpy as np
from sklearn.model_selection import StratifiedKFold
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt 
from sklearn.model_selection import learning_curve
from sklearn.model_selection import validation_curve
from lightgbm import LGBMClassifier
from sklearn.model_selection import GridSearchCV 
from sklearn.svm import SVC 

data_url = 'https://archive.ics.uci.edu/ml/machine-learning-databases/breast-cancer-wisconsin/wdbc.data'
column_name = ['id', 'diagnosis', 'radius_mean', 'texture_mean', 'perimeter_mean', 'area_mean', 'smoothness_mean', 'compactness_mean', 'concavity_mean', 
               'concave points_mean', 'symmetry_mean', 'fractal_dimension_mean', 'radius_se', 'texture_se', 'perimeter_se', 'area_se', 'smoothness_se', 
               'compactness_se', 'concavity_se', 'concave points_se', 'symmetry_se', 'fractal_dimension_se', 'radius_worst', 'texture_worst', 'perimeter_worst', 
               'area_worst', 'smoothness_worst', 'compactness_worst', 'concavity_worst', 'concave points_worst', 'symmetry_worst', 'fractal_dimension_worst']

df = pd.read_csv(data_url, names=column_name)

X = df.loc[:, "radius_mean":].values
y = df.loc[:, "diagnosis"].values

le = LabelEncoder()
y = le.fit_transform(y)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.20, 
                                                    # stratify = y, 
                                                    random_state=1)

pipe_svc = make_pipeline(StandardScaler(), 
                        PCA(n_components=2), 
                        SVC(random_state=1))

param_range = [0.0001, 0.001, 0.01, 0.1, 1.0, 10.0, 100.0, 1000.0]
param_grid = [{"svc__C": param_range, 
               "svc__gamma": param_range, 
               "svc__kernel": ["linear"]}]

gs = GridSearchCV(estimator = pipe_svc, 
                  param_grid = param_grid, 
                  scoring="accuracy", 
                  cv = 10)

gs = gs.fit(X_train, y_train)
print(gs.best_score_)
print(gs.best_params_)

clf = gs.best_estimator_
clf.fit(X_train, y_train) 
print("테스트 정확도:", clf.score(X_test, y_test))
```

효효효 

---

