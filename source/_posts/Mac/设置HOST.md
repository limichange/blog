## 步骤
目标是`/private/etc/hosts`文件，
打开即可。

```bash
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	localhost
255.255.255.255	broadcasthost
::1             localhost
```

别忘了更新生效，
```bash
$ dscacheutil -flushcache
```

完。

## 参考
https://decoding.wordpress.com/2009/04/06/how-to-edit-the-hosts-file-in-mac-os-x-leopard/
