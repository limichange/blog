## 开始

本机Mac。

先看看官网。
https://cordova.apache.org/#getstarted

ok，
有新手教程。
```shell
# 安装
$ npm install -g cordova

# 创建项目
$ cordova create MyApp

# 进入项目
$ cd MyApp

# 添加平台
$ cordova platform add browser

# 运行
$ cordova run browser
```

看看生成的目录是什么样的。
```shell
➜  MyApp tree . -L 2
.
├── config.xml
├── hooks
│   └── README.md
├── platforms
│   ├── browser
│   └── platforms.json
├── plugins
│   ├── browser.json
│   ├── cordova-plugin-whitelist
│   └── fetch.json
└── www
    ├── css
    ├── img
    ├── index.html
    └── js

10 directories, 6 files
```

很顺利，
没有遇到什么问题。

## 创建第一个APP

原来文档里面还有一个更加详细的新手指导（摔。
https://cordova.apache.org/docs/en/latest/guide/cli/index.html

### 安装
跟上面说的一样。

### 创建
```shell
$ cordova create hello com.example.hello HelloWorld
```

不是很明白为啥是三个名字，
看看说明文档是怎么讲的。
```shell
$ cordova help create
```

*看文档中*

三个参数分别是PATH、ID、NAME，
PATH：创建的路径，
ID：包名，
NAME：应用的名字，
ok。

下面是生成的目录文件，
还是很好懂的，
里面具体是什么东西我们回头再看。
```shell
➜  hello tree . -L 2
.
├── config.xml
├── hooks
│   └── README.md
├── platforms
├── plugins
└── www
    ├── css
    ├── img
    ├── index.html
    └── js
```

### 添加平台

先看看命令。
```shell
$ cd hello
$ cordova platform add ios --save
$ cordova platform add android --save
$ cordova platform ls
```

怎么又和上面不一样？
`--save`？
再看看文档。
```shell
$ cordova help platform
```

*看文档中*

> Save specified platforms into config.xml after installing them

噢，
和NPM的用法一样，
就是装完后存到`config.xml`这个文件里。

接下来开始执行命令看看。

这两个命令没问题，
Xcode我之前就装过了。
```shell
# 进入项目目录
$ cd hello

# 添加IOS平台
$ cordova platform add ios --save
```

到这个就报错了。
```shell
# 添加Android平台
$ cordova platform add android --save
```

下面是错误详情。
```shell
Adding android project...
Creating Cordova project for the Android platform:
	Path: platforms/android
	Package: com.example.hello
	Name: HelloWorld
	Activity: MainActivity
	Android target: android-24
Subproject Path: CordovaLib
Android project created with cordova-android@6.0.0
Installing "cordova-plugin-whitelist" for android
Failed to install 'cordova-plugin-whitelist':CordovaError: Failed to find 'ANDROID_HOME' environment variable. Try setting setting it manually.
Failed to find 'android' command in your 'PATH'. Try update your 'PATH' to include path to valid SDK directory.
    at /Users/limi/testCode/cordova/hello/platforms/android/cordova/lib/check_reqs.js:222:19
    at _fulfilled (/Users/limi/testCode/cordova/hello/platforms/android/cordova/node_modules/q/q.js:834:54)
    at self.promiseDispatch.done (/Users/limi/testCode/cordova/hello/platforms/android/cordova/node_modules/q/q.js:863:30)
    at Promise.promise.promiseDispatch (/Users/limi/testCode/cordova/hello/platforms/android/cordova/node_modules/q/q.js:796:13)
    at /Users/limi/testCode/cordova/hello/platforms/android/cordova/node_modules/q/q.js:857:14
    at runSingle (/Users/limi/testCode/cordova/hello/platforms/android/cordova/node_modules/q/q.js:137:13)
    at flush (/Users/limi/testCode/cordova/hello/platforms/android/cordova/node_modules/q/q.js:125:13)
    at _combinedTickCallback (internal/process/next_tick.js:67:7)
    at process._tickCallback (internal/process/next_tick.js:98:9)
Error: Failed to find 'ANDROID_HOME' environment variable. Try setting setting it manually.
Failed to find 'android' command in your 'PATH'. Try update your 'PATH' to include path to valid SDK directory.
```
说白了就是没装安卓的环境，
跑去下载Android Studio。
https://developer.android.com/studio/index.html

*下载中*

*安装中*

打开，
提示`Unable to access Android SDK add-on list`？

额，
是长城挡住了我的去路。

从这篇文章里挑一个镜像配置一下就好了。
http://tools.android-studio.org/index.php/85-tools/110-androidsdk-mirrors

