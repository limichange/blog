步骤：
 - 建模型
 - 连接池
 - 增加数据
 - 查询数据
 - 更新数据
 - 删除数据

先看看大致的目录结构，
完成了之后大致就是这个样子的。
```
 ➜  db git:(master) tree .
.
├── config
│   └── config.json
├── example
│   ├── create.js
│   ├── delete.js
│   ├── find.js
│   └── update.js
├── index.js
├── migrations
│   └── 20161219052831-init.js
├── models
│   ├── index.js
│   └── user.js
└── seeders
```

为了操作数据库肯定就是要先做一个数据库的连接,
指定MySQL的地址、密码、账号、数据库等等。
```js
const Sequelize = require('sequelize')

const sequelize = new Sequelize('database_development', 'root', null, {
  host: 'localhost',
  dialect: 'mysql',
  pool: {
    max: 5,
    min: 0,
    idle: 10000
  }
});

module.exports = sequelize
```
这个应该问题不大。

然后我们建立一个模型，
这个模型是对数据库的一个映射。
```js
const sequelize = require('../index')
const Sequelize = require('sequelize')

const User = sequelize.define('user', {
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
}, {
  freezeTableName: true
});

module.exports = User;
```

定义的方式和之前的migration几乎一样。

接下来就是example里的增删改查，
例子本身不是很难，
不懂的地方可以提一下。
```js
// find.js
const User = require('../models/user')

function logUserToJSON (user) {
  if (user) {
    console.log(user.toJSON())
  }
  return user
}

User
  .findAll()
  .then((result) => {
    result.forEach(logUserToJSON)
  })

User
  .findOne({
    where: {
      id: 3
    }
  })
  .then(logUserToJSON)

User
  .findById(4)
  .then(logUserToJSON)
```

```js
// crate.js
const User = require('../models/user')

User.create({
  nickname: 'nickname5',
  password: 'password5',
  username: 'username5'
}).then((user) => {
  console.log(user.toJSON())
})
```

```js
// delete.js
const User = require('../models/user')

function logUserToJSON (user) {
  if (user) {
    console.log(user.toJSON())
  }
  return user
}

User
  .findOne({
    where: {
      id: 3
    }
  })
  .then(logUserToJSON)
  .then((user) => {
    if (user) {
      user.destroy()
    }
    return user
  })
```

```js
// update.js
const User = require('../models/user')

function logUserToJSON (user) {
  if (user) {
    console.log(user.toJSON())
  }
  return user
}

User
  .findOne({
    where: {
      id: 3
    }
  })
  .then(logUserToJSON)
  .then((user) => {
    return user.update({
      nickname: 'nickname333'
    })
  })
  .then(logUserToJSON)
```

这部分最好还是自己一行行写，
虽然代码本身比较容易，
但自己写的话能发现很多的问题。

> 项目地址
> https://github.com/limichange/sequelize-demo
