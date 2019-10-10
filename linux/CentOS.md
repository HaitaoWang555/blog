## 命令

```bash
# 更新
yum -y update
```

## 安装node
[下载地址](https://npm.taobao.org/mirrors/node/)
[文档地址](https://github.com/nodejs/help/wiki/Installation)
```
yum -y install wget
wget https://npm.taobao.org/mirrors/node/v10.15.0/node-v10.15.0-linux-x64.tar.xz
sudo mkdir -p /usr/local/lib/nodejs
sudo tar -xJvf node-v10.15.0-linux-x64.tar.xz -C /usr/local/lib/nodejs

export PATH=/usr/local/lib/nodejs/node-v10.15.0-linux-x64/bin:$PATH
node -v
npm -v
npm config set registry https://registry.npm.taobao.org
```

## 安装java
```bash
yum -y install java-1.8.0-openjdk.x86_64
java -version
```

## 安装git
```bash
yum install git
```

## 安装mysql
[下载地址](https://dev.mysql.com/downloads/repo/yum/)

```bash
cd /tmp
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm  
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum update
yum install mysql mysql-server mysql-devel -y

# 启动 MySQL
systemctl start mysql.service
# 查看 MySQL 运行状态
systemctl status mysqld
# 验证
netstat -anp|grep 3306
# 设置密码
mysqladmin -u root password 密码
# 登陆验证
mysql -uroot -p密码
show databases;
```

## ssh连接
> xshell连接本地linux虚拟机速度很慢的解决办法
```bash
# 修改
vi /etc/ssh/sshd_config

#UseDNS yes
UseDNS no
# 重启
systemctl restart sshd
```

## 安装nginx
[下载地址](http://nginx.org/en/download.html)
[文档地址](http://nginx.org/en/docs/)

```bash
# 安装编译工具及库文件
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel

wget http://nginx.org/download/nginx-1.16.1.tar.gz
tar -xzf nginx-1.16.1.tar.gz
cd nginx-1.16.1.tar.gz
```