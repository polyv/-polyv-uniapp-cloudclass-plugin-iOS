# README

本项目是<u>针对保利威云课堂的uni-app客户提供的打包工程</u>（不支持模拟器运行调试），用于打包 uni-app 项目（包含云课堂插件）的自定义基座，也可用于打正式包。使用该项目进行离线打包，可使你免受 uni-app 云打包的限制。支持在该工程的基础上添加其他uni-app插件。

⚠️注意：Polyv云课堂插件在 iOS 端仅支持云端打自定义基座包，正式发布时，需使用该打包工程进行离线打包。



## 开发环境

1. iOS 开发环境：Xcode 11.0+
2. uni-app 开发环境：HBuilderX 2.8.0+



## 前提条件

1. [保利威官网](http://www.polyv.net/)账号
2. 苹果开发者账号



## 下载离线 SDK 包

该项目想正常运行，需先到 uni-app 的官网下载离线 SDK 包 —— [下载链接](https://nativesupport.dcloud.net.cn/AppDocs/download/ios)。

⚠️注意，SDK 包的与 HBuilderX 的版本一一对应，请根据你 HBuilderX 的版本下载对应的 SDK 包。本项目基于 v2.8.6 版本的 SDK 进行开发，建议不要使用跟该版本差异太大的版本。 

将下载后的离线 SDK 包中的 SDK 文件夹放到当前项目的<u>根目录</u>下。



## 引入 App 资源

每个 uni-app 项目都有自己的应用标识，可在配置文件 mainfest.json 中的【基础配置】里看到。关于 AppID 的更多信息，详见 [DCloud appid 用途/作用/使用说明](https://ask.dcloud.net.cn/article/35907)。

1. 生成本地打包 App 资源

   打开 HBuilderX 项目，选择【发行】-【原生App-本地打包】-【生成本地打包App资源】。

2. 拷贝 App 资源到插件工程

   将该文件夹拷贝到工程的 HBuilder-Vod/Pandora/apps 路径下，确保该路径下有且只有一个文件夹，且名称为你的 AppID。

   确保 HBuilder-Vod/control.xml 里面 app 节点的 appid 属性为你的 AppID。



## 离线打包

由于 uni-app 的云端打包限制了每日免费打包的次数以及免费打包的包体的大小（超过40M每次打包需充值付费），使用离线打包的方式会更加自由、方便。

首先，菜单栏选择【Product】-【Scheme】-【Edit Scheme】，在弹出的窗口左侧选择【Run】，右侧选择【info】栏，把【Build Configuration】改为 Release。

接着，在 Xcode 顶部的选择运行设备一栏，选择设备为【Any iOS Device】，位于一系列模拟器上面那一行。

然后，在【TARGETS】-【HBuilder-Vod】-【Signing & Capabilities】-【Signing（Release）】配置打包需要的苹果发布证书及授权文件。

### 自定义基座

1. 修改 TARGETS  - HBuilder-Vod - General - Bundle Identifier 为你的 uni.${AppID去掉下划线}；

2. 打开 HBuilder-Vod-Info.plist，确保 【Application supports iTunes file sharing】这一行的值为 YES；

3. 打开 HBuilder-Vod/control.xml，确保 HBuilder 节点包含 debug、syncDebug 属性值 true：

   ```xml
   <HBuilder debug="true" syncDebug="true" version="1.9.9.56853">
   ```

选择菜单栏【Product】-【Archive】进行打包。

将打出来的 ipa 文件，重命名为 iOS_debug.ipa，放到 HBuilderX 项目下的 unpackage/debug 路径下。

选择 HBuilderX 菜单栏【运行】-【运行到手机或模拟器】-【运行基座选择】-【自定义基座】。

把 iPhone 设备用数据线连接到电脑上，选择【信任】当前连接设备，HBuilderX 检测到真机之后，选择运行到手机即可。如果检测不到真机，参考[HBuilder/HBuilderX真机运行、手机运行、真机联调常见问题](https://ask.dcloud.net.cn/article/97)



### 正式包

1. 确保 Bundle Identifier 与你的发布授权文件相匹配。
2. 打开 HBuilder-CloudClass-Info.plist，把【Application supports iTunes file sharing】这一行的值改为 NO；
3. 打开 HBuilder-CloudClass/control.xml，移除 HBuilder 节点的属性 debug、syncDebug。

选择菜单栏【Product】-【Archive】进行打包。



## 项目配置

关于如何对 app 进行个性化配置，譬如 app 图标、app 名称、app 版本号、app 启动图等，uni-app 官方有详细的文档指导，见：[iOS 原生工程配置](https://nativesupport.dcloud.net.cn/AppDocs/usesdk/ios?id=开发环境)。

该项目还支持添加其他 uni-app 原生插件，添加方法详见 uni-app 官方的指引文档[iOS 离线打包使用插件](https://nativesupport.dcloud.net.cn/NativePlugin/offline_package/ios?id=预备环境)。

