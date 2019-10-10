## 安装与使用

### nginx for Windows

1. [下载](http://nginx.org/en/download.html)

2. 相关命令  [文档地址](http://nginx.org/en/docs/windows.html)

```
start nginx // 启动 
nginx -s stop 	// 快速关机
nginx -s quit	 // 正常关机
nginx -s reload	 // 更改配置，使用新配置启动新工作进程，正常关闭旧工作进程
nginx -s reopen  // 重新打开日志文件
```

### 配置文件

[文档地址](http://nginx.org/en/docs/)

```
http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;
    gzip  on;
    server {
        listen       80;
        server_name  localhost;
        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            alias  example/; # 指定地址
            autoindex on; # 显示文件内容
            # set $limit_rate 1k; # 限制访问速度
            # root   html;
            index  index.html index.htm;
        }
    }

}
```
