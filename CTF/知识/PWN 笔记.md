## 格式化字符串漏洞
应用于 printf 家族的不规范使用
```cpp
printf("format",output);//标准格式
printf("%s",str);//这个很规范
printf(str);//虽然这个也可以输出,但会出现问题
```
format 参数告诉程序改用什么格式输出 str
比如 %s 就是把 str 以字符串形式输出
%d 以 int 类型输出

但是当没有 format 参数直接上 output
程序就会把 output 当做 format 参数使用
也就是说,如果 outputdan
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNzE0OTg5MDZdfQ==
-->