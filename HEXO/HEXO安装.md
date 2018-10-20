---
title:
tags:
categories:
mathjax: true
---



<!--more-->
- 安装git
- 配置账号
	> 1. 用户名: `git config --global user.name "ffdy"`
	> 2. Email: `git config --global user.email 1836017424@qq.com`
- 生成ssh秘钥
	> 1. `ssh-keygen`+三下`Enter`~~(就是回车)~~
	> 2. Linux环境下`cd ~/.ssh`
	> 3. 查看ssh秘钥`cat ~/.ssh/id_rsa.pub`
	> 4. copy ssh秘钥 添加到`github`or`coding`
	> 5. 回到终端:
		github:`ssh git@github.com`
		coding:`ssh git@git.coding.net`
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MTgyODUxODEsLTc3MTM1ODIzM119
-->