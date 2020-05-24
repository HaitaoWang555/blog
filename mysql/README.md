## CentOS7  mysql5.6.46

```bash
# 登陆验证
mysql -uroot -p密码
# 查看数据库
show databases;
# 创建blog数据库
CREATE DATABASE blog DEFAULT CHARACTER SET utf8mb4;
# 使用blog数据库
use blog;
# 创建表
create table yonghu(name varchar(20));
# 数据库下的表
show tables;
# 显示表的结构
describe yonghu;
# 修改表字符属性
alter table yonghu character set utf8mb4;
# 添加一条数据
insert into yonghu values ('123');
# 删除一条数据
delete from yonghu where name=123;
# 查询用户表所有字段
select * from yonghu;
# 删除用户表所有数据
delete from yonghu;
# 改变表名
alter table yonghu rename xx;
# 删除表
drop table xx;
# 删除数据库
drop databases xx;
# 导入.sql文件
source /root/mysql.sql;
# 批量更改 article 表中的 type 字段
UPDATE article a
  LEFT JOIN article b
  ON a.id = b.id
  SET a.comment_count = null
# 获取表行数
SELECT COUNT(*) FROM article;

# 导出数据库结构.sql文件
mysqldump -h localhost -uroot -p'123456' -d blog > dump.sql
# 导出数据库结构及数据.sql文件
mysqldump -h localhost -uroot -p'123456' blog > dump.sql
```

## 重置密码
```bash
# 安全模式启动
/usr/bin/mysqld_safe --skip-grant-tables &

use mysql;
update user set password=password('123456') where user='root';
flush privileges;
exit;

# 杀掉mysqld进程
ps -aux | grep mysqld
kill -s 9 xxxxxx

# 启动
systemctl start mysql.service

# 登录
mysql -uroot -p'密码'
```

## win10 多Mysql

[下载](https://dev.mysql.com/downloads/mysql/)

