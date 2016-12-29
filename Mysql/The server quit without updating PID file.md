这是一个在Mac启动Mysql的时候产生的一个报错。

## 1

查看此文件的详情
/usr/local/var/mysql/your_computer_name.local.err

## 2

查看是否正在运行
```shell
$ ps -ef | grep mysql
```

停止服务
```shell
$ kill -9 PID
```

## 3

查看权限
```shell
$ ls -laF /usr/local/var/mysql/
```

修改执行的权限
```shell
$ sudo chown -R mysql /usr/local/var/mysql/
```

## 4

重新运行
```shell
$ mysql.server start
```
