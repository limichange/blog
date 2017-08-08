## 场景

在Github上Fork了一个项目后慢慢看，
在某一天，
这个项目更新了，
这个时候你也需要更新，
这个时候怎么办呢？
超简单，
按步骤来就可以了。

## 首先

你需要一个Fork的项目，
这个很简单，
上Github上点一下Fork就可以了。

## 更新

下载自己Fork的项目。
```bash
$ git clone git@github.com:limichange/docs.git
Cloning into 'docs'...
remote: Counting objects: 598, done.
remote: Compressing objects: 100% (76/76), done.
remote: Total 598 (delta 39), reused 0 (delta 0), pack-reused 522
Receiving objects: 100% (598/598), 111.61 KiB | 3.00 KiB/s, done.
Resolving deltas: 100% (287/287), done.
```

进去。
```bash
$ cd docs
```

添加源项目。
```bash
$ git remote add upstream https://github.com/nuxt/docs.git
```

下载源项目最新的代码。
```bash
$ git fetch upstream
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 6 (delta 5), reused 6 (delta 5), pack-reused 0
Unpacking objects: 100% (6/6), done.
From https://github.com/nuxt/docs
 * [new branch]      master     -> upstream/master
 ```

合并。
```bash
$ git merge upstream/master
Updating 3790576..8b3978b
Fast-forward
 en/guide/commands.md | 2 +-
 en/guide/pages.md    | 4 +++-
 2 files changed, 4 insertions(+), 2 deletions(-)
```

Push到自己那边。
```bash
$ git push
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 673 bytes | 0 bytes/s, done.
Total 6 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), completed with 5 local objects.
To github.com:limichange/docs.git
  3790576..8b3978b  master -> master
```

完。

## 相关
http://www.cnblogs.com/zyumeng/p/3442263.html
http://geek.csdn.net/news/detail/67091
