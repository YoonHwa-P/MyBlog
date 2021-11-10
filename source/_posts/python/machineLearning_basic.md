---
title: machine Learning_basic
date: 2021-11-04 12:21:20
categories:
- DecisionTree
tags: 
- machineLearning
- decisionTree
- predictionModel
---

# machine learning 
## Decision tree learing

<br>
<hr>

- Decision tree 란

관측값과 목표값을 연결 시켜주는 예측 모델

통계학과 데이터 마이닝, 기계학습에서 예측모델링으로 사용하는 방법

데이터 기능에서 유추된 결정규칙을 학습하여 대상 변수값을 예측 하는 모델을 만들기 위해 사용


####종류
 - 분류 트리 
   - 변수가 유한한 수의 값을 가지는 것, **클래스 출력**
   - Leaf node는 클래스 라벨을 나타내고 가지는 클래스 라벨과 관련있는 특징들의 논리 곱을 나타낸다. 
 - 회귀트리 
   - 목표변수가 연속하는 값(일반적으로 실수 )을 가지는 트리
   - **특정 의미를 가지는 실수값**을 출력

    의사결정 분석에서 결정트리는 시각적으로 명시적인 방법으로 과정을 보여준다.     


#### 결정트리의 학습
- 결정 트리의 학습 : 자료 집합을 적절한 분할기준 또는 분할 테스트에 따라 부분집합들로 나누는 과정 
  - 하향식 결정 트리 귀납법 (TDIDT_top-down induction of decision trees) 
    - 순환분할 방식으로 나눠진 자료의 부분집합에 재귀적으로 반복됨.
    - 분할로 인해 더이상 새로운 예측값이 추가 되지 않거나 부분집합의 노드가 목표변수와 같은 값을 지닐대 까지 계속됨.

- 데이터 마이닝에서 결정트리 
  - 수학 적으로 표현됨 
    
예를 들어 아래와 같은 데이터 마이닝에서의 결정 트리가 있다고 가정 해 보자. 

    ( f{x},Y ) = (x_1, x_2, x_3, ..., x_k, Y)

    종속 변수 Y는 분류를 통해 학습하고자 하는 목표 변수이며, 
    벡터 x는 {  x_{1}, x_{2}, x_{3} } x_1, x_2, x_3 등의 
    입력 변수로 구성된다.
 
## 장점
    1. 이해하기 쉬우며 시각화 가능
    2. data 준비가 거의 필요하지 않음
    3. 수치형, 범주형 data를 모두 처리 가능
    4. multi-output problems를 다룰 수 있다. 
    5. a white box model을 사용 할 수 있다. (bool 가능)
    6. 통계 검정을 사용하여 모형을 검증하기 때문에 모델의 신뢰성을 설명할 수 있다.
    7. 생성된 데이터가 실제 모형에 의해 가정이 다소 위반되더라도 잘 수행된다.

## 단점

1. data 일반화가 잘 되지 못하면 복잡한 트리가 만들어짐 (과적합)
        - 가지치기, 리프노드에 필요한 샘플 최소화, 트리 최대깊이 설정 으로 해결 가능
2. variation이 작은 경우 의사결정트리가 불안정 할 수 있다. 
    - 앙상블(ensemble) 내에서 의사결정 트리 사용으로 해결 가능.
    - 실용적인 의사 결정 트리 학습 알고리즘은 각 노드에서 국소적으로 최적의 의사결정이 이루어지는 그리디 알고리즘과 같은 경험적 알고리즘을 기반으로 한다. 이러한 알고리즘은 전역 최적 의사 결정 트리를 반환한다고 보장할 수 없다. 
    - 이는 특징과 샘플이 교체와 함께 무작위로 샘플링되는 앙상블 학습기에서 여러 트리를 훈련시킴으로써 완화될 수 있다.
3. 의사 결정 트리의 예측은 근사치이기 때문에 좋은 추정은 아닐 수 있다.
4. 의사결정 트리의 최적화의 문제는 NP-complete로 잘 알려진 문제이다.
   잘 모르겠다.
- [NP-complete](https://ko.wikipedia.org/wiki/NP-%EC%99%84%EC%A0%84)
 
5. XOR, parity or multiplexer problems와 같이 배우기 쉽지 않은 컨셉들 때문에 표현하기 쉽지않다. 
6. 학습자가 편향된 트리르르 만들 수 있으므로, 데이터 세트의 균형을 맞춰줘야 한다. 


<br>
<hr>
<br>

#### 이어지는 posting
- [classification](https://yoonhwa-p.github.io/2021/11/04/DecisionTreeClassifier/)
- [Regression]()

---
Ref.

1) [결정트리학습법/wiki](https://ko.wikipedia.org/wiki/%EA%B2%B0%EC%A0%95_%ED%8A%B8%EB%A6%AC_%ED%95%99%EC%8A%B5%EB%B2%95)
2) [결정트리 국문](https://injo.tistory.com/15)
3) [Decusuib Trees](https://scikit-learn.org/stable/modules/tree.html)
4) [1.10.1](https://scikit-learn.org/stable/modules/tree.html#tree-classification) 
5) [1.10.2](https://scikit-learn.org/stable/modules/tree.html#tree-regression)