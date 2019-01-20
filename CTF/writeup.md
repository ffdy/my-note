# Re1 WriteUp
打开程序，随便输入什么，没什么发现

用 IDA 打开程序，大致浏览，并没有发现什么

Shift+F12，发现出现程序中的字符：
![kPpFLd.md.png](https://s2.ax1x.com/2019/01/20/kPpFLd.md.png)
查看对应的汇编源码，发现初始化了大量的变量，并且地址连续：
![kPCQVs.png](https://s2.ax1x.com/2019/01/20/kPCQVs.png)
将其转化为字符串，得到 Flag：
![kPCwZ9.png](https://s2.ax1x.com/2019/01/20/kPCwZ9.png)
提交 Flag：BUPT{R3_1s_qul7e_3!mple:)}
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc0MTkwNzczNSwzMDM4MjA0MzMsMTEyNT
c2NTU4OCw1NTIzMTY2MjVdfQ==
-->