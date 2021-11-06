---
title: "Study Pandas"
date: 2021-11-02 15:11:02
tags: 
- Study 
- Pandas
---


# Pandas 



<br>
<hr>

## 판다스란



pandas는 데이터를 시각화 하기 좋은 python base의 한 라이브러리이다.

python을 이용한 data분석과 같은 작업에서 필수적으로 쓰이고 있다.

아나콘다와 같은 IDE를 이용하여 작업 할 수도 있지만, 기본적으로 Import하여 편하게 사용 할 수 있다.

<br>

판다스의 이름은 계량 경제학에서 사용되는 용어인 **'PANel DAta'**의 앞 글자를 따서 지어졌다. 
판다스는 R에서 사용되던 data.frame 구조를 본뜬 DataFrame이라는 구조를 사용하기 때문에,
  R의 data.frame에서 사용하던 기능 상당수를 무리없이 사용할 수 있도록 만들었다. 
더욱이 파이썬이라는 접근성이 좋은 언어 기반으로 동작하기 때문에 데이터 분석을 파이썬으로 입문하는 
사람들이 필수적으로 사용하는 라이브러리가 되었다.’

<br>

## 판다스 Import 하기

쥬피터 노트로 설치가 가능 하다고 하지만, 구글코랩이나 케글 노트에서는 Import하여 쉽게 사용한다. 


```python
import pandas as pd
pd.__version__
```


pandas는 오픈소스로 누구나 무료로 이용 할 수 있고,
Numpy, matplotlib등 다른 라이브러리들과 함께 쓰인다.

일반적으로 pandas는 pd로 import되기 때문에 pd.[function] 으로 써있으면 
pandas 라이브러리를 이용한다고 생각 하면 된다.

Livraly의 경우 version 오류가 많이 있으므로,
Import 해 주는 라이브러리는 version을 꼭 확인하여 오류에 대비 하도록 한다. 


  
Pandas에는 3가지 자료 구조가 있는데 series와 dataFrame, panel이 그것이다.




ref.

  [판다스/위키](https://wikidocs.net/32829)  <br>








<hr align="center" style="border: solid; 5px; #920E88; width: 80%;">

판다스를 이용에는 유용한 Tutorial들이 있다. 
아래 있으니 시간이 날 때마다 보면서 Update하자. 

영문판이 있지만 지금은 일단 한글 번역한것을 보자
화이팅 !

[판다스/doc](https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html)
[10 minutes to Pandas/한글](https://great-woman-hoseung.tistory.com/4)

