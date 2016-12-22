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

```shell
# nginx worker进程数，建议设置为等于CPU总核心数。
worker_processes  1;

# events模块设置事件模型与连接数上限
events {

    # 单个进程最大连接数（最大连接数 = 连接数 * 进程数）
    worker_connections  1024;
}

# 设定http服务器，利用它的反向代理功能提供负载均衡支持，这是nginx的核心模块
http {

    # 设定mime类型，类型由mime.type文件定义(文件扩展名与文件类型映射表)
    include       mime.types;

    # 指定默认文件类型为二进制流
    default_type  application/octet-stream;

    # 用于开启高效文件传输模式
    sendfile        on;

    # 长连接超时时间，单位是秒
    keepalive_timeout  65;

    # 虚拟主机的配置
    server {

        # 配置监听端口
        listen       8080;

        # 配置访问域名
        server_name  localhost;

        # 配置根路径
        location / {
            root   html;
            index  index.html index.htm;
        }

        # 配置错误页
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    include servers/*;
}
```

我们把指向的目录改一下，
重启下，
页面的内容就变了。

```shell
# 配置根路径
# /Users/limi 这个目录下需要有一个index.html或index.htm
location / {
    root   /Users/limi;
    index  index.html index.htm;
}
```

这配置只是其中的一小部分，
还有很多的功能可以开启。

## 相关资料
http://learnaholic.me/2012/10/10/installing-nginx-in-mac-os-x-mountain-lion/
http://nginx.org/en/docs/beginners_guide.html
http://tengine.taobao.org/documentation_cn.html
http://webres.wang/nginx-config-summary/
https://fraserxu.me/2013/06/22/Nginx-for-developers/
http://www.ha97.com/5194.html
http://www.wiquan.com/article/597
