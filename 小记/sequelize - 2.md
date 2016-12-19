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

我们来看看文档，
http://docs.sequelizejs.com/en/v3/docs/migrations/

ok，
仿写了一个，
创建了一个表，
定义了一些列。

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
    queryInterface.createTable('user', {
      id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true
      },
      createdAt: {
        type: Sequelize.DATE
      },
      updatedAt: {
        type: Sequelize.DATE
      },
      nickname: {
        type: Sequelize.STRING,
        allowNull: false
      },
      password: {
        type: Sequelize.STRING,
        allowNull: false
      },
      username: {
        type: Sequelize.STRING,
        allowNull: false
      }
    });
  },

  down: function (queryInterface, Sequelize) {
    /*
      Add reverting commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.dropTable('users');
    */
    queryInterface.dropTable('user')
  }
};
```

现在我们准备一下MySQL，
把MySQL打开，
在项目上装上MySQL的依赖。
```shell
sudo npm i --save mysql
```

开始运行脚本。
```shell
sequelize db:migrate --options-path sequelizeOptions.js
```

等一下，
写成脚本比较好。
```shell
# migrate
echo "sequelize db:migrate --options-path sequelizeOptions.js"
sequelize db:migrate --options-path sequelizeOptions.js
```

好了。
```shell
➜  sequelize-demo git:(master) ✗ ./migration.sh
sequelize db:migrate --options-path sequelizeOptions.js

Sequelize [Node: 6.9.1, CLI: 2.5.1, ORM: 3.27.0]

Loaded configuration file "db/config/config.json".
Using environment "development".
Unable to connect to database: SequelizeConnectionError: ER_BAD_DB_ERROR: Unknown database 'database_development'
```

忘了定义数据库了，
连接到MySQL上把这个库创建一下。

*创建中*

再次运行。
```shell
➜  sequelize-demo git:(master) ✗ ./migration.sh
sequelize db:migrate --options-path sequelizeOptions.js

Sequelize [Node: 6.9.1, CLI: 2.5.1, ORM: 3.27.0]

Loaded configuration file "db/config/config.json".
Using environment "development".
== 20161219052831-init: migrating =======
== 20161219052831-init: migrated (0.092s)
```

这就是执行成功了，
此时数据库里应该就有这个user表了。

*打开数据库确认中*

ok，
这样我们之后对数据库操作都通过这个脚本来进行，
下回我们写增删改查。
