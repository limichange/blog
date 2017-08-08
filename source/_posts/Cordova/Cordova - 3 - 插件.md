## 开始
Cordova的插件看起来就是原生组件暴露了JS接口，
安装后调用一下就可以了。

每一个插件都大同小异，
我们挑几个比较有用的看看。

## cordova-plugin-camera

我们之前已经安装好了摄像头的插件，
接下来让我们调用一下试试。

http://cordova.apache.org/docs/en/latest/reference/cordova-plugin-camera/index.html

基础代码很简单，
写好后运行一下，
拍完照就会显示出来了。
```js
// www/js/index.js
onDeviceReady: function() {
    this.receivedEvent('deviceready');

    navigator.camera.getPicture(onSuccess, onFail, { quality: 50,
        destinationType: Camera.DestinationType.FILE_URI });

    function onSuccess(imageURI) {
        var image = document.getElementById('myImage');
        image.src = imageURI;
    }

    function onFail(message) {
        navigator.notification.alert(message);
        alert('Failed because: ' + message);
    }
},
```

```html
<!-- www/index.html -->
<img id="myImage" width="200px" src="" alt="">
```

功能挺丰富的，
不但能控制图片的质量还可以选择调用摄像头的方向，
拍照成功后会返回图片的临时地址。

## cordova-plugin-dialogs

http://cordova.apache.org/docs/en/latest/reference/cordova-plugin-dialogs/index.html

直接用alert的话会出现弹窗显示标题的问题，
所以需要一个插件调用系统的弹窗。

```shell
# 安装
$ cordova plugin add cordova-plugin-dialogs
```

这几个方法都是可用的。
 - navigator.notification.alert
 - navigator.notification.confirm
 - navigator.notification.prompt
 - navigator.notification.beep

在代码直接调用即可。

```js
// 简单用法
navigator.notification.alert('这是一个弹窗');

// 所用选项
navigator.notification.alert(
    '内容',
    function () { alert('回调') },
    '标题',
    '按钮'
);
```

还能出声音🎶

```js
// 数字是次数
navigator.notification.beep(1);
```

## cordova-plugin-image-picker

如果我们需要像微信那样一下子获取9张图的话就需要这个插件。

https://github.com/apache/cordova-plugin-media-capture

```shell
# 安装
cordova plugin add cordova-plugin-image-picker
```

返回的结果就是临时地址，
可以限制图片的数量，
这个组件不是官方的，
所以使用的方式和上面的两个不太一样。

```js
window.imagePicker.getPictures(
    function(results) {
        for (var i = 0; i < results.length; i++) {
            alert('Image URI: ' + results[i]);
        }
    }, function (error) {
        alert('Error: ' + error);
    }
);
```

## 看法
看样子，
插件不是那么的丰富，
很多的插件只支持IOS或只支持Android，
毕竟维护的难度很大。
