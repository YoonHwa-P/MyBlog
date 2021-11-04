---
<<<<<<< HEAD
title: "to use gitHub in Multi place" 
=======
title: "to use gitHub in Multi place " 
>>>>>>> c5589a672e850f5edca309f81ca50915cbf17ada
date: 2021-11-02 14:21:20
tags: 
- python_basic
- github
---



깃허브 노트북과 데스크탑 두군데서 
깃허브를 동시에 사용하고 싶은 분들은 
아래 코드 확인해서 해보세요.

이 때, 깃헙 blog 저장소 삭제할 필요가 없어요~


``` bash

$ hexo init myblog # 여기는 각자 소스 레포 확인
$ cd myblog
$ git init 
$ git remote add origin https://github.com/rain0430/myblog.git # 각자 소스 레포 주소
$ git pull --set-upstream origin main # 에러 발생
$ git clean -d -f
$ git pull --set-upstream origin main # 에러 발생 안함 / 소스 확인
$ npm install 
$ hexo clean
$ hexo generate
$ hexo server 
```

<br>



    저장 해 놓고 나중에 해 봐야지 .