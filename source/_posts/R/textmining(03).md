---
title: "Text Mining in R"
date: 2021-12-15 09:11:16
categories:
- R
- data_science
tags:
- R4ds
- dataScience
---

## Text Mining in R (03)

앞선 내용 :
<br>[Text Mining in R (01)](https://yoonhwa-p.github.io/2021/12/14/R/textmining(01)/)
<br>[Text Mining in R (02)](https://yoonhwa-p.github.io/2021/12/14/R/textmining(02)/)
<br>다음 내용 :
<br>[Text Mining in R (04)]()

### MeCab 설치

Mecab-ko 형태소 분석기 사용 위해서는 Rcppmecab 패키지를 설치해야 함.

RcppMeCab 패키지 설치 앞서서 설치할 파일이 있음.

[URL:](https://github.com/junhewk/RcppMeCab/blob/master/README_kr.md)

해당 깃허브에서 설치해야 할 파일을 다운로드 받은 후, 

“C:\mecab” 경로에 설치한다.

- 이 과정에서 
![Rcppmecab](/../../imeges/R_images/Rcppmecab.png)

- 위의 file내의 폴더 형태와, file 명, 경로 가 같지 않으면 에러가 난다. 



---



