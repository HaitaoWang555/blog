## 命令

```
yum -y update // 更新
```

## 安装node
[下载地址](https://npm.taobao.org/mirrors/node/)
[文档地址](https://github.com/nodejs/help/wiki/Installation)
```
wget https://npm.taobao.org/mirrors/node/v10.15.0/node-v10.15.0-linux-x64.tar.xz
sudo mkdir -p /usr/local/lib/nodejs
sudo tar -xJvf node-v10.15.0-linux-x64.tar.xz -C /usr/local/lib/nodejs

export PATH=/usr/local/lib/nodejs/ node-v10.15.0-linux-x64/bin:$PATH

npm config set registry https://registry.npm.taobao.org
```

## 安装mysql
[下载地址](https://dev.mysql.com/downloads/repo/yum/)

```
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum update
yum install mysql-server

// 权限设置
chown mysql:mysql -R /var/lib/mysql
// 初始化 MySQL
mysqld --initialize
// 启动 MySQL
systemctl start mysqld
// 查看 MySQL 运行状态
systemctl status mysqld
```
