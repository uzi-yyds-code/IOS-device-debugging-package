# 更新最新热门文章

*   [逆向入门到进阶项目合集](https://github.com/uzi-yyds-code/IOS-reverse-security)



* **欢迎可以加入iOS高级技术交流群：[1001906160](https://jq.qq.com/?_wv=1027&k=KjioxJty)；群文件可以免费真机包下载，更多中高级进阶资料！**


在iOS13适配过程中会有使用低版本的SDK进行编译然后跑在高版本的设备上进行兼容性适配。 如果每次都打包出来跑在高版本的设备上实在有些麻烦又不方便Debug。其实，低版本的Xcode是可以调试高版本的设备的，只是需要进行一点改动。

## 低版本Xcode调试高版本真机

Xcode的真机部署和调试依赖一个叫做 **Device Support File**的东西， 每个版本的固件都有对应的该文件，只有电脑的**device Support File**和目标设备的系统匹配才可以调试。

对应新版本固件的**Device Support File**都是随新版本的Xcode附带， 首先下载好新版本的Xcode(目前最新版的是Xcode 11 beta)，然后到(假设新版本的Xcode是Xcode-beta)

```
/Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport
复制代码
```

复制对应版本的**Device Support File**,

![image.png](https://upload-images.jianshu.io/upload_images/19704571-bccd2e8bb3e5bb15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 注意：要整个目录一起复制， 比如上图就是复制整个**13.0**目录

到旧版本的目录

```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport
复制代码
```

输入密码， 重启下旧版Xcode就可以调试高版本真机了。

## 低版本Xcode调试高版本模拟器

要调试高版本的模拟器， 只需要先下载一个高版本的Xcode，然后随便打开一个项目运行一下， 等模拟器启动起来。

然后再旧版本的Xcode上就可以选择高版本的模拟器进行调试了。

# 更新最新热门文章

*   [逆向入门到进阶项目合集](https://github.com/uzi-yyds-code/IOS-reverse-security)
* **欢迎可以加入iOS高级技术交流群：[1001906160](https://jq.qq.com/?_wv=1027&k=KjioxJty)；群文件可以免费真机包下载，更多中高级进阶资料！**


作者：lxyzk
链接：https://juejin.cn/post/6844903910658801672

