上次我们把sequelize给初始化好了，
这次我们配置一下，
跑一跑。

我们直接选MySQL，
毕竟大众数据库，
出了什么问题好解决。

为了避免老是敲长长的命令，
我们先来写一个脚本。

先花个10分钟看看这个学习一下。
http://www.runoob.com/linux/linux-shell-passing-arguments.html

创建了一个用于生成migration的脚本，
很简单，
只有两行。

```shell
# migration-crate.sh
echo "sequelize migration:create --name $1 --options-path sequelizeOptions"
sequelize migration:create --name $1 --options-path sequelizeOptions
```

给这个脚本赋予可执行的权限。
```shell
chmod +x ./migration-create.sh
```

这样就方便了不少。
```
./migration-create.sh name
```

我们来创建第一个migration。
```
./migration-create.sh init
```

这样就有了`/db/migrations/20161219052831-init.js`这个文件，
我们在这个文件里面对数据库做操作，
创建表什么的，
这个文件的名字千万别乱改。

我们先看看里面的初始内容。
```js
'use strict';

module.exports = {
  up: function (queryInterface, Sequelize) {
    /*
      Add altering commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.createTable('users', { id: Sequelize.INTEGER });
    */
  },

  down: function (queryInterface, Sequelize) {
    /*
      Add reverting commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.dropTable('users');
    */
  }
};
```
