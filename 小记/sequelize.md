sequelize是nodejs的一个ORM，
就是连接数据库的，
因为之前用过，
有点印象，
所以这次就直接拿过来用了。

先全局装一个命令行工具。
```
sudo npm install -g sequelize-cli
```

然后本地项目再装一个。
```
sudo npm install --save sequelize
```

ok，
很顺利。

先看一个版本。
```
sequelize version

-> Sequelize [Node: 6.9.1, CLI: 2.5.1, ORM: 3.27.0]
```

然后用这个工具初始化一下项目。
```
sequelize init
```

相关的文件和文件就自动创建好了，
但是我发现一个问题，
就是创建的这些文件夹和文件都直接散在根目录了，
有点乱，
需要想一个办法把这些东西直接放在一个文件夹里。

*此处过了半小时*

（扶额），
好啦，
知道怎么搞了。

创建一个目录和配置文件，
目录是放这些生成文件的地方，
配置文件是说明路径的。
```
mkdir db
touch sequelizeOptions.js
```

```js
// sequelizeOptions.js
module.exports = {
  'config': 'db/config/config.json',
  'migrations-path': 'db/migrations',
  'seeders-path': 'db/seeders',
  'models-path': 'db/models',
}
```

初始化的时候指定一下这个配置文件。
```
sequelize init --options-path sequelizeOptions
```

创建一个migration试试。
```
sequelize migration:create --name init --options-path sequelizeOptions
```

打印目录看看。
```
├── config
│   └── config.json
├── migrations
│   └── 20161218091219-init.js
├── models
│   └── index.js
└── seeders
```

估计就是设计的这个样子，
还得自己写一个脚本简化一下这个命令，
每次执行都得写这么多蛮麻烦的。

> 项目地址
> git@github.com:limichange/sequelize-demo.git
