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
# 添加一条数据
insert into yonghu values ('123');
# 查询用户表所有字段
select * from yonghu;
# 删除用户表所有数据
delete from yonghu;
# 改变表名
alter table yonghu rename xx;
# 删除表
drop table xx;
# 导入.sql文件
source /root/mysql.sql;
# 批量更改 article 表中的 type 字段
UPDATE article a
  LEFT JOIN article b
  ON a.id = b.id
  SET a.type = 'markdownEditor'
```
