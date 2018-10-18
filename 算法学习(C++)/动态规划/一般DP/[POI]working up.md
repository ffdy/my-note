---
title: [POI]working up
tags: DP
categories: 算法学习
top: false

---
# [POI]working up
### 题意
> 有两个人和一个$n \times m$的矩阵,一个从左上角出发,只能向右,向下走,去右下角;另一个从右上角出发只能向左,向下走,去左下角.矩阵的每一格有一个权值,走过会得到这个权值.很显然,两个人肯定会相遇,他们相遇的那一格的权值不取,求两人只相遇一次权值和的最大值

### 题解
> **读完题,你是否有想到`方格取数`
> 做法非常简单也非常暴力,开四个二维数组分别记录从四个角出发的最优解,然后$n^2$枚举合法的点,更新$ans$
> 因为只相遇一次,
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzMxODczNzQsODMxMTI3NjM4XX0=
-->