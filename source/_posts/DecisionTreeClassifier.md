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




# DecisionTreeClassifier


##Classifier function 

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
