---
title: "R for DS_01 welcome & Introduction"
date: 2021-11-29 17:21:20
categories:
- R
- data_science
tags:
- R4ds
- dataScience
---


## Welcome
<hr>

![R_Welcome](/../../imeges/R_images/R_Welcome.png)

- 저작권 : "R for DataScience by Hadley Wickham and Garrett Grolemund(O'Reilly). Copyright 2017 Garrett Grolemund, Hadley Wickham, 978-1-491-91039-9

<br><br><hr>

<br>

## Introduction 

<hr>

    Data science is an exciting discipline that allows you to turn raw data into understanding, insight, and knowledge. The goal of “R for Data Science” is to help you learn the most important tools in R that will allow you to do data science. After reading this book, you’ll have the tools to tackle a wide variety of data science challenges, using the best parts of R.
    
R을 이용한 data science를 해 봅시다. 

- ggplot2를 더 잘 쓰고 싶다면 
[A Layered Grammar of Graphics](https://byrneslab.net/classes/biol607/readings/wickham_layered-grammar.pdf) 
을 읽어보세요.


### 1.1 What you will learn
<hr>

![R4ds_Learn](/../../imeges/R_images/R4ds_Learn.png)


### 1.2 How this book is organised
### 1.3 What you won't learn
### 1.4 Prerequisites
#### 1.4.1 R
- https://cloud.r-project.org,
#### 1.4.2 RStudio
- http://www.rstudio.com/download.
- https://cran.r-project.org/bin/windows/Rtools/rtools40.html
- 위에 Rtools(64 bit) 실행


- 설치 해야 할 programs : 확인
![Rprograms_install](/../../imeges/R_images/Rprograms_install.png)

#### 1.4.3 The tidyverse

```r
install.packages("tidyverse")
```
- https://cloud.r-project.org/

```r
library(tidyverse)
#> ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──
#> ✔ ggplot2 3.3.2     ✔ purrr   0.3.4
#> ✔ tibble  3.0.3     ✔ dplyr   1.0.2
#> ✔ tidyr   1.1.2     ✔ stringr 1.4.0
#> ✔ readr   1.4.0     ✔ forcats 0.5.0
#> ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
```

+ tidyverse : ggplot2, tibble, tidyr, readr, purrr, and dplyr packages
#### 1.4.4 Other packages
```r
install.packages(c("nycflights13", "gapminder", "Lahman"))
```

### 1.5 Running R code
### 1.6 Getting help and learning more
### 1.7 Acknowledgements
### 1.8 Colophon


[My_rpubs](https://rpubs.com/YoonHwa-P)

---
ref. 

[introduction](https://r4ds.had.co.nz/introduction.html)

[Part 3 more](https://stat545.com/r-basics.html)