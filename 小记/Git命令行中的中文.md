其实之前一直没在意，
但现在想了想，
不如一了百了。

之前是这样的。
```bash
➜  blog git:(master) ✗ gst
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   "\345\260\217\350\256\260/Object.freeze.md"

no changes added to commit (use "git add" and/or "git commit -a")
```

命令行里敲一句。
```bash
git config --global core.quotepath false
```

于是现在是这样的。
```bash
➜  blog git:(master) ✗ gst
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   小记/Object.freeze.md

no changes added to commit (use "git add" and/or "git commit -a")
```

谢。
http://blog.csdn.net/gxp/article/details/26563579
