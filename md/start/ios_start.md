#### 自动安装

[CocoaPods](http://www.cocoapods.org) 是一个很好的依赖管理工具，我们推荐您使用这个方法来安装 SDK，最大化的简化安装过程。

LeanCloud SDK for iOS 同时支持动态库和静态库，使用 CocoaPods 进行集成时要进行区分。

如果使用 **静态库** 方式进行集成，则在 Podfile 中添加：

```ruby
pod 'AVOSCloud'

# 如果使用实时通信功能，可以添加：
pod 'AVOSCloudIM'

# 如果使用崩溃收集功能，可以添加：
pod 'AVOSCloudCrashReporting'
```

如果使用 **动态库** 方式进行集成，则在 Podfile 中添加：

```ruby
use_frameworks!

pod 'AVOSCloudDynamic'

# 如果使用实时通信功能，可以添加：
pod 'AVOSCloudIMDynamic'

# 如果使用崩溃收集功能，可以添加：
pod 'AVOSCloudCrashReportingDynamic'
```

然后在项目根目录执行 `pod install` 命令，就能将 LeanCloud SDK for iOS 集成到你的项目中。


#### 手动安装

你也可以手动将 LeanCloud SDK for iOS 集成到项目中。

iOS 从 8.0 开始支持动态库，如果你的项目只支持 iOS 8 及以上，使用动态库是个不错的选择。


##### 目录结构

首先，从下面的地址下载最新版本的 iOS SDK：

<p><a class="btn btn-default" href="sdk_down.html">下载 iOS SDK</a></p>

下载完成后，解压缩下载的文件，可以看到每个模块有如下的目录结构：

![img](images/quick_start/ios/dir_tree.png)

就像目录名描述的那样，Dynamic 目录下存放着动态库，Static 目录下存放着静态库。


##### 安装动态库

首先，准备好待集成的模块。将它们放入同一个目录中：

![img](images/quick_start/ios/all_frameworks.png)

请注意，所有 frameworks 都是 Dynamic 目录下面的，确保它们都是动态库。

然后，将这个目录拖入你的项目中：

![img](images/quick_start/ios/1.png)

确保 **Copy items if needed** 选择框处于选中状态：

![img](images/quick_start/ios/2.png)

做完上面这些步骤后，项目看起来是这样：

![img](images/quick_start/ios/3.png)

然后切换到 Targets 的 General 选项卡，点击 **Embedded Binaries** 左下角的加号按钮，添加 frameworks：

![img](images/quick_start/ios/embedded_binaries.png)

这样就集成完毕了。


##### 安装静态库

<div class="callout callout-info">确保你正在使用最新版本的 Xcode（4.6+），并且面向 iOS 4.3 或者更高版本。我们推荐 Xcode 5 和 iOS 5 或以上系统。</div>

首先，跟安装动态库一样，准备好待集成的模块。将它们放入同一个目录中：

![img](images/quick_start/ios/all_frameworks.png)

请注意，所有 frameworks 都是 Static 目录下面的，确保它们都是动态库。

然后，将这个目录拖入你的项目中：

![img](images/quick_start/ios/1.png)

确保 **Copy items if needed** 选择框处于选中状态：

![img](images/quick_start/ios/2.png)

做完上面这些步骤后，项目看起来是这样：

![img](images/quick_start/ios/3.png)

切换到 Targets 的 **Build Phases** 选项卡，展开 **Link Binary With Libraries** 可以看到：

![img](images/quick_start/ios/4.png)

点击 **Link Binary With Libraries** 部分左下角的加号按钮：

![img](images/quick_start/ios/6.png)

添加下列库：

- SystemConfiguration.framework
- MobileCoreServices.framework
- CoreTelephony.framework
- CoreLocation.framework
- libicucore.dylib

如果使用了 AVOSCloudCrashReporting 模块，还需额外添加 **libc++.dylib**。

如果使用了 AVOSCloudIM 模块，还需额外添加 **libsqlite3.dylib**。

在 Targets 的 **Build Settings** 中，为 **Other Linker Flags** 增加 **-all_load** 和 **-ObjC** 链接选项。

![img](images/quick_start/ios/all_load.png)

#### 初始化 SDK

打开 AppDelegate.m 文件，添加下列导入语句到头部：

```
#import <AVOSCloud/AVOSCloud.h>;
```

然后粘贴下列代码到 `application:didFinishLaunchingWithOptions` 函数内：

```
//如果使用美国站点，请加上这行代码 [AVOSCloud useAVCloudUS];
[AVOSCloud setApplicationId:@"{{appid}}"
              clientKey:@"{{appkey}}"];
```

如果想跟踪统计应用的打开情况，后面还可以添加下列代码：

```
[AVAnalytics trackAppOpenedWithLaunchOptions:launchOptions];
```

创建应用后，可以在 [控制台 > 应用设置](/app.html?appid={{appid}}#/key) 里面找到应用对应的 id 和 key。

修改编译选项 **Architectures** 值为 **Standard architectures(armv7,arm64)**：

![img](images/quick_start/ios/arm64.png)

保证在你的 **.h** 头文件里包含了 SDK 库文件：

```
#import <AVOSCloud/AVOSCloud.h>;
```

将下面的代码拷贝到你的 app 里，比如在 `viewDidLoad` 方法（或者其他在运行 app 时会调用到的方法）：

```
AVObject *testObject = [AVObject objectWithClassName:@"TestObject"];
[testObject setObject:@"bar" forKey:@"foo"];
[testObject save];
```

运行 app，一个类名为 `TestObject` 的新对象会被发送到 LeanCloud 并保存下来。当做完这一切，访问 [控制台 > 数据管理](/data.html?appid={{appid}}#/TestObject) 可以看到上面创建的 TestObject 的相关数据。


#### 社交组件

最后，如果希望使用社交组件功能，可以使用我们的开源组件：[leancloud-social-ios](https://github.com/leancloud/leancloud-social-ios)。
