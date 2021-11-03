---
layout: post
title: "Deploy gitHub blog by hexo"
date: 2021-11-04 08:21:20
tags: makeBlog
---



##github blog를 Hexo를 이용하여 deploy 해 보자.
    이전에 만들었던 포스팅이 컴퓨터가 바뀌면서 사라졌더군

Ref.

https://dschloe.github.io/settings/hexo_blog/


###Hexo를 이용하여 블로그를 만들어 보자 


#### Node js를 download 해 준다. 
+ [Node js](https://nodejs.org/en/) 설치
+ [GitBash](https://git-scm.com/) 에서 node의 version을  확인

```bash
$ node -v
```
+ Hexo를 설치 해 준다. 
  + Hexo의 경우  npm을 이용하여 설치 한다. 

```bash
$ npm install -g hexo-cli
```



#### git bash를 적당한 경로에서 들어간다. 
makeBlog folder를 만들어준다. 

``` bash

$ mkdir makeBlog
$ cd makeBlog
$ hexo init myblog

```

hexo init 을 myblog에서 해 준다. 
hexo server와 deployer를 설치 해 준다. 

이를 설치 하지 않으면 에러가 날 수 있다. 

``` bash
$ cd myblog
$ npm install
$ npm install hexo-server --save
$ npm install hexo-deployer-git --save
```



