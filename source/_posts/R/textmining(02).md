---
title: "Text Mining in R(02)"
date: 2021-12-14 18:21:20
categories:
- R
- data_science
tags:
- R4ds
- dataScience
---

## Text Mining in R (02)

앞선 내용 :
[Text Mining in R (01)](https://yoonhwa-p.github.io/2021/12/14/R/textmining(01)/)

다음 내용 :
[Text Mining in R (03)]()

<br><br>

---
<br>

### § MeCab 설치

Mecab-ko 형태소 분석기 사용 위해서는 Rcppmecab 패키지가 있어야함.

[URL:](https://github.com/junhewk/RcppMeCab/blob/master/README_kr.md)

해당 깃허브에서 설치해야 할 파일을 다운로드 받은 후, 

![RcppMeCab_zipfiles](/../../imeges/R_images/RcppMeCab_zipfiles.png)
<br><br>

<div style="border: 1px solid gold">
<br>

- 압축 해제 시에 `C drive` 에서 `mecab` folder 생성 
- 오른쪽 버튼 클릭 후  `여기에압출풀기`를 선택하면 쉽다. 

<br>
</div>


- 이 과정에서 
![Rcppmecab](/../../imeges/R_images/Rcppmecab.png)

- 위의 file내의 폴더 형태와, file 명, 경로 가 같지 않으면 다음과 같은 에러가 난다. 


> Exception: 
list()

- 경로, file명 등을 확인 하기 바란다. 
- [오류 해결 참조](https://github.com/junhewk/RcppMeCab/issues/12)

- 

---


### §



