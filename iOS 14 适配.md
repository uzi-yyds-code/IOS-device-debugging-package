# 更新最新热门文章

*   [逆向入门到进阶项目合集](https://github.com/uzi-yyds-code/IOS-reverse-security)



* **欢迎可以加入iOS高级技术交流群：[1001906160](https://jq.qq.com/?_wv=1027&k=KjioxJty)；群文件可以免费真机包下载，更多中高级进阶资料！**

苹果每年发布新手机的时间前后，都会发布最新的 iOS 系统，一般会包含比较多的更新，例如隐私策略、API 更新和废弃、审核策略等等。所以针对每次更新，都需要进行适配，以便可以让用户无感知的过渡到新系统，不会出现崩溃等一些体验性问题。

####KVC 方法禁用
现状：
如同 iOS 13 时，UITextField 禁止使用 setValue:forKey: 来设置 _placeholderLabel 私有属性，iOS 14 中，UIPageControl 也禁止使用 setValue 方法，来设置 _pageImage 和 _currentPageImage 属性。否则运行时将会崩溃。

解决方法：
全局搜索 _pageImage 和 _currentPageImage 关键字，发现项目中没有用到。

####选择照片
现状：
访问相册选择照片时，弹出的权限弹窗中多出了一个选项：选择照片。可以让用户选择部分照片，以供 App 使用，未选择的其他照片，App 是看不到的。葩趣 App 中引入了一个三方 TZImagePickerController ，会受到影响白屏，通过升级到最新的版本，解决。

更优方案：

用户选择了这个选项以后，每次重启手机再次请求时，这个权限弹窗都会再次弹出 可以通过在 info.plist 中配置 PHPhotoLibraryPreventAutomaticLimitedAccessAlert 项值为 YES，来阻止每次弹窗的弹出。
同时可以有个策略告诉用户，你选择的模式是部分图片，可以在设置中进行更改。产品暂时没有时间进行排期。

####获取唯一标识符 API 废弃
现状：
在 iOS13 及以前，系统会默认为用户开启允许追踪设置，我们可以简单的通过代码来获取到用户的 IDFA 标识符。
```
if ([[ASIdentifierManager sharedManager] isAdvertisingTrackingEnabled]) {
    NSString *idfaString = [[ASIdentifierManager sharedManager] advertisingIdentifier].UUIDString;
    NSLog(@"%@", idfaString);
}
复制代码
```
iOS 14 以后，会默认关闭广告追踪权限，而且上面判断是否开启权限的方法已经废弃。

解决方案：需要我们需要在 Info.plist 中配置" NSUserTrackingUsageDescription " 及描述文案，接着使用 AppTrackingTransparency 框架中的 ATTrackingManager 中的 requestTrackingAuthorizationWithCompletionHandler 请求用户权限，在用户授权后再去访问 IDFA 才能够获取到正确信息。
```
#import <AppTrackingTransparency/AppTrackingTransparency.h>
#import <AdSupport/AdSupport.h>

- (void)testIDFA {
    if (@available(iOS 14, *)) {
        [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
            if (status == ATTrackingManagerAuthorizationStatusAuthorized) {
                NSString *idfaString = [[ASIdentifierManager sharedManager] advertisingIdentifier].UUIDString;
            }
        }];
    } else {
        // 使用原方式访问 IDFA
    }
}
复制代码
```
葩趣中没有用到广告标识符，但是需要确认 Bugly 等一些第三方有没有 SDK 更新。经过查看官网，没有需要更新的 SDK。

####获取相册 API 更新

现状：

AssetLibrary 方式获取照片被禁用，使用该方式，将不会获取到照片。推荐使用 PHPhotoKit 来获取相册照片。

解决方案：葩趣 App 中使用了 PhotoKit 来获取相册，无需适配。

####UITableViewCell
cell 内部必须使用 self.contentView addsubview: 方式，添加子控件，否则会出现无法响应点击的情况。



# 更新最新热门文章

*   [逆向入门到进阶项目合集](https://github.com/uzi-yyds-code/IOS-reverse-security)



* **欢迎可以加入iOS高级技术交流群：[1001906160](https://jq.qq.com/?_wv=1027&k=KjioxJty)；群文件可以免费真机包下载，更多中高级进阶资料！**

作者：落寒
链接：https://juejin.cn/post/6912339107268329485
