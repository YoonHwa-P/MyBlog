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

<br>앞선 내용 :
<br>[Text Mining in R (01)](https://yoonhwa-p.github.io/2021/12/14/R/textmining(01)/)
: library(KoNLP), useNIADic() 사용/설치 <br>
<br>다음 내용 :
<br>[Text Mining in R (03)]()
<br>

<br><br><br>

---
<br>

### § MeCab 설치

Mecab-ko 형태소 분석기 사용 위해서는 Rcppmecab 패키지가 있어야함.

[RcppMeCab install file URL:](https://github.com/junhewk/RcppMeCab/blob/master/README_kr.md)

해당 깃허브에서 설치해야 할 파일을 다운로드 받은 후, 

![RcppMeCab_zipfiles](/../../imeges/R_images/RcppMeCab_zipfiles.png)
<br><br>

<div style="border: 1px solid gold">
<br>

- 압축 해제 시에 `C drive` 에서 `mecab` folder 생성 
- 오른쪽 버튼 클릭 후  `여기에압출풀기`를 선택하면 쉽다. 

<br>
</div>


- 이 과정에서 <br>
![Rcppmecab](/../../imeges/R_images/Rcppmecab.png)

- 위의 file내의 폴더 형태와, file 명, 경로 가 같지 않으면 다음과 같은 에러가 난다. 


> Exception: 
list()

- 경로, file명 등을 확인 하기 바란다. 
- [오류 해결 참조](https://github.com/junhewk/RcppMeCab/issues/12)



---



### § R 에서 설치 

```R
# library(remotes)
remotes::install_github("junhewk/RcppMeCab", force = TRUE)

library(RcppMeCab)
```

<div style="border: 1px solid gold">

> \# library(remotes)
> remotes::install_github("junhewk/RcppMeCab", force = TRUE)
Downloading GitHub repo junhewk/RcppMeCab@HEAD
Installing 2 packages: BH, RcppParallel
‘C:/Users/brill/Documents/R/win-library/4.1’의 위치에 패키지(들)을 설치합니다.
(왜냐하면 ‘lib’가 지정되지 않았기 때문입니다)
trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/BH_1.75.0-0.zip'
Content type 'application/zip' length 19675040 bytes (18.8 MB)
downloaded 18.8 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/RcppParallel_5.1.4.zip'
Content type 'application/zip' length 2140731 bytes (2.0 MB)
downloaded 2.0 MB

package ‘BH’ successfully unpacked and MD5 sums checked
package ‘RcppParallel’ successfully unpacked and MD5 sums checked

The downloaded binary packages are in
	C:\Users\brill\AppData\Local\Temp\RtmpmuDZXg\downloaded_packages
√  checking for file 'C:\Users\brill\AppData\Local\Temp\RtmpmuDZXg\remotes2cd0f4c5d4d\junhewk-RcppMeCab-e1800aa/DESCRIPTION' (414ms)
-  preparing 'RcppMeCab': (373ms)
√  checking DESCRIPTION meta-information ...
-  cleaning src
-  checking for LF line-endings in source and make files and shell scripts
-  checking for empty or unneeded directories
   Omitted 'LazyData' from DESCRIPTION
-  building 'RcppMeCab_0.0.1.3-2.tar.gz'
   
‘C:/Users/brill/Documents/R/win-library/4.1’의 위치에 패키지(들)을 설치합니다.
(왜냐하면 ‘lib’가 지정되지 않았기 때문입니다)
* installing *source* package 'RcppMeCab' ...
** using staged installation
** libs
"C:/rtools40/mingw64/bin/"g++  -std=gnu++11 -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -I../inst/include -DBOOST_NO_AUTO_PTR -I'C:/Users/brill/Documents/R/win-library/4.1/Rcpp/include' -I'C:/Users/brill/Documents/R/win-library/4.1/RcppParallel/include' -I'C:/Users/brill/Documents/R/win-library/4.1/BH/include'     -DRCPP_PARALLEL_USE_TBB=1 -DDLL_IMPORT -DSTRICT_R_HEADERS -Wno-parentheses   -O2 -Wall  -mfpmath=sse -msse2 -mstackrealign  -c RcppExports.cpp -o RcppExports.o
"C:/rtools40/mingw64/bin/"g++  -std=gnu++11 -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -I../inst/include -DBOOST_NO_AUTO_PTR -I'C:/Users/brill/Documents/R/win-library/4.1/Rcpp/include' -I'C:/Users/brill/Documents/R/win-library/4.1/RcppParallel/include' -I'C:/Users/brill/Documents/R/win-library/4.1/BH/include'     -DRCPP_PARALLEL_USE_TBB=1 -DDLL_IMPORT -DSTRICT_R_HEADERS -Wno-parentheses   -O2 -Wall  -mfpmath=sse -msse2 -mstackrealign  -c posParallelRcpp.cpp -o posParallelRcpp.o
"C:/rtools40/mingw64/bin/"g++  -std=gnu++11 -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -I../inst/include -DBOOST_NO_AUTO_PTR -I'C:/Users/brill/Documents/R/win-library/4.1/Rcpp/include' -I'C:/Users/brill/Documents/R/win-library/4.1/RcppParallel/include' -I'C:/Users/brill/Documents/R/win-library/4.1/BH/include'     -DRCPP_PARALLEL_USE_TBB=1 -DDLL_IMPORT -DSTRICT_R_HEADERS -Wno-parentheses   -O2 -Wall  -mfpmath=sse -msse2 -mstackrealign  -c posRcpp.cpp -o posRcpp.o
"C:/rtools40/mingw64/bin/"g++  -std=gnu++11 -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -I../inst/include -DBOOST_NO_AUTO_PTR -I'C:/Users/brill/Documents/R/win-library/4.1/Rcpp/include' -I'C:/Users/brill/Documents/R/win-library/4.1/RcppParallel/include' -I'C:/Users/brill/Documents/R/win-library/4.1/BH/include'     -DRCPP_PARALLEL_USE_TBB=1 -DDLL_IMPORT -DSTRICT_R_HEADERS -Wno-parentheses   -O2 -Wall  -mfpmath=sse -msse2 -mstackrealign  -c posloopRcpp.cpp -o posloopRcpp.o
C:/rtools40/mingw64/bin/g++ -shared -s -static-libgcc -o RcppMeCab.dll tmp.def RcppExports.o posParallelRcpp.o posRcpp.o posloopRcpp.o -L../inst/libs/x64 -LC:/Users/brill/Documents/R/win-library/4.1/RcppParallel/lib/x64 -ltbb -ltbbmalloc -lm -llibmecab -LC:/PROGRA~1/R/R-41~1.2/bin/x64 -lR
installing to C:/Users/brill/Documents/R/win-library/4.1/00LOCK-RcppMeCab/00new/RcppMeCab/libs/x64
** R
** inst
** byte-compile and prepare package for lazy loading
** help
*** installing help indices
  converting help for package 'RcppMeCab'
    finding HTML links ... done
    RcppMeCab                               html  
    pos                                     html  
    posParallel                             html  
** building package indices
** testing if installed package can be loaded from temporary location
** testing if installed package can be loaded from final location
** testing if installed package keeps a record of temporary installation path
* DONE (RcppMeCab)

</div>


### RcppMeCab 설치 확인 (형태소 분리기)

text 1에 한글을 써 본다. 

```R
text1 = "안녕하세요?!"
pos(sentence = text1)
```
<br>

> text1 = "안녕하세요?!"
> pos(sentence = text1)
> $`�ȳ\xe7\xc7ϼ��\xe4?!`
> [1] "�/SY"           "ȳ/SL"            "\xe7\xc7\xcf/SH"
> [4] "���/SY"       "\xe4?!/SH"      

<br>
<br>
- 인코딩이 UTF-8로 되어 있지 안아서 생기는 문제이다. 

```R
text2 = enc2utf8(text1)
pos(sentence = text2)
```
<br>

> text2 = enc2utf8(text1) <br>
> pos(sentence = text2) <br>
> $`안녕하세요?!` <br>
> [1] "안녕/NNG"   "하/XSV"     "세요/EP+EF" "?/SF"       "!/SF"    <br>



