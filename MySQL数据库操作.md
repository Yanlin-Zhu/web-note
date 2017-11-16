# MySQL数据库操作
进入数据库
mysql -u用户名 -p密码；

展示数据库
show databases；

进入到某一个数据库
use 数据库名；

展示表
show tables；

查询表结构
desc 表名称；

#### 查询

加索引、分表加快查询

SELECT * FROM 表名称；

SELECT \`字段名\` FROM 表名 where \`字段名\`=xxx;

SELSCT * FROM limit 0,5;从位置0开始查询5条可用于做分页