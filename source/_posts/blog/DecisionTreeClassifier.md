---
title: "decisionTree Classifier()"
date: 2021-11-04 14:21:20
categories:
- DecisionTree
tags: 
- machineLearning
- decisionTree
- predictionModel
- Classifier
---

## DecisionTreeClassifier

### Classifier function 

Decision Tree **Classifier**는 데이터 집합에서 다중 클래스 분류를 수행할 수 있는 클래스이다. 

다른 분류자와 마찬가지로 Decision Tre Classifier는 훈련 샘플을 고정하는 배열 X(n_샘플, n_특징)와 훈련 샘플에 대한 클래스 레이블을 고정하는 정수 값, 형상(n_샘플, n_특징)의 배열 Y의 두 배열을 입력으로 사용합니다.


```python
from sklearn import tree
X = [[0, 0], [1, 1]]
Y = [0, 1]
clf = tree.DecisionTreeClassifier()
clf = clf.fit(X, Y)
```

이제 예측 모델을 만들어 봅시다. 


```python
clf.predict([[2., 2.]])
clf.predict_proba([[2., 2.]])
```

    array([[0., 1.]])



의사결정트리의 분류는 classification과 multiclass 양쪽으로 모두 분류 할 수 있다. 

iris dataset을 하용하면, 우리는 아래와 같은 **plot_Tree**를 만들 수 잇다. 


```python
from sklearn.datasets import load_iris
from sklearn import tree
iris = load_iris()
x, y = iris.data, iris.target
clf = tree.DecisionTreeClassifier()
clf = clf.fit(x, y)
tree.plot_tree(clf)
```



    [Text(167.4, 199.32, 'X[2] <= 2.45\ngini = 0.667\nsamples = 150\nvalue = [50, 50, 50]'),
     Text(141.64615384615385, 163.07999999999998, 'gini = 0.0\nsamples = 50\nvalue = [50, 0, 0]'),
     Text(193.15384615384616, 163.07999999999998, 'X[3] <= 1.75\ngini = 0.5\nsamples = 100\nvalue = [0, 50, 50]'),
     Text(103.01538461538462, 126.83999999999999, 'X[2] <= 4.95\ngini = 0.168\nsamples = 54\nvalue = [0, 49, 5]'),
     Text(51.50769230769231, 90.6, 'X[3] <= 1.65\ngini = 0.041\nsamples = 48\nvalue = [0, 47, 1]'),
     Text(25.753846153846155, 54.359999999999985, 'gini = 0.0\nsamples = 47\nvalue = [0, 47, 0]'),
     Text(77.26153846153846, 54.359999999999985, 'gini = 0.0\nsamples = 1\nvalue = [0, 0, 1]'),
     Text(154.52307692307693, 90.6, 'X[3] <= 1.55\ngini = 0.444\nsamples = 6\nvalue = [0, 2, 4]'),
     Text(128.76923076923077, 54.359999999999985, 'gini = 0.0\nsamples = 3\nvalue = [0, 0, 3]'),
     Text(180.27692307692308, 54.359999999999985, 'X[2] <= 5.45\ngini = 0.444\nsamples = 3\nvalue = [0, 2, 1]'),
     Text(154.52307692307693, 18.119999999999976, 'gini = 0.0\nsamples = 2\nvalue = [0, 2, 0]'),
     Text(206.03076923076924, 18.119999999999976, 'gini = 0.0\nsamples = 1\nvalue = [0, 0, 1]'),
     Text(283.2923076923077, 126.83999999999999, 'X[2] <= 4.85\ngini = 0.043\nsamples = 46\nvalue = [0, 1, 45]'),
     Text(257.53846153846155, 90.6, 'X[1] <= 3.1\ngini = 0.444\nsamples = 3\nvalue = [0, 1, 2]'),
     Text(231.7846153846154, 54.359999999999985, 'gini = 0.0\nsamples = 2\nvalue = [0, 0, 2]'),
     Text(283.2923076923077, 54.359999999999985, 'gini = 0.0\nsamples = 1\nvalue = [0, 1, 0]'),
     Text(309.04615384615386, 90.6, 'gini = 0.0\nsamples = 43\nvalue = [0, 0, 43]')]


    
![DecisionTreeClassifier](/imeges/DT.png)
    


decision tree는 약 20여종의 parameter가 있다.



[Parameter](https://scikit-learn.org/stable/modules/generated/sklearn.tree.plot_tree.html#sklearn.tree.plot_tree)

* criterion : 분할 품질을 측정하는 기능 (default : gini)
* splitter : 각 노드에서 분할을 선택하는 데 사용되는 전략 (default : best)
* max_depth : 트리의 최대 깊이 (값이 클수록 모델의 복잡도가 올라간다.)
* min_samples_split : 자식 노드를 분할하는데 필요한 최소 샘플 수 (default : 2)
* min_samples_leaf : 리프 노드에 있어야 할 최소 샘플 수 (default : 1)
* min_weight_fraction_leaf : min_sample_leaf와 같지만 가중치가 부여된 샘플 수에서의 비율
* max_features : 각 노드에서 분할에 사용할 특징의 최대 수
* random_state : 난수 seed 설정
* max_leaf_nodes : 리프 노드의 최대수
* min_impurity_decrease : 최소 불순도
* min_impurity_split : 나무 성장을 멈추기 위한 임계치
* class_weight : 클래스 가중치
* presort : 데이터 정렬 필요 여부




```python
sklearn.tree.plot_tree(decision_tree, *, max_depth=None, feature_names=None, class_names=None, label='all', filled=False, impurity=True, node_ids=False, proportion=False, rounded=False, precision=3, ax=None, fontsize=None)
tree.plot_tree()
```

가장 원시적이면서 기본적인 parameter가 있는 decision tree의 parameter를 외우기 보다 지금 어떤 형태로 코드가 들어 가는지에 집중하자. 


왜냐하면, 최신의 버전은 decision tree가 아니기 때문.


```python
from sklearn.datasets import load_iris
from sklearn import tree

clf = tree.DecisionTreeClassifier(random_state=0)
iris = load_iris()

```
