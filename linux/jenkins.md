```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum install jenkins

wget https://mirrors.tuna.tsinghua.edu.cn/jenkins/redhat-stable/jenkins-2.235.1-1.1.noarch.rpm
rpm -ivh jenkins-2.235.1-1.1.noarch.rpm


# 修改默认端口8080
vi /etc/sysconfig/jenkins 
JENKINS_PORT="8080"
# 在启动Jenkins的时候直接通过Java选项来关闭Jenkins杀掉所有衍生进程的这个功能

sudo systemctl start jenkins 
sudo systemctl status jenkins

cd /var/lib/jenkins/updates
vi default.json
# 替换所有插件下载的url
:1,$s/http:\/\/updates.jenkins-ci.org\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g
# 替换连接测试url
:1,$s/http:\/\/www.google.com/https:\/\/www.baidu.com/g
:wq


# 卸载Jenkins
rpm -e jenkins

# 删除遗留文件:
find / -iname jenkins | xargs -n 1000 rm -rf
```


```bash
# 关闭 SELINUX

vi /etx/sysconfig/selinux
SELINUX=disabled
```