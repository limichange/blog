## 开头
Nginx入门可以看看这篇，
主要是学一些基本的用法。

## 安装运行
```shell
# 安装
$ brew install nginx

# 查看版本
$ nginx -v
nginx version: nginx/1.10.2

# 运行
$ nginx
```

查看`http://localhost:8080/`会看到欢迎的页面。

## 配置
```shell
# 停止
$ nginx -s stop
```

此时页面就无法访问了。
我们来看看配置文件是什么样的。

```shell
$ vim /usr/local/etc/nginx/nginx.conf
```

文件还挺大的，
为了方便看，
我们先把注释的部分删掉。
为了方便看，
就直接把解释放进去了。

```conf
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       8080;
        server_name  localhost;

        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    include servers/*;
}
```

## 相关资料
http://learnaholic.me/2012/10/10/installing-nginx-in-mac-os-x-mountain-lion/
http://nginx.org/en/docs/beginners_guide.html
http://tengine.taobao.org/documentation_cn.html
http://webres.wang/nginx-config-summary/
https://fraserxu.me/2013/06/22/Nginx-for-developers/
http://www.ha97.com/5194.html
http://www.wiquan.com/article/597
