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
写好后运行一下。
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
        alert('Failed because: ' + message);
    }
},
```

功能挺丰富的，
不但能控制图片的质量还可以选择调用摄像头的方向，
拍照成功后会返回图片的临时地址。

## cordova-plugin-file-transfer

http://cordova.apache.org/docs/en/latest/reference/cordova-plugin-file-transfer/index.html

*看文档中*

文件传输，
用于文件的上传下载，
我们专门来看看上传是什么样的。

```shell
$ cordova plugin add cordova-plugin-file-transfer
```

安装成功后，
检查一下这个模块的对象有没有加载成功。
```js
alert(FileTransfer);
```

看起来没有什么问题。
