## 格式化字符串漏洞
应用于 printf 家族的不规范使用
```cpp
printf("format",输出);//标准格式
printf("%s",str);//这个很规范
printf(str);//虽然这个也可以输出,但会出现问题
```
forat
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ2NDE5MTE5OF19
-->