---
title: "DecisionTreeMachineLearning(03)"
date: 2021-12-10 17:50:48
categories:
- python
- machineLeaning
tags:
- Study
- python
- machineLeaning
- Decision Tree 
---

## machine Learning Model Algoridms

- 비 선형 모델  : KNN, 
- 선형 모델 : 

<br><br>

---

Decision Tree MachineLearning

![ML_DecisionTree01](/../../imeges/python/ML_DecisionTree01.png)

### Introduction 

 - 과적합 : 모델의 정확도만 높이기 위해 분류 조건(depth)만 강조하여 실제 상황에서 유연하게 대처하는 능력이 떨어지게 되는 문제가 발생하게 되는것.
 - 가지치기(pruning)을 통해 유연성을 유지. 
   - Max_depth를 대략적으로 잡아서 (3, 5, 10...) RMS 값 비교
   - Random search
   - 하이퍼파라미터 (grid Search)
 
<br><br>

 - 분류기준 (수식은 아래서 책에서 확인)
   1. 정보이득 : 
      - 자식노드의 불순도가 낮을 수록 정보의 이득이 커진다.(효율성 Up)
      1. 엔트로피의 정의 :
         - 엔트로피는 높을 수록 좋다.  
      2. 지니불순도 :
         - 순도는 높을 수록 좋다. 
      3. 분류오차 :
         - 어떤 시나리오가 더 좋은가에 대한 계산
         - 1이 되면 균등, 완벽하게 나누어 졌다고 

 - ㅇㅇ

![PythonMacnineLeanting_equ](/../../imeges/python/PythonMacnineLeanting_equ.png)

공식은 이쪽에 가면 있다. 

---

계산은 컴퓨터가 다 해준다. 

우리는 보고 좋은 분류 기준을 선택 하며 됩니다. 

#### 분류기준 1. 분류 오차 

![PythonMacnineLeanting_E01](/../../imeges/python/PythonMacnineLeanting_E01.png)

#### 분류기준 2. 지니 불순도 

![PythonMacnineLeanting_E02](/../../imeges/python/PythonMacnineLeanting_E02.png)

#### 분류기준 2. 엔트로피

![PythonMacnineLeanting_E03](/../../imeges/python/PythonMacnineLeanting_E03.png)



