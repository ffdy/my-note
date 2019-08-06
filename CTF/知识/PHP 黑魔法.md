# $_REQUESTS
PHP这个`$_REQUESTS`里面是有个使用的变量的顺序的.​ 我在write-up中看到有一位同学说了POST会覆盖GET, 这是对的, 但是为什么呢?​ 在PHP的配置中, 有个的东西是`**_variables_order=EGPCS_**`​ 这个代表了php在加载 `**_$_REQUESTS_**`时, 会按照 `**_$_ENV_**`(环境变量), `**_$_GET_**`(GET相关参数), `**_$_POST_**`(POST相关参数), `**_$_COOKIE_**`(COOKIE键值), `**_$_SERVER_**`(服务器及运行环境相关信息)的顺序进行加载和覆盖.​

这里的$_REQUEST有一个特性，就是当GET和POST都存在同一个变量名的时候，只获取POST中的值
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUxNDY5NDM1NF19
-->