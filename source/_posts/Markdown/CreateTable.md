---
title: "MarkDown Table, hr, `code`"
date: 2021-12-06 18:31:04
categories:
- Markdown
tags:
- Study
- markdown
- table
- table arrange
- pipe
---

## MarkDown Table

마크다운 문법에서 테이블 그리기 
- pipe로 table을 쉽게 생성 할 수 있다. 

<hr>

- 아래 표는 
[table ref.](https://yoonhwa-p.github.io/2021/12/06/python/nameNnamespace)
에서 가져온 것.

```html
|  종류     |      기호      |      ex       |  exp  |
|:-----------:|:-------------:|:-------------:|:------:|
| assignment | =  | a = 10 | 10에 a 를 바인딩 |
| assignmented assignment |  **=, +=, -=, *=, //=, %=. <<=, >>=, &=, &#124;=, ^=, @=  | a+= 10  |a에 10을 더한 결과 객체에 a를 바인딩 |


```

대충 칸에 맞게 순서만 맞으면
아래와 같이 예쁜 표를 만들 수 있다. 


|  종류     |      기호      |      ex       |  exp  |
|:-----------:|:-------------:|:-------------:|:------:|
| assignment | =  | a = 10 | 10에 a 를 바인딩 |
| assignmented assignment |  **=, +=, -=, *=, //=, %=. <<=, >>=, &=, &#124;=, ^=, @=  | a+= 10  |a에 10을 더한 결과 객체에 a를 바인딩 |

<br><br>

### table 안에 정렬

```html
<!-- 하이픈 갯수로 크기 조절 -->
|왼쪽정렬 | 중간정렬|오른쪽정렬|
|:---------|:---------------:|---------:| 

```

|왼쪽정렬|중간정렬|오른쪽정렬|
|:---------|:---------------:|---------:| 
|왼쪽정렬|중간정렬|오른쪽정렬|




<br><br><hr>

### pipe 보이기 

pipe는 Enrer Key 위에 있다. 

```html
 &#124 
```
하지만, Markdown형식에서 보이지 않으닌까 \ |

만약 사용 하고 싶으면, `&#124` 를 사용 하도록 한다. ^0^



table만드는 것이 귀찮다면 아래 링크를 타고 가보쟈 

[table](https://www.tablesgenerator.com/markdown_tables) 쉽게 만드는 곳


<br><br><hr>


### code 강조하기 

inline code 강조하는 방법

```html
<!--Tab key 위쪽, 1 왼쪽에 위치한 Grave 키 이용-->

`code`

```

`code` 이것이 가능하다니, 이제야 알았다. !!!

<br><br><hr>

### 수평선

```html
---
(hyphen X 3)

***
(asterisk X 3)

___
(Underscore X 3)
```

---
(hyphen X 3)

***
(asterisk X 3)

___
(Underscore X 3)


[Ref.](https://heropy.blog/2017/09/30/markdown/)

