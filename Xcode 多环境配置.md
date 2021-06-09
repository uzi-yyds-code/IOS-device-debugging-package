# 更新最新热门文章

*   [逆向入门到进阶项目合集](https://github.com/uzi-yyds-code/IOS-reverse-security)



* **欢迎可以加入iOS高级技术交流群：[1001906160](https://jq.qq.com/?_wv=1027&k=KjioxJty)；群文件可以免费真机包下载，更多中高级进阶资料！**

在进行多环境配置之前，我们需要对Xcode内的元素组成做一些了解。

*   Project:包含了项目所有的代码，资源文件等所有信息。
*   Target:对指定代码和资源文件的具体构建方式。
*   Scheme:对指定Target的环境配置。

## 多环境配置三种方式

### 方式1\. 多Target

新建一个Target，修改BundleId，修改跟随Target新生成的Info-plist文件名

![BunduleId](https://upload-images.jianshu.io/upload_images/19704571-d5f47352d9d8563f.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![info.plist](https://upload-images.jianshu.io/upload_images/19704571-2d6869456e454fb5.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对OC来说，根据target在Builed setting 的Preprocessor Macros中定义宏，来区分不同target的不同环境

![环境](https://upload-images.jianshu.io/upload_images/19704571-cbf808530a1adf81.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

切换不同的target的编译scheme后,在代码中以下面方式使用

![代码](https://upload-images.jianshu.io/upload_images/19704571-2ce284441c4fe8fb.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对swift来说，可以在Builed setting 的 Other Swift Flags 里配置一个变量，然后在Swift代码中进行判断

![swift](https://upload-images.jianshu.io/upload_images/19704571-65a71d107d1de1b8.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是该方式并不是很方便，缺点如下：

*   会生成多个Info-plist，不便于管理
*   配置起来比较混乱

### 方式2\. 多Scheme - 多Configurations

一般Scheme默认有Debug和Relesse 两种Configurations,此处可以再添加第三种

![Scheme](https://upload-images.jianshu.io/upload_images/19704571-5b0e2088ed9b51e7.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时所有的配置选项都会变成三个，此时可以新建多个Scheme，分别在不同Scheme选择对应不同的Configurations

![Configurations](https://upload-images.jianshu.io/upload_images/19704571-894ffaab52782ae8.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们在User-Defined定义一个HOST_URL,并在info-plist中暴露出来

![User-Defined](https://upload-images.jianshu.io/upload_images/19704571-ba90be1b7c6cc982.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

切换Scheme就可以切换不同环境的HOST_URL，达到多环境配置的目的

![代码](https://upload-images.jianshu.io/upload_images/19704571-c0d1de5d41ed5607.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此外，还可以在User-Defined中 为不同Scheme配置不同的appico

![icon](https://upload-images.jianshu.io/upload_images/19704571-ad07c339682f38f2.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![icon配置](https://upload-images.jianshu.io/upload_images/19704571-46216df39b907de1.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时不同Target就有了不同的icon
这种方式相较第一种方便了一些，但是依然需要在Builed setting 中进行配置

### 方式3\. xconfig文件 （cocoapods就是使用的这种方式）

首先创建xconfig文件，需要注意的是xconfig文件与环境一一对应
命名时以 路径-项目名-环境名 来命名

![xconfig](https://upload-images.jianshu.io/upload_images/19704571-fec39a88273b97b5.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

并在Configurations中选择对应的xconfig文件

![Configurations](https://upload-images.jianshu.io/upload_images/19704571-51502bff7f8a5212.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### xconfig冲突处理

此时删除之前配置的HOST_URL，在不同Xconfig文件中写入不同HOST_URL的值，也能得到相同的效果，但是cocoa pods中也有自己的Xconfig文件，在这之前需要把cocoa pods的xconfig文件路径（注意加上根目录Pods/）引入到Xconfig文件中来解决冲突。

![xconfig1](https://upload-images.jianshu.io/upload_images/19704571-5eaf606613703d12.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![xconfig2](https://upload-images.jianshu.io/upload_images/19704571-844f7e366746862d.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时就准确打印出了Debug模式下HOST_URL

![代码](https://upload-images.jianshu.io/upload_images/19704571-9a3dd00a65c04441.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是在两个Xconfig文件中可能配置了两个相同的Builed setting配置，这也是一种冲突,这时需要在自己的XConfig文件中用$(inherited)来继承cocoapods中的配置就可以了。

![相同配置](https://upload-images.jianshu.io/upload_images/19704571-ffe139f0d967e71a.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![相同配置设置](https://upload-images.jianshu.io/upload_images/19704571-8edfc3f0d5e36960.image?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 对以上三种方式进行总结：

*   多target ,可以控制需要编译的代码文件和资源文件
*   多Scheme可以提供多套环境变量
*   多xconfig 可以更好的管理和配置这些环境变量

# 更新最新热门文章

*   [逆向入门到进阶项目合集](https://github.com/uzi-yyds-code/IOS-reverse-security)



* **欢迎可以加入iOS高级技术交流群：[1001906160](https://jq.qq.com/?_wv=1027&k=KjioxJty)；群文件可以免费真机包下载，更多中高级进阶资料！**

作者：ives_w
链接：https://juejin.cn/post/6971303989342109726

