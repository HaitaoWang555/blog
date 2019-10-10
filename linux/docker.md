## 命令

### centos安装 docker-ce
[文档地址](https:#docs.docker.com/install/linux/docker-ce/centos/)
```bash
# 安装所需的软件包
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

# 设置稳定的存储库。
sudo yum-config-manager \
    --add-repo \
    http:#mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 更新 yum 缓存
sudo yum makecache fast

# 安装 Docker-ce
sudo yum -y install docker-ce

# 启动docker并设置为开机启动(centos7)
systemctl  start docker.service
systemctl  enable docker.service
```
