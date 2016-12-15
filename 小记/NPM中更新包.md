随着时间的推移，
很多库都会或多或少的升级，
所以大家平时就需要看看需不需要更新，
NPM做的还是比较简单的。

先运行`npm outdated`检查一下版本号。
```bash
➜  dir git:(master) ✗ npm outdated
Package                Current  Wanted  Latest  Location
autoprefixer             6.5.3   6.5.4   6.5.4  dir
css-loader              0.25.0  0.25.0  0.26.1  dir
eslint                  3.12.0  3.12.2  3.12.2  dir
eslint-plugin-promise    2.0.1   2.0.1   3.4.0  dir
node-sass               3.13.1  3.13.1   4.0.0  dir
sass-loader              4.0.2   4.1.0   4.1.0  dir
webpack-merge           0.14.1  0.14.1   1.1.1  dir
```

必要的话瞅瞅升级的细节，
如果不怎么在意的话就直接的升级。

指定一个模块升级，
不要一下子全都升级，
很容易出现问题的。
```bash
sudo npm update eslint
```

升级完再看看。
```bash
➜  dir git:(master) ✗ npm outdated
Package                Current  Wanted  Latest  Location
autoprefixer             6.5.3   6.5.4   6.5.4  dir
css-loader              0.25.0  0.25.0  0.26.1  dir
eslint-plugin-promise    2.0.1   2.0.1   3.4.0  dir
node-sass               3.13.1  3.13.1   4.0.0  dir
sass-loader              4.0.2   4.1.0   4.1.0  dir
webpack-merge           0.14.1  0.14.1   1.1.1  dir
```

完美。

本来我是用了`npm-check-updates`来做检查的，
今天才知道原来NPM自带了，
（摊手）。
