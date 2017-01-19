## 场景

是不是有人在commit前不lint，
导致推送了垃圾代码？
那我们就让你不lint就commit不了。

## 方法
我们使用`pre-commit`来实现这个需求，
让我们简单的配置一下。

```bash
$ npm install pre-commit --save-dev
```

```json
{
  "scripts": {
    "lint": "eslint ./ --cache --ignore-pattern .gitignore"
  },
  "pre-commit": [ "lint" ],
  "devDependencies": {
    "eslint": "^2.12.0",
    "pre-commit": "^1.1.3"
  }
}
```

就这样。
当然我们也可以在commit之前添加测试或其他命令，
记住要抛出报错。
```json
{
  "scripts": {
    "lint": "eslint ./ --cache --ignore-pattern .gitignore",
    "precommit-msg": "echo 'Pre-commit checks...' && exit 0"
  },
  "pre-commit": [ "precommit-msg", "lint" ],
  "devDependencies": {
    "eslint": "^2.12.0",
    "pre-commit": "^1.1.3"
  }
}
```

## 参考
http://elijahmanor.com/npm-precommit-scripts/
