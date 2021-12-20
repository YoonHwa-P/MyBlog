---
title: "[Kgg]Tabular Playground Series (Dec. 2021)"
date: 2021-12-20 12:12:20
categories:
- python
    - plotly
tags:
- kaggle
- summary
- kaggle_dictation
---

![Tabular_Playground_Series_Dec(2021)](/../../imeges/kgg/Tabular_Playground_Series_Dec.png)

<br><br>
[origin of dictation](https://www.kaggle.com/andrej0marinchenko/tps12-21-data-visualization)
---
[Kgg_TPS 02]()



## Description of competition 

### data overview

![TPS12_overView](/../../imeges/kgg/TPS12_overView.png)

Kaggle에서 매달 1일에 data scientists의 Featured competitions을 위해 beginner- friendly로 제공하는 대회

- 대회의 목적 : a fun, and approachable 를 위해  anyone에게 tabular dataset 제공.
- dataset : [CTGAN](https://github.com/sdv-dev/CTGAN)에서 만들어진 [숲 토양 타입 예측 대회](https://www.kaggle.com/c/forest-cover-type-prediction/overview) data



<div style="border: 1px solid gold">
<br>

The study area includes four wilderness areas located in 
the **Roosevelt National Forest** of northern Colorado. 
Each observation is a 30m x 30m patch. 
You are asked to predict an integer classification for the forest cover type(FCT). 

<br>
<p style="color:#C6563B;font-size:130%;">The seven types are: </p>
<center>
1 - Spruce/Fir <br>
2 - Lodgepole Pine <br>
3 - Ponderosa Pine <br>
4 - Cottonwood/Willow <br>
5 - Aspen <br>
6 - Douglas-fir <br>
7 - Krummholz <br>
</center>

<br>

The training set (15120 observations) contains both features and the Cover_Type. 
The test set contains only the features. 
You must predict the Cover_Type for every row in the test set (565892 observations).

<br>

<p style="font-size:130%;"> Data Fields</p> <br>
Elevation - 미터 단위 고도  <br>
Aspect - 방위각의 종횡비 (위치) <br>
Slope - 경사 기울기 <br>
Horizontal_Distance_To_Hydrology - 해수면까지의 수평거리
<br> Vertical_Distance_To_Hydrology - 해수면까지의 수직거리
<br> Horizontal_Distance_To_Roadways - 도로와의 수평 거리 
<br> Hillshade_9am (0 to 255 index) - 여름, 오전 9시 언덕 모양 지수
<br> Hillshade_Noon (0 to 255 index) - 여름, 정오 언덕 모양 지수 
<br> Hillshade_3pm (0 to 255 index) - 여름, 오후 3시 언덕 모양 지수
<br> Horizontal_Distance_To_Fire_Points - 산불 발화점까지 수평거리
<br>
<p style="color:#4E3BC6;font-size:130%;"> Wilderness_Area </p> : 야생지역 <br>
- 4 개의 columns (토양 유형 지정) <br>
+ 0 = 없음 <br>
+ 1 = 있음 <br>
<p style="color:#3BC669;font-size:130%;"> Soil_Type </p> : 토양 유형 지정
<br>  - 40 개의  columns 
<br>    + 0 = 없음 
<br>    + 1 = 있음
<br> <p style="color:#C6563B;font-size:130%;"> Cover_Type </p> FCT 지정
br>  - 7 개  columns 
<br>    + 0 = 없음 
<br>    + 1 = 있음
<br><br><br>
<p style="color:#4E3BC6;font-size:130%;">
The wilderness areas are:
</p>
<br>
<center>
<br> 1 - Rawah Wilderness Area
<br> 2 - Neota Wilderness Area
<br> 3 - Comanche Peak Wilderness Area
<br> 4 - Cache la Poudre Wilderness Area
<br>
</center>
<br><br>

<p style="color:#3BC669;font-size:130%;"> The soil types are: </p>
<center>
 
<br> 1 Cathedral family - Rock outcrop complex, extremely stony.
<br> 2 Vanet - Ratake families complex, very stony.
<br> 3 Haploborolis - Rock outcrop complex, rubbly.
<br> 4 Ratake family - Rock outcrop complex, rubbly.
<br> 5 Vanet family - Rock outcrop complex complex, rubbly.
<br> 6 Vanet - Wetmore families - Rock outcrop complex, stony.
<br> 7 Gothic family.
<br> 8 Supervisor - Limber families complex.
<br> 9 Troutville family, very stony.
<br> 10 Bullwark - Catamount families - Rock outcrop complex, rubbly.
<br> 11 Bullwark - Catamount families - Rock land complex, rubbly.
<br> 12 Legault family - Rock land complex, stony.
<br> 13 Catamount family - Rock land - Bullwark family complex, rubbly.
<br> 14 Pachic Argiborolis - Aquolis complex.
<br> 15 unspecified in the USFS Soil and ELU Survey.
<br> 16 Cryaquolis - Cryoborolis complex.
<br> 17 Gateview family - Cryaquolis complex.
<br> 18 Rogert family, very stony.
<br> 19 Typic Cryaquolis - Borohemists complex.
<br> 20 Typic Cryaquepts - Typic Cryaquolls complex.
<br> 21 Typic Cryaquolls - Leighcan family, till substratum complex.
<br> 22 Leighcan family, till substratum, extremely bouldery.
<br> 23 Leighcan family, till substratum - Typic Cryaquolls complex.
<br> 24 Leighcan family, extremely stony.
<br> 25 Leighcan family, warm, extremely stony.
<br> 26 Granile - Catamount families complex, very stony.
<br> 27 Leighcan family, warm - Rock outcrop complex, extremely stony.
<br> 28 Leighcan family - Rock outcrop complex, extremely stony.
<br> 29 Como - Legault families complex, extremely stony.
<br> 30 Como family - Rock land - Legault family complex, extremely stony.
<br> 31 Leighcan - Catamount families complex, extremely stony.
<br> 32 Catamount family - Rock outcrop - Leighcan family complex, extremely stony.
<br> 33 Leighcan - Catamount families - Rock outcrop complex, extremely stony.
<br> 34 Cryorthents - Rock land complex, extremely stony.
<br> 35 Cryumbrepts - Rock outcrop - Cryaquepts complex.
<br> 36 Bross family - Rock land - Cryumbrepts complex, extremely stony.
<br> 37 Rock outcrop - Cryumbrepts - Cryorthents complex, extremely stony.
<br> 38 Leighcan - Moran families - Cryaquolls complex, extremely stony.
<br> 39 Moran family - Cryorthents - Leighcan family complex, extremely stony.
<br> 40 Moran family - Cryorthents - Rock land complex, extremely stony.
</center>

</div>

- stony : 돌처럼 딱딱한
- rubbly: 쓰레기 같은 
- outcrop : 노출


---

## Evaluation 

![TPS12_Evaluation](/../../imeges/kgg/TPS12_Evaluation.png)

각각의 ID 를 cover type 과 Matching하여 file format 형태를 만들어 제출 하면 됩니다. 

