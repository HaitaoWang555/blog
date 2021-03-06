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
# 修改表字段的长度
alter table 表名 table 字段名  数据类型(修改后的长度)
# 增加某个字段
ALTER TABLE cms_article ADD COLUMN comment_count int(11) NOT NULL DEFAULT '0' COMMENT '文章评论数量';
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
# 删除某个字段
ALTER TABLE cms_article drop COLUMN updated_content_time;
# 删除表
drop table xx;
# 删除数据库
drop database xx;
# 导入.sql文件
source /root/mysql.sql;
# 批量更改 article 表中的 type 字段
UPDATE cms_article a
  LEFT JOIN cms_article b
  ON a.id = b.id
  SET a.updated_content_time = a.created_time
# 获取表行数
SELECT COUNT(*) FROM article;


# 导出数据库结构忽略一张表
mysqldump -h localhost -uroot -p'密码' 数据库名 --ignore-table=数据库名.表名 > 自定义名称.sql

# 导出数据库结构忽略多张表
mysqldump -h localhost -uroot -p'密码' 数据库名 --ignore-table=数据库名.表名1 --ignore-table=数据库名.表名2 > 自定义名字.sql

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

第二个

新建my.ini文件
```
[mysqld]
# 设置3307端口
port=3307
explicit_defaults_for_timestamp=true
default_password_lifetime=0
# 设置mysql的安装目录
basedir=D:\mysql-8.0.21-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\mysql-8.0.21-winx64\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。
max_connect_errors=10

# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB

[mysql]
default-character-set=utf8mb4
[mysqld]
# 不对mysqld 的导入|导出做限制
secure_file_priv =
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3307
default-character-set=utf8mb4
```
用管理员命令模式进入mysql/bin目录，进行mysql初始化安装
mysqld --initialize --console
控制台会打印出密码，请记住初始化密码，如果忘记把根目录data文件夹删除，重新执行上面语句。
mysqld --install mysql1
net start mysql1
mysql -uroot -p
alter user 'root'@'localhost' identified with mysql_native_password by '123456';



## centos7 安装 mysql8

```bash
# 配置Mysql 8.0安装源
sudo rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
# 安装Mysql 8.0
sudo yum --enablerepo=mysql80-community install mysql-community-server
# 启动 MySQL
systemctl start mysql.service
# 查看 MySQL 运行状态
systemctl status mysqld
# 验证
netstat -anp|grep 3306
# 查看root临时密码
grep "A temporary password" /var/log/mysqld.log
# 更改密码
mysql -uroot -p
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';
```

```bash

SHOW VARIABLES LIKE '%sql_mode%';
SHOW VARIABLES LIKE '%explicit_defaults_for_timestamp%';
SET @@SESSION.explicit_defaults_for_timestamp = 'OFF';

vi  /etc/my.cnf
explicit_defaults_for_timestamp=false
```
## 导入csv

```bash
load data local infile "E:\\csv\\weijinmonanbeichaochu.csv"
into table poetry
fields terminated by ',' optionally enclosed by '"' lines terminated by '\r\n'
ignore 1 lines
(title, dynasty, author, content);
```