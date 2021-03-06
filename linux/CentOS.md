## 命令

```bash
# 更新
yum -y update
# 列出所有端口
yum install net-tools
netstat -ntlp
# zip压缩包
yum install -y unzip zip
```

## CentOS7修改SSH端口
```
vi /etc/ssh/sshd_config
Port 22
systemctl restart sshd.service
# 登录指定端口
ssh -p xxx root@xx.xx.x.xx
```

## 安装node
[下载地址](https://npm.taobao.org/mirrors/node/)
[文档地址](https://github.com/nodejs/help/wiki/Installation)
```bash
yum -y install wget
wget https://npm.taobao.org/mirrors/node/v12.18.0/node-v12.18.0-linux-x64.tar.xz
sudo mkdir -p /usr/local/lib/nodejs
sudo tar -xJvf node-v12.18.0-linux-x64.tar.xz -C /usr/local/lib/nodejs

echo 'export PATH=/usr/local/lib/nodejs/node-v12.18.0-linux-x64/bin:$PATH' > /etc/profile.d/node.sh
chmod +x /etc/profile.d/node.sh
source /etc/profile

node -v
npm -v
npm config set registry https://registry.npm.taobao.org
# 使用 sudo npm
sudo ln -s /usr/local/lib/nodejs/node-v12.18.0-linux-x64/bin/node /usr/bin/node
sudo ln -s /usr/local/lib/nodejs/node-v12.18.0-linux-x64/bin/npm /usr/bin/npm


# pm2部署
npm i pm2 -g
pm2 start npm --name "name" -- run start
# 保存服务
pm2 save
# 把已启动服务加到systemd中
pm2 startup
# 重启，发现之前的服务都已经启动
systemctl reboot
# 删除自动启动服务
pm2 unstartup systemd
# 如果代码发生变动，需要重新save、startup
# 查看启动服务列表
pm2 list
# 显示APP-NAME日志 
pm2 logs APP-NAME       
# 刷新所有日志 
pm2 flush    
# 重新加载所有日志             
pm2 reloadLogs
# 删除指定应用 id 0
pm2 delete 0                  
```

## 安装java
```bash
yum -y install java-1.8.0-openjdk.x86_64
yum -y install java-1.8.0-openjdk-devel.x86_64
java -version
# 启动java服务
nohup java -jar blog-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod &
# 开机自启服务
# 编写启动脚本 最后一行要有空行
touch /blog/blog-admin-service.sh
vi /blog/blog-admin-service.sh

#!/bin/bash
cd /blog/
nohup java -jar /blog/wht-blog/item-admin/target/item-admin-1.0-SNAPSHOT.jar --spring.profiles.active=prod &
echo $! > /blog/blog-admin-service.pid


# 编写结束脚本
touch /blog/stop-blog-admin-service.sh
vi /blog/stop-blog-admin-service.sh

#!/bin/sh
PID=$(cat /blog/blog-admin-service.pid)
kill -9 $PID

# 增加可执行权限
cd /blog
chmod +x blog-admin-service.sh blog-portal-service.sh stop-blog-admin-service.sh stop-blog-portal-service.sh

# 编写注册服务
cd /usr/lib/systemd/system
touch blog-admin-service.service
vi blog-admin-service.service

[Unit]
Description=blog-admin-service
After=network.target

[Service]
Type=forking
ExecStart=/blog/blog-admin-service.sh
ExecStop=/blog/stop-blog-admin-service.sh
PrivateTmp=true
 
[Install]
WantedBy=multi-user.target

# 
systemctl enable blog-admin-service.service #开机自启动
systemctl stop blog-admin-service.service  #停止
systemctl start blog-admin-service.service  #启动

# 杀死java服务
ps -aux | grep java
kill -s 9 xxxxxx

#安装 maven
wget http://mirrors.hust.edu.cn/apache/maven/maven-3/3.1.1/binaries/apache-maven-3.1.1-bin.tar.gz
tar -zxf apache-maven-3.1.1-bin.tar.gz -C /usr/local/lib/
echo 'export PATH=/usr/local/lib/apache-maven-3.1.1/bin:$PATH' > /etc/profile.d/maven3.sh
chmod +x /etc/profile.d/maven3.sh
source /etc/profile
mvn -v
mvn -DskipTests=true package
```

## 安装git
```bash
yum install git
```

## 安装mysql
[下载地址](https://dev.mysql.com/downloads/repo/yum/)
[安装文档](https://dev.mysql.com/doc/refman/8.0/en/linux-installation-yum-repo.html)

```bash
cd /tmp
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
rpm -ivh mysql80-community-release-el7-3.noarch.rpm
yum update
sudo yum install mysql-community-server

# 设置字符 utf8mb4
vi /etc/my.cnf
[mysqld]
character-set-server = utf8mb4
collation-server = utf8mb4_bin
[client]
default-character-set=utf8mb4

# 启动 MySQL
sudo service mysqld start
# 查看 MySQL 运行状态
systemctl status mysqld
# 验证
netstat -anp|grep 3306

# 登陆验证
mysql -uroot -p密码
show databases;
show variables like 'character%';
# 创建数据库
CREATE DATABASE blog DEFAULT CHARACTER SET utf8mb4;
# 运行.sql
mysql -u root -p123456 --default-character-set=utf8mb4 blog < /root/blog/blog-serve-java/bash/blog.sql
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
[文档地址](http://nginx.org/en/docs/configure.html)

```bash
# 安装编译工具及库文件
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel

wget http://nginx.org/download/nginx-1.16.1.tar.gz
wget https://ftp.pcre.org/pub/pcre/pcre-8.43.tar.gz
wget http://prdownloads.sourceforge.net/libpng/zlib-1.2.11.tar.gz

tar -xzf nginx-1.16.1.tar.gz
tar -xzf pcre-8.43.tar.gz
tar -xzf zlib-1.2.11.tar.gz

cd nginx-1.16.1
./configure \
    --sbin-path=/usr/local/nginx/nginx \
    --conf-path=/usr/local/nginx/nginx.conf \
    --pid-path=/usr/local/nginx/nginx.pid \
    --with-http_ssl_module \
    --with-pcre=../pcre-8.43 \
    --with-zlib=../zlib-1.2.11

make && make install

# 启动
/usr/local/nginx/nginx

stop — fast shutdown
quit — graceful shutdown
reload — reloading the configuration file
reopen — reopening the log files

nginx -s quit
# 开机启动
cd /lib/systemd/system/
vi nginx.service
[Unit]
Description=nginx service
After=network.target 
   
[Service] 
Type=forking 
ExecStart=/usr/local/nginx/nginx
ExecReload=/usr/local/nginx/nginx -s reload
ExecStop=/usr/local/nginx/nginx -s quit
PrivateTmp=true 
   
[Install] 
WantedBy=multi-user.target
# 重载systemctl
systemctl daemon-reload
# 启动nginx服务
systemctl start nginx.service
# 停止服务
systemctl stop nginx.service
# 重新启动服务
systemctl restart nginx.service
# 查看所有已启动的服务
systemctl list-units --type=service     
# 查看服务当前状态
systemctl status nginx.service         
# 设置开机自启动
systemctl enable nginx.service
# 停止开机自启动
systemctl disable nginx.service
```

## 防火墙
```bash
# 安装防火墙
yum -y install firewalld firewall-config

# 为了启动防火墙，要先重启下 dbus
systemctl restart dbus

# 启动一个服务
systemctl start firewalld.service

# 关闭一个服务
systemctl stop firewalld.service
 
# 重启一个服务
systemctl restart firewalld.service

# 显示一个服务的状态
systemctl status firewalld.service
 
# 在开机时启用一个服务
systemctl enable firewalld.service

# 在开机时禁用一个服务
systemctl disable firewalld.service

# 查看服务是否开机启动
systemctl is-enabled firewalld.service
 
# 查看已启动的服务列表
systemctl list-unit-files|grep enabled
 
# 查看启动失败的服务列表
systemctl --failed

# 查看版本
firewall-cmd --version

# 查看帮助
firewall-cmd --help

# 显示状态
firewall-cmd --state

# 查看所有打开的端口
firewall-cmd --zone=public --list-ports

# 更新防火墙规则
firewall-cmd --reload
 
# 查看区域信息
firewall-cmd --get-active-zones

# 查看指定接口所属区域
firewall-cmd --get-zone-of-interface=eth0
 

# 拒绝所有包，测试别用这个。不如然只有到VMWare 的终端上去关闭防火墙 SSH 客户端，稍显麻烦：
firewall-cmd --panic-on

# 取消拒绝状态
firewall-cmd --panic-off

# 查看是否拒绝
firewall-cmd --query-panic

# 开启一个端口 --permanent永久生效
firewall-cmd --zone=public --add-port=80/tcp --permanent 
# 重新载入
firewall-cmd --reload
# 查看
firewall-cmd --zone=public --query-port=80/tcp
# 删除
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

## 安装Redis 
```bash
# 添加EPEL存储库
sudo yum install epel-release
sudo yum update

# 安装Redis
sudo yum install redis
# 启动Redis
sudo systemctl start redis
# 开机启动
sudo systemctl enable redis
# 验证安装
redis-cli ping
PONG
# 配置Redis 
# 持久性选项
/etc/redis.conf
appendonly yes
appendfsync everysec
curl https://localhost:8081/api/manage/poetry/search?pageNum=1&pageSize=20&author=%E6%9D%8E%E7%99%BD
sudo systemctl restart redis
# 基本系统调整
sudo sysctl vm.overcommit_memory=1
# 永久生效
vi /etc/sysctl.conf
vm.overcommit_memory = 1
# 设置密码
/etc/redis.conf
requirepass xxxxx

```
## 安装 ElasticSearch

[下载地址](https://www.elastic.co/downloads/elasticsearch)

```bash
tar -zxvf elasticsearch-7.8.0-linux-x86_64.tar.gz

# 在解压后的文件夹里创建一个data文件夹，一会存数据文件用
cd elasticsearch-7.8.0
mkdir data
```
```
目录结构介绍：
bin：可执行文件，运行es的命令
config：配置文件目录
 config/elasticsearch.yml：ES启动基础配置
 config/jvm.options：ES启动时JVM配置
 config/log4j2.properties：ES日志输出配置文件
lib：依赖的jar
logs：日志文件夹
modules：es模块
plugins：可以自己开发的插件
data：我们自己创建的，存放es存储文件
```

```bash
# 安装elasticsearch-analysis-ik
cd plugins
mkdir analysis-ik
cd analysis-ik
wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.8.0/elasticsearch-analysis-ik-7.8.0.zip
unzip elasticsearch-analysis-ik-7.8.0.zip
```

```bash
vi /etc/sysctl.conf
vm.max_map_count = 262144
sudo sysctl -p
```

```bash
# 创建用户并赋予es安装目录权限
useradd -c 'ES user' -d /home/esroot esroot
passwd esroot

chown -R esroot elasticsearch-7.8.0
chmod -R 775 elasticsearch-7.8.0
```
vi /etc/security/limits.conf
esroot soft nofile 100001
esroot hard nofile 100002
root soft nofile 100001
root hard nofile 100002

```

su esroot

cd /bin
./elasticsearch -d
```

```bash
ps -ef | grep elastic

kill -9 2382（进程号)
```

```
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.8.0-linux-x86_64.tar.gz
tar -zxvf kibana-7.8.0-linux-x86_64.tar.gz

chown -R esroot kibana-7.8.0-linux-x86_64
chmod -R 775 kibana-7.8.0-linux-x86_64
su esroot
vi config/kibana.yml

server.host: "0.0.0.0"
i18n.locale: "zh-CN"
```

## wget

```bash
wget -m -e robots=off
```