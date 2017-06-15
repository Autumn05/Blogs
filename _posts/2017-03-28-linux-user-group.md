---
layout: post
title: linux中user、group和权限
date: 2017-03-28
tag: 运维
---

* TOC
{:toc}


# Linux用户和用户组的日常管理

## 用户（user）

linux用户存在/etc/passwd 中，密码加密存在/etc/shadow 中
- /etc/passwd 的内容理解：

在/etc/passwd 中，每一行都表示的是一个用户的信息；一行有7个段位；每个段位用:号分割，比如下面是我的系统中的/etc/passwd 的两行；


    beinan:x:500:500:beinan sun:/home/beinan:/bin/bash

    linuxsir:x:505:502:linuxsir open,linuxsir office,13898667715:/home/linuxsir:/bin/bash

    beinan:x:500:500:beinan sun:/home/beinan:/bin/bash

    linuxsir:x:501:502::/home/linuxsir:/bin/bash


    第一字段：用户名（也被称为登录名）；在上面的例子中，我们看到这两个用户的用户名分别是 beinan 和linuxsir；

    第二字段：口令；在例子中我们看到的是一个x，其实密码已被映射到/etc/shadow 文件中；

    第三字段：UID ；请参看本文的UID的解说；

    第四字段：GID；请参看本文的GID的解说；

    第五字段：用户名全称，这是可选的，可以不设置，在beinan这个用户中，用户的全称是beinan sun ；而linuxsir 这个用户是没有设置全称；

    第六字段：用户的家目录所在位置；beinan 这个用户是/home/beinan ，而linuxsir 这个用户是/home/linuxsir ；

    第七字段：用户所用SHELL 的类型，beinan和linuxsir 都用的是 bash ；所以设置为/bin/bash ；    

- 关于UID 的理解：

