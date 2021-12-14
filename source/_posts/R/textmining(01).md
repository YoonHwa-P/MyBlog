---
title: "Text Mining in R(01)"
date: 2021-12-14 14:21:20
categories:
- R
- data_science
tags:
- R4ds
- dataScience
---

## R을 이용한 TextMining

- NLP : 자연어
- NLP slide 110p, python코드 책 3권 참조
- [딥 러닝을 이용한 자연어 처리 입문/Ko](https://wikidocs.net/book/2155)
  -  06. 토픽 모델링(Topic Modeling) 까지는 가능 
- 다음 내용 :
[Text Mining in R (02)](https://yoonhwa-p.github.io/2021/12/14/R/textmining(02)/)


<br><br><br><hr>

** R Install에 관한 내용은   
[여기](https://yoonhwa-p.github.io/2021/11/29/R/R_R4ds_Welcom/) 있다.

### 빅카인즈 (Korea)

- Text data 분석 하는 곳
- [bigkinds/ko](https://www.bigkinds.or.kr/)
- ![bigkinds](/../../imeges/R_images/bigkinds.png)

<br><br><hr>

### 감정분석

- 댓글에서 부정/ 긍정 에 대해 확인 

### R 환경 설정 

```R
install.packaged("multilinguer")
#위에 Install이 안되면, 아래 것으로 설치 

install.packages("remotes")
remotes::install_github("mrchypark/multilinguer")

install_jdk()
#자바 설치가 자동으로 path 설정 까지 될 수 있도록 해줌
```

>package ‘rJava’ successfully unpacked and MD5 sums checked

<br><br>

#### R-tool 설치 (path 설정)

- 이미 R-tool 이 설치가 되어있다면, Pass
- [R-tool 설치 후](https://cran.r-project.org/bin/windows/Rtools/rtools40.html)
- 아래 코드를 실행 한 후 R Studio program 종료후 재시작

```R
write('PATH="${RTOOLS40_HOME}\\usr\\bin;${PATH}"', 
      file = "~/.Renviron", append = TRUE)
Sys.which("make")
```

> write('PATH="${RTOOLS40_HOME}\\usr\\bin;${PATH}"', <br>
>       file = "~/.Renviron", append = TRUE) <br>
> Sys.which("make") <br>
.............................................make <br> 
"C:\\rtools40\\usr\\bin\\make.exe"  <br>

#### jsonlite install


```R
install.packages("jsonlite", type = "source")
```
<div style="border: 1px solid gold">

> install.packages("jsonlite", type = "source")
‘C:/Users/brill/Documents/R/win-library/4.1’의 위치에 패키지(들)을 설치합니다.
(왜냐하면 ‘lib’가 지정되지 않았기 때문입니다)
trying URL 'https://cran.rstudio.com/src/contrib/jsonlite_1.7.2.tar.gz'
Content type 'application/x-gzip' length 421716 bytes (411 KB)
downloaded 411 KB

* installing *source* package 'jsonlite' ...
** package 'jsonlite' successfully unpacked and MD5 sums checked
** using staged installation
** libs
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c base64.c -o base64.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c collapse_array.c -o collapse_array.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c collapse_object.c -o collapse_object.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c collapse_pretty.c -o collapse_pretty.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c escape_chars.c -o escape_chars.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c integer64_to_na.c -o integer64_to_na.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c is_datelist.c -o is_datelist.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c is_recordlist.c -o is_recordlist.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c is_scalarlist.c -o is_scalarlist.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c modp_numtoa.c -o modp_numtoa.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c null_to_na.c -o null_to_na.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c num_to_char.c -o num_to_char.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c parse.c -o parse.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c prettify.c -o prettify.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c push_parser.c -o push_parser.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c r-base64.c -o r-base64.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c register.c -o register.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c row_collapse.c -o row_collapse.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c transpose_list.c -o transpose_list.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c validate.c -o validate.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c yajl/yajl.c -o yajl/yajl.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c yajl/yajl_alloc.c -o yajl/yajl_alloc.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c yajl/yajl_buf.c -o yajl/yajl_buf.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c yajl/yajl_encode.c -o yajl/yajl_encode.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c yajl/yajl_gen.c -o yajl/yajl_gen.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c yajl/yajl_lex.c -o yajl/yajl_lex.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c yajl/yajl_parser.c -o yajl/yajl_parser.o
"C:/rtools40/mingw64/bin/"gcc  -I"C:/PROGRA~1/R/R-41~1.2/include" -DNDEBUG -Iyajl/api       -D__USE_MINGW_ANSI_STDIO   -O2 -Wall  -std=gnu99 -mfpmath=sse -msse2 -mstackrealign  -c yajl/yajl_tree.c -o yajl/yajl_tree.o
"C:/rtools40/mingw64/bin/"ar rcs yajl/libstatyajl.a yajl/yajl.o yajl/yajl_alloc.o yajl/yajl_buf.o yajl/yajl_encode.o yajl/yajl_gen.o yajl/yajl_lex.o yajl/yajl_parser.o yajl/yajl_tree.o
C:/rtools40/mingw64/bin/gcc -shared -s -static-libgcc -o jsonlite.dll tmp.def base64.o collapse_array.o collapse_object.o collapse_pretty.o escape_chars.o integer64_to_na.o is_datelist.o is_recordlist.o is_scalarlist.o modp_numtoa.o null_to_na.o num_to_char.o parse.o prettify.o push_parser.o r-base64.o register.o row_collapse.o transpose_list.o validate.o -Lyajl -lstatyajl -LC:/PROGRA~1/R/R-41~1.2/bin/x64 -lR
installing to C:/Users/brill/Documents/R/win-library/4.1/00LOCK-jsonlite/00new/jsonlite/libs/x64
** R
** inst
** byte-compile and prepare package for lazy loading
in method for 'asJSON' with signature '"blob"': no definition for class "blob"
** help
*** installing help indices
  converting help for package 'jsonlite'
    finding HTML links ... done
    base64                                  html  
    flatten                                 html  
    fromJSON                                html  
    prettify                                html  
    rbind_pages                             html  
    read_json                               html  
    serializeJSON                           html  
    stream_in                               html  
    unbox                                   html  
    validate                                html  
** building package indices
** installing vignettes
** testing if installed package can be loaded from temporary location
** testing if installed package can be loaded from final location
** testing if installed package keeps a record of temporary installation path
* DONE (jsonlite)

</div>

<br><br>

---

#### R packages 설치 

```R
install.packages(c("stringr", "hash", "tau", "Sejong", "RSQLite", "devtools"),
                 type = "binary")
```

<br>
<div style="border: 1px solid gold">
<br>

> The downloaded source packages are in
	‘C:\Users\brill\AppData\Local\Temp\RtmpmuDZXg\downloaded_packages’
> install.packages(c("stringr", "hash", "tau", "Sejong", "RSQLite", "devtools"),
+                  type = "binary")
‘C:/Users/brill/Documents/R/win-library/4.1’의 위치에 패키지(들)을 설치합니다.
(왜냐하면 ‘lib’가 지정되지 않았기 때문입니다)
‘fastmap’, ‘highr’, ‘xfun’, ‘diffobj’, ‘rematch2’, ‘bit’, ‘cachem’, ‘processx’, ‘prettyunits’, ‘digest’, ‘xopen’, ‘brew’, ‘commonmark’, ‘knitr’, ‘cpp11’, ‘brio’, ‘evaluate’, ‘praise’, ‘ps’, ‘waldo’, ‘bit64’, ‘blob’, ‘DBI’, ‘memoise’, ‘Rcpp’, ‘plogr’, ‘callr’, ‘pkgbuild’, ‘pkgload’, ‘rcmdcheck’, ‘roxygen2’, ‘rversions’, ‘sessioninfo’, ‘testthat’(들)을 또한 설치합니다.

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/fastmap_1.1.0.zip'
Content type 'application/zip' length 215381 bytes (210 KB)
downloaded 210 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/highr_0.9.zip'
Content type 'application/zip' length 46725 bytes (45 KB)
downloaded 45 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/xfun_0.28.zip'
Content type 'application/zip' length 386111 bytes (377 KB)
downloaded 377 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/diffobj_0.3.5.zip'
Content type 'application/zip' length 999001 bytes (975 KB)
downloaded 975 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/rematch2_2.1.2.zip'
Content type 'application/zip' length 47584 bytes (46 KB)
downloaded 46 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/bit_4.0.4.zip'
Content type 'application/zip' length 635254 bytes (620 KB)
downloaded 620 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/cachem_1.0.6.zip'
Content type 'application/zip' length 79002 bytes (77 KB)
downloaded 77 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/processx_3.5.2.zip'
Content type 'application/zip' length 1246508 bytes (1.2 MB)
downloaded 1.2 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/prettyunits_1.1.1.zip'
Content type 'application/zip' length 37755 bytes (36 KB)
downloaded 36 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/digest_0.6.29.zip'
Content type 'application/zip' length 266591 bytes (260 KB)
downloaded 260 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/xopen_1.0.0.zip'
Content type 'application/zip' length 24785 bytes (24 KB)
downloaded 24 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/brew_1.0-6.zip'
Content type 'application/zip' length 113926 bytes (111 KB)
downloaded 111 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/commonmark_1.7.zip'
Content type 'application/zip' length 265490 bytes (259 KB)
downloaded 259 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/knitr_1.36.zip'
Content type 'application/zip' length 1469306 bytes (1.4 MB)
downloaded 1.4 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/cpp11_0.4.2.zip'
Content type 'application/zip' length 327396 bytes (319 KB)
downloaded 319 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/brio_1.1.3.zip'
Content type 'application/zip' length 48880 bytes (47 KB)
downloaded 47 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/evaluate_0.14.zip'
Content type 'application/zip' length 76790 bytes (74 KB)
downloaded 74 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/praise_1.0.0.zip'
Content type 'application/zip' length 19849 bytes (19 KB)
downloaded 19 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/ps_1.6.0.zip'
Content type 'application/zip' length 775912 bytes (757 KB)
downloaded 757 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/waldo_0.3.1.zip'
Content type 'application/zip' length 96434 bytes (94 KB)
downloaded 94 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/bit64_4.0.5.zip'
Content type 'application/zip' length 565517 bytes (552 KB)
downloaded 552 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/blob_1.2.2.zip'
Content type 'application/zip' length 48321 bytes (47 KB)
downloaded 47 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/DBI_1.1.1.zip'
Content type 'application/zip' length 686681 bytes (670 KB)
downloaded 670 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/memoise_2.0.1.zip'
Content type 'application/zip' length 50131 bytes (48 KB)
downloaded 48 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/Rcpp_1.0.7.zip'
Content type 'application/zip' length 3263462 bytes (3.1 MB)
downloaded 3.1 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/plogr_0.2.0.zip'
Content type 'application/zip' length 18943 bytes (18 KB)
downloaded 18 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/callr_3.7.0.zip'
Content type 'application/zip' length 437774 bytes (427 KB)
downloaded 427 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/pkgbuild_1.3.0.zip'
Content type 'application/zip' length 146266 bytes (142 KB)
downloaded 142 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/pkgload_1.2.4.zip'
Content type 'application/zip' length 156265 bytes (152 KB)
downloaded 152 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/rcmdcheck_1.4.0.zip'
Content type 'application/zip' length 170257 bytes (166 KB)
downloaded 166 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/roxygen2_7.1.2.zip'
Content type 'application/zip' length 1352846 bytes (1.3 MB)
downloaded 1.3 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/rversions_2.1.1.zip'
Content type 'application/zip' length 67399 bytes (65 KB)
downloaded 65 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/sessioninfo_1.2.2.zip'
Content type 'application/zip' length 186234 bytes (181 KB)
downloaded 181 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/testthat_3.1.1.zip'
Content type 'application/zip' length 2545637 bytes (2.4 MB)
downloaded 2.4 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/stringr_1.4.0.zip'
Content type 'application/zip' length 216715 bytes (211 KB)
downloaded 211 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/hash_2.2.6.1.zip'
Content type 'application/zip' length 178061 bytes (173 KB)
downloaded 173 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/tau_0.0-24.zip'
Content type 'application/zip' length 186662 bytes (182 KB)
downloaded 182 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/Sejong_0.01.zip'
Content type 'application/zip' length 1617954 bytes (1.5 MB)
downloaded 1.5 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/RSQLite_2.2.9.zip'
Content type 'application/zip' length 2511267 bytes (2.4 MB)
downloaded 2.4 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/devtools_2.4.3.zip'
Content type 'application/zip' length 423398 bytes (413 KB)
downloaded 413 KB

package ‘fastmap’ successfully unpacked and MD5 sums checked
package ‘highr’ successfully unpacked and MD5 sums checked
package ‘xfun’ successfully unpacked and MD5 sums checked
package ‘diffobj’ successfully unpacked and MD5 sums checked
package ‘rematch2’ successfully unpacked and MD5 sums checked
package ‘bit’ successfully unpacked and MD5 sums checked
package ‘cachem’ successfully unpacked and MD5 sums checked
package ‘processx’ successfully unpacked and MD5 sums checked
package ‘prettyunits’ successfully unpacked and MD5 sums checked
package ‘digest’ successfully unpacked and MD5 sums checked
package ‘xopen’ successfully unpacked and MD5 sums checked
package ‘brew’ successfully unpacked and MD5 sums checked
package ‘commonmark’ successfully unpacked and MD5 sums checked
package ‘knitr’ successfully unpacked and MD5 sums checked
package ‘cpp11’ successfully unpacked and MD5 sums checked
package ‘brio’ successfully unpacked and MD5 sums checked
package ‘evaluate’ successfully unpacked and MD5 sums checked
package ‘praise’ successfully unpacked and MD5 sums checked
package ‘ps’ successfully unpacked and MD5 sums checked
package ‘waldo’ successfully unpacked and MD5 sums checked
package ‘bit64’ successfully unpacked and MD5 sums checked
package ‘blob’ successfully unpacked and MD5 sums checked
package ‘DBI’ successfully unpacked and MD5 sums checked
package ‘memoise’ successfully unpacked and MD5 sums checked
package ‘Rcpp’ successfully unpacked and MD5 sums checked
package ‘plogr’ successfully unpacked and MD5 sums checked
package ‘callr’ successfully unpacked and MD5 sums checked
package ‘pkgbuild’ successfully unpacked and MD5 sums checked
package ‘pkgload’ successfully unpacked and MD5 sums checked
package ‘rcmdcheck’ successfully unpacked and MD5 sums checked
package ‘roxygen2’ successfully unpacked and MD5 sums checked
package ‘rversions’ successfully unpacked and MD5 sums checked
package ‘sessioninfo’ successfully unpacked and MD5 sums checked
package ‘testthat’ successfully unpacked and MD5 sums checked
package ‘stringr’ successfully unpacked and MD5 sums checked
package ‘hash’ successfully unpacked and MD5 sums checked
package ‘tau’ successfully unpacked and MD5 sums checked
package ‘Sejong’ successfully unpacked and MD5 sums checked
package ‘RSQLite’ successfully unpacked and MD5 sums checked
package ‘devtools’ successfully unpacked and MD5 sums checked

<br><br>

</div>
<br><br>

#### 명사 분리기(KoNLP) 설치를 위한 remotes packages 설치 (in R) 

```R
# install.packages("remotes")
remotes::install_github("haven-jeon/KoNLP",
                        upgrade = "never",
                        force = TRUE,
                        INSTALL_opts = c("--no-multiarch"))
```

<div style="border: 1px solid gold">
<br><br>

>> \# install.packages("remotes")
> remotes::install_github("haven-jeon/KoNLP",
>+                         upgrade = "never",
>+                         force = TRUE,
>+                         INSTALL_opts = c("--no-multiarch"))
>Downloading GitHub repo haven-jeon/KoNLP@HEAD
√  checking for file 'C:\Users\brill\AppData\Local\Temp\RtmpmuDZXg\remotes2cd03d177e06\haven-jeon-KoNLP-960fbbc/DESCRIPTION' ... 
>-  preparing 'KoNLP': (722ms)
>√  checking DESCRIPTION meta-information ... 
>-  checking for LF line-endings in source and make files and shell scripts
-  checking for empty or unneeded directories
-  looking to see if a 'data/datalist' file should be added
-  building 'KoNLP_0.80.2.tar.gz'
   
‘C:/Users/brill/Documents/R/win-library/4.1’의 위치에 패키지(들)을 설치합니다.
(왜냐하면 ‘lib’가 지정되지 않았기 때문입니다)
* installing *source* package 'KoNLP' ...
** using staged installation
** R
** data
** inst
** byte-compile and prepare package for lazy loading
** help
*** installing help indices
  converting help for package 'KoNLP'
    finding HTML links ... done
    HangulAutomata                          html  
    KtoS                                    html  
    MorphAnalyzer                           html  
    SimplePos09                             html  
    SimplePos22                             html  
    StoK                                    html  
    backupUsrDic                            html  
    buildDictionary                         html  
    concordance_file                        html  
    concordance_str                         html  
    convertHangulStringToJamos              html  
    convertHangulStringToKeyStrokes         html  
    convertTag                              html  
    editweights                             html  
    extractNoun                             html  
    get_dictionary                          html  
    is.ascii                                html  
    is.hangul                               html  
    is.jaeum                                html  
    is.jamo                                 html  
    is.moeum                                html  
    mergeUserDic                            html  
    mutualinformation                       html  
    reloadAllDic                            html  
    reloadUserDic                           html  
    restoreUsrDic                           html  
    scala_library_install                   html  
    statDic                                 html  
    tags                                    html  
    useNIADic                               html  
    useSejongDic                            html  
    useSystemDic                            html  
** building package indices
** installing vignettes
** testing if installed package can be loaded from temporary location
[1] "DEBUG start"
[1] "C:/Users/brill/Documents/R/win-library/4.1/00LOCK-KoNLP/00new/KoNLP/java/scala-library-2.11.8.jar"
[1] "My R is over 3.2.0"
[1] "scala-library target url: https://repo1.maven.org/maven2/org/scala-lang/scala-library/2.11.8/scala-library-2.11.8.jar"
[1] "'method' parameter for download.file() function in your R:  wininet"
URL 'https://repo1.maven.org/maven2/org/scala-lang/scala-library/2.11.8/scala-library-2.11.8.jar'을 시도합니다
Content type 'application/java-archive' length 5744974 bytes (5.5 MB)
==================================================
downloaded 5.5 MB

[1] TRUE
[1] 5744974
Successfully installed Scala runtime library in C:/Users/brill/Documents/R/win-library/4.1/00LOCK-KoNLP/00new/KoNLP/java/scala-library-2.11.8.jar
** testing if installed package can be loaded from final location
** testing if installed package keeps a record of temporary installation path
* DONE (KoNLP)

</div>
<br><br>

---

### 명사 분리기 KoNLP 설치 

```R
library(KoNLP)
useNIADic()
```

<div style="border: 1px solid gold">
<br><br>

> library(KoNLP)
> useNIADic()
Backup was just finished!
Downloading package from url: https://github.com/haven-jeon/NIADic/releases/download/0.0.1/NIADic_0.0.1.tar.gz
Installing 16 packages: colorspace, viridisLite, RColorBrewer, munsell, labeling, farver, base64enc, htmltools, scales, isoband, gtable, jquerylib, tinytex, ggplot2, data.table, rmarkdown
‘C:/Users/brill/Documents/R/win-library/4.1’의 위치에 패키지(들)을 설치합니다.
(왜냐하면 ‘lib’가 지정되지 않았기 때문입니다)
trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/colorspace_2.0-2.zip'
Content type 'application/zip' length 2645307 bytes (2.5 MB)
downloaded 2.5 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/viridisLite_0.4.0.zip'
Content type 'application/zip' length 1299504 bytes (1.2 MB)
downloaded 1.2 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/RColorBrewer_1.1-2.zip'
Content type 'application/zip' length 55707 bytes (54 KB)
downloaded 54 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/munsell_0.5.0.zip'
Content type 'application/zip' length 245486 bytes (239 KB)
downloaded 239 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/labeling_0.4.2.zip'
Content type 'application/zip' length 62679 bytes (61 KB)
downloaded 61 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/farver_2.1.0.zip'
Content type 'application/zip' length 1752621 bytes (1.7 MB)
downloaded 1.7 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/base64enc_0.1-3.zip'
Content type 'application/zip' length 43156 bytes (42 KB)
downloaded 42 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/htmltools_0.5.2.zip'
Content type 'application/zip' length 347310 bytes (339 KB)
downloaded 339 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/scales_1.1.1.zip'
Content type 'application/zip' length 558895 bytes (545 KB)
downloaded 545 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/isoband_0.2.5.zip'
Content type 'application/zip' length 2726764 bytes (2.6 MB)
downloaded 2.6 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/gtable_0.3.0.zip'
Content type 'application/zip' length 434327 bytes (424 KB)
downloaded 424 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/jquerylib_0.1.4.zip'
Content type 'application/zip' length 525848 bytes (513 KB)
downloaded 513 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/tinytex_0.35.zip'
Content type 'application/zip' length 126495 bytes (123 KB)
downloaded 123 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/ggplot2_3.3.5.zip'
Content type 'application/zip' length 4130301 bytes (3.9 MB)
downloaded 3.9 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/data.table_1.14.2.zip'
Content type 'application/zip' length 2600846 bytes (2.5 MB)
downloaded 2.5 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/rmarkdown_2.11.zip'
Content type 'application/zip' length 3660449 bytes (3.5 MB)
downloaded 3.5 MB

package ‘colorspace’ successfully unpacked and MD5 sums checked
package ‘viridisLite’ successfully unpacked and MD5 sums checked
package ‘RColorBrewer’ successfully unpacked and MD5 sums checked
package ‘munsell’ successfully unpacked and MD5 sums checked
package ‘labeling’ successfully unpacked and MD5 sums checked
package ‘farver’ successfully unpacked and MD5 sums checked
package ‘base64enc’ successfully unpacked and MD5 sums checked
package ‘htmltools’ successfully unpacked and MD5 sums checked
package ‘scales’ successfully unpacked and MD5 sums checked
package ‘isoband’ successfully unpacked and MD5 sums checked
package ‘gtable’ successfully unpacked and MD5 sums checked
package ‘jquerylib’ successfully unpacked and MD5 sums checked
package ‘tinytex’ successfully unpacked and MD5 sums checked
package ‘ggplot2’ successfully unpacked and MD5 sums checked
package ‘data.table’ successfully unpacked and MD5 sums checked
package ‘rmarkdown’ successfully unpacked and MD5 sums checked

The downloaded binary packages are in
	C:\Users\brill\AppData\Local\Temp\RtmpmuDZXg\downloaded_packages
√  checking for file 'C:\Users\brill\AppData\Local\Temp\RtmpmuDZXg\remotes2cd0437ea43\NIADic/DESCRIPTION' ... 
-  preparing 'NIADic':
√  checking DESCRIPTION meta-information ... 
√  checking vignette meta-information ...
-  checking for LF line-endings in source and make files and shell scripts
-  checking for empty or unneeded directories
-  building 'NIADic_0.0.1.tar.gz'
   
‘C:/Users/brill/Documents/R/win-library/4.1’의 위치에 패키지(들)을 설치합니다.
(왜냐하면 ‘lib’가 지정되지 않았기 때문입니다)
* installing *source* package 'NIADic' ...
** using staged installation
** R
** inst
** byte-compile and prepare package for lazy loading
** help
*** installing help indices
  converting help for package 'NIADic'
    finding HTML links ... done
    get_dic                                 html  
** building package indices
** installing vignettes
** testing if installed package can be loaded from temporary location
** testing if installed package can be loaded from final location
** testing if installed package keeps a record of temporary installation path
* DONE (NIADic)
1213109 words dictionary was built.
> 
</div>

<br><br>

---


### 명사 분리기 설치 후 확인 

[New](https://news.v.daum.net/v/20211214145504242)

```R

text = "뿌리산업’의 기반이 되는 공정기술의 범위가 관련법 제정 10년 만에 확대 개편된다. 뿌리기업 우대 지원과 청년층 등 신규인력 유입 지원을 강화하기 위한 법적 토대도 마련된다.
산업통상자원부는 이 같은 내용을 담은 ‘뿌리산업 진흥과 첨단화에 관한 법률(뿌리산업법) 시행령’ 개정안이 14일 국무회의에서 의결돼 오는 16일부터 시행된다고 밝혔다.
먼저 뿌리산업법 기반 공정기술(뿌리기술)의 범위가 기존 6개(주조, 금형, 소성가공, 용접, 표면처리, 열처리)에서 14개로 늘어난다.
구체적으로 소재 다원화 공정기술에 사출·프레스, 정밀가공, 적층제조, 산업용 필름 및 지류 등 4개 기술이 포함된다. 산업부는 이를 통해 세라믹, 플라스틱, 탄성소재, 탄소, 펄프 등 다양한 소재 기반 제조 공정을 확산할 계획이다. 또 지능화 공정기술로 로봇, 센서, 산업 지능형 소프트웨어, 엔지니어링 설계 등 4개 기술이 추가된다.
뿌리기술 범위가 확대되면서 뿌리산업의 범위도 기존 6대 산업, 76개 업종에서 14대 산업, 111개 업종으로 늘어난다.

이번 개정을 통해 뿌리기업 확인 절차, 확인서 유효기간(3년), 사후관리 등에 관한 규정도 신설됐다. 뿌리기업은 뿌리기술을 활용해 사업을 영위하는 업종 또는 뿌리기술에 활용되는 장비 제조 분야를 말한다.
뿌리기업 확인 제도는 외국인 근로자 고용 우대 혜택 등이 주어지는 뿌리산업 관련 우대 지원 대상을 명확히 정하기 위한 것으로 국가뿌리산업진흥센터에서 확인서를 발급해오고 있다. 2012년부터 1만1766건이 발급됐으며 현재 5843건이 유효한 것으로 집계됐다.

‘일하기 좋은 뿌리기업’ 선정을 위한 기준과 절차, 지원 내용 등에 관한 규정도 새로 만들어졌다. ‘일하기 좋은 뿌리기업’은 뿌리산업에 청년층 등 신규 인력 유입을 촉진하기 위해 근로·복지 환경, 성장 역량 등이 우수한 기업을 산업부가 선정해 홍보 등을 지원하는 제도다.
산업부는 이번 개정 사항이 원활히 시행될 수 있도록 업종별 협·단체, 뿌리기업, 지자체 등을 대상으로 적극 홍보할 방침이다. 아울러 매년 발간하는 뿌리산업 백서를 통해 새롭게 추가되는 8대 차세대 공정기술에 대한 내용, 기술 동향 등을 상세하게 제공하기로 했다．
산업부 관계자는 “이번 개정은 2011년 뿌리산업법 제정 후 10년 만에 뿌리기술을 소재다원화와 지능화 중심으로 확장한 것으로, 뿌리산업의 기술 융복합화와 첨단화를 촉진하고 신규 인력 유입 지원을 강화하기 위한 법적 토대를 마련하였다는 데에 의의가 있다”고 말했다."

extractNoun(text)

```
<div style="border: 1px solid gold">
<br><br>

> extractNoun(text)
  [1] "뿌리산업’"         "기반"               "공정기술"          
  [4] "범위"               "관련"               "법"                
  [7] "제정"               "10"                 "년"                
 [10] "만"                 "확대"               "개편"              
 [13] "뿌리"               "기업"               "우대"              
 [16] "지원"               "청년층"             "등"                
 [19] "신규인력"           "유입"               "지원"              
 [22] "강화"               "하기"               "토대"              
 [25] "마련"               "산업"               "통상"              
 [28] "자원"               "부"                 "내용"              
 [31] "담"                 "‘뿌리산업"         "진흥"              
 [34] "첨단화"             "법률(뿌리산업법)"   "시행령’"          
 [37] "개정안"             "14"                 "국무회의"          
 [40] "의결"               "16"                 "일"                
 [43] "시행"               "뿌리"               "산업"              
 [46] "법"                 "기반"               "공정기술(뿌리기술)"
 [49] "범위"               "기존"               "6개(주조"          
 [52] "금형"               "소성"               "가공"              
 [55] "용접"               "표면처리"           "열처리"            
 [58] "14"                 "개"                 "구체"              
 [61] "적"                 "소재"               "다원화"            
 [64] "공정기술"           "사출·프레스"       "정밀가공"          
 [67] "적층"               "제조"               "산업용"            
 [70] "필름"               "지류"               "등"                
 [73] "4"                  "개"                 "기술"              
 [76] "포함"               "산업"               "부"                
 [79] "이"                 "세라믹"             "플라스틱"          
 [82] "탄성소"             "재"                 "탄소"              
 [85] "펄프"               "등"                 "다양"              
 [88] "한"                 "소재"               "기반"              
 [91] "제조"               "공정"               "확산"              
 [94] "할"                 "계획"               "지능화"            
 [97] "공정기술"           "로봇"               "센서"              
[100] "산업"               "지능형"             "소프트웨어"        
[103] "엔지니어링"         "설계"               "등"                
[106] "4"                  "개"                 "기술"              
[109] "추가"               "뿌리"               "기술"              
[112] "범위"               "확대"               "되"                
[115] "뿌리"               "산업"               "범위"              
[118] "기존"               "6"                  "대"                
[121] "산업"               "76"                 "개"                
[124] "업종"               "14"                 "대"                
[127] "산업"               "111"                "개"                
[130] "업종"               "이번"               "개정"              
[133] "뿌리"               "기업"               "확인"              
[136] "절차"               "확인"               "유효"              
[139] "기"                 "3"                  "년"                
[142] "사후관리"           "등"                 "규정도"            
[145] "신설"               "뿌리"               "기업"              
[148] "뿌리"               "기술"               "활용"              
[151] "해"                 "사업"               "영위"              
[154] "하"                 "업종"               "뿌리"              
[157] "기술"               "활용"               "되"                
[160] "장비"               "제조"               "분야"              
[163] "말"                 "뿌리"               "기업"              
[166] "확인"               "제"                 "외국"              
[169] "근로자"             "고용"               "우대"              
[172] "혜택"               "등"                 "뿌리"              
[175] "산업"               "관련"               "우대"              
[178] "지원"               "대상"               "것"                
[181] "국가"               "뿌리"               "산업진흥"          
[184] "센터"               "확인서"             "발급"              
[187] "해오"               "2012"               "년"                
[190] "1"                  "만"                 "1766"              
[193] "건"                 "발급"               "5843"              
[196] "건"                 "유효"               "한"                
[199] "것"                 "집계"               "‘일하기"          
[202] "뿌리기업’"         "선정"               "기준"              
[205] "절차"               "지원"               "내용"              
[208] "등"                 "규정도"             "‘일하기"          
[211] "뿌리기업’은"       "뿌리"               "산업"              
[214] "청년층"             "등"                 "신규"              
[217] "인력"               "유입"               "촉진"              
[220] "하기"               "근로·복지"         "환경"              
[223] "성장"               "역량"               "등"                
[226] "우수"               "한"                 "기업"              
[229] "산업"               "부"                 "선정"              
[232] "해"                 "홍보"               "등"                
[235] "지원"               "하"                 "제도"              
[238] "산업"               "부"                 "이번"              
[241] "개정"               "사항"               "시행"              
[244] "수"                 "업종"               "별"                
[247] "협·단체"           "뿌리"               "기업"              
[250] "지자체"             "등"                 "대상"              
[253] "적극"               "홍보"               "할"                
[256] "방침"               "발간"               "하"                
[259] "뿌리"               "산업"               "백서"              
[262] "추가"               "되"                 "8"                 
[265] "대"                 "차세대"             "공정기술"          
[268] "내용"               "기술"               "동향"              
[271] "등"                 "상세"               "하게"              
[274] "제공"               "하기"               "산업"              
[277] "부"                 "관계자"             "“이번"            
[280] "개정"               "2011"               "년"                
[283] "뿌리"               "산업"               "법"                
[286] "제정"               "후"                 "10"                
[289] "년"                 "만"                 "뿌리"              
[292] "기술"               "소재"               "다원화"            
[295] "지능화"             "중심"               "확장"              
[298] "한"                 "것"                 "뿌리"              
[301] "산업"               "기술"               "융복합"            
[304] "화"                 "첨단화"             "촉진"              
[307] "신규"               "인력"               "유입"              
[310] "지원"               "강화"               "하기"              
[313] "토대"               "마련"               "데"                
[316] "의의"               "있다”고"           "말" 
> 

 </div>
<p style="color:#E84D4D;font-size:150%;">
- 명사 분리기 설치 끝 
</p>