一路next。
重新看看是不是好了，
怎么检查？
此时我瞄到了下面的内容。

### 环境检查
……你有检查命令啊？
早说啊！

```shell
# 检查环境是否安装正确
$ cordova requirements
```

打印出来的报错好多啊。

往下瞄了一眼文档，
还有配置环境指南……赶紧打开。

#### IOS

`$ xcode-select --install`
运行后提示我我已经装好了。

```shell
$ npm install -g ios-deploy
```

提示。
```text
stderr: xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance
```

查了一下。
https://github.com/nodejs/node-gyp/issues/569

运行完这个命令就可以装ios-deploy了。
`sudo xcode-select -s /Applications/Xcode.app/Contents/Developer`

再运行命令检查一下，
提示要执行一下这个。
```shell
pod setup
```

*等了好久*

ok了。

这个操作可能会报错，
多试几次，
不行的话就再试几次。


#### Android

装 JAVA SDK。
http://download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-macosx-x64.dmg

装 Android SDK。
不知怎么回事，
Android的那个SDK工具没法用，
专门去下载一下。
https://dl.google.com/android/repository/tools_r25.2.3-macosx.zip

解压后放到`/Users/${name}/Library/Android/sdk`。
```shell
# 打开管理器开始安装Android SDK
./android
```

*安装中*

检查配置，
`Please install Android target: "android-24"`，
好吧，
我再装一次。

*安装中*

```shell
$ cordova requirements

Requirements check results for android:
Java JDK: installed 1.8.0
Android SDK: installed true
Android target: installed android-14,android-15,android-24,Google Inc.:Google APIs:15,Google Inc.:Google APIs:24
Gradle: installed

Requirements check results for ios:
Apple OS X: installed darwin
Xcode: installed 8.2
ios-deploy: installed 1.9.0
CocoaPods: installed
```

ok，
没问题了。

### 构建

一定要确保你上面的步骤都搞定了再往下看。

```shell
# 构建所有的平台
$ cordova build

# 构建单个平台
$ cordova build ios
```

详细的命令文档。
https://cordova.apache.org/docs/en/latest/reference/cordova-cli/index.html#cordova-build-command

### 测试

```shell
# 启动一个安卓模拟器
$ cordova emulate android
```

这里可能会报一个解压的错，
http://stackoverflow.com/questions/32535930/cordova-build-gradle-error-while-opening-extracting-zip-file
你去他报错的目录里把文件删掉再运行一次这个命令就可以了。

*又下载了好久*

然而又报错了，
说没有接受SDK的协议？

http://stackoverflow.com/questions/38096225/automatically-accept-all-sdk-licences
```shell
cd /Users/limi/Library/Android/sdk
echo -e "\n84831b9409646a918e30573bab4c9c91346d8abd" > "./android-sdk-preview-license"
echo -e "\n8933bad161af4178b1185d1a37fbf41ea5269c55" > "./android-sdk-license"
```

再次运行。

`Error: No emulator images (avds) found.`

我不搞了🙄，
……直接连接真机。

```shell
# 用这个命令直接运行
$ cordova run android
```

好了，
运行成功。

再看看IOS的。
```shell
# 用这个命令直接运行
$ cordova run ios
```

ok，
报错，
提示需要登录Xcode上注册一下身份，
打开IOS项目，
登录后再次执行命令。

再次报错，
提示需要去开发者中心注册这个应用，
不是开发者，
卒。

### 添加插件

如果需要什么插件，
去NPM上搜索，
关键字带上cordova就行了。

```shell
# 安装一个照相机插件
$ cordova plugin add cordova-plugin-camera
```

*下载安装中*

好了，
至于怎么用我们回头再看，
此外官方还推荐用`Plugman`来管理插件，
这个我们也回头再看，
先往下走。

### 平台定制

如果你希望IOS上按钮是红的，
在安卓上按钮是绿的，
就需要他提供的方法了。

就是创建一个`merges`文件夹，
里面的内容会根据平台加载。

```html
<!-- www/index.html -->
<link rel="stylesheet" type="text/css" href="css/overrides.css" />
```

```css
/* www/css/overrides.css */
```

```css
/* merges/android/overrides.css */
body { font-size:14px; }
```

这样就只会安卓的字体大小被调整了，
当然图片什么的也可以这么做。

~但是我觉得没啥用，很容易就乱了~

### 更新Cordova
```shell
# 更新工具
$ sudo npm update -g cordova

# 更新平台
$ cordova platform update android --save
$ cordova platform update ios --save
```

## 总结
只跑通了Android，
配置一次累死人，
无尽的报错，
无尽的下载，
这一大堆工具都是啥啊？
Android Studio好丑。
