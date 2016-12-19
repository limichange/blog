有这么一个场景，
你从别处弄来了一个工程，
里面塞满了html文件，
此时你需要看看这个工程是什么样的，
于是你随便打开了一个html文件，
发现整个网页是乱七八糟的，
没有CSS也没有JS，
因为你打开的方式是file而不是http，
这个时候你就需要快速的在这个文件夹下创建一个http服务器，
你需要`http-server`。

安装。
```
sudo npm install http-server -g
```

然后跳到那个项目的目录下，
运行命令。
```
➜  blog git:(master) ✗ http-server .
Starting up http-server, serving .
Available on:
  http://127.0.0.1:8080
  http://192.168.2.194:8080
Hit CTRL-C to stop the server
```

打开他提示的地址，
一切ok，
是不是很方便？

> http-server
> A simple zero-configuration command-line http server
> https://www.npmjs.com/package/http-server
