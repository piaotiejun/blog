---
title: react-native
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- js
---

# react native前置基础
## html
[html-w3school](http://www.w3school.com.cn/html/index.asp)
~~~
react native使用自己的内嵌组件(标签)，比如<Text>等，不支持html的任何标签
~~~
## js
~~~
react native 从0.4(应该是)开始，全面使用es6语法
~~~
[廖雪峰js](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)
[JS_关键字详细](http://www.cnblogs.com/somesayss/archive/2013/01/12/2857996.html)
[彻底搞清楚javascript中的require、import和export](https://www.cnblogs.com/libin-1/p/7127481.html)
[http://es6-features.org/  es6]
[ES6摘要](https://www.jianshu.com/p/287e0bb867ae )
[js对象模型](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Details_of_the_Object_Model)
## jsx
[jsx详细讲解](https://reactjs.org/docs/jsx-in-depth.html)
[React JSX](http://www.runoob.com/react/react-jsx.html)
[JSX](https://www.cnblogs.com/zourong/p/6043914.html)
[jsx](http://www.infoq.com/cn/articles/react-jsx-and-component)
## 工具
[npm常用命令详解](http://www.cnblogs.com/PeunZhang/p/5553574.html)


# react native相关
[react native中文站文档](https://reactnative.cn/docs/0.51/getting-started.html)
[react native官网文档](https://facebook.github.io/react-native/docs/getting-started.html)
[理解React中es6方法创建组件的this](https://mingjiezhang.github.io/2016/08/28/%E7%90%86%E8%A7%A3React%E4%B8%ADes6%E6%96%B9%E6%B3%95%E5%88%9B%E5%BB%BA%E7%BB%84%E4%BB%B6%E7%9A%84this/)
[React中使用ES6 class的this](https://www.zhihu.com/question/42935015/answer/153671878)
## 环境搭建及第一个react natvie应用
### mac环境搭建
~~~
brew install node
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
npm install -g yarn react-native-cli
yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global
~~~
## 第一个react natvie应用
[环境搭建及第一个react natvie应用](https://reactnative.cn/docs/0.51/getting-started.html#content)
~~~
react-native init reactNativeCompent   --version 0.55.4  # 不能有'-'字符; 要使用0.554版本的rn，这个版本稳定bug少;
cd reactNativeCompent
react-native run-ios --simulator "iPhone 6"  # 需要等待编译一会 大概五分钟

项目目录:
./
├── App.js
├── __tests__
├── android
├── app.json
├── index.js
├── ios
├── node_modules
├── package.json
└── yarn.lock

运行的默认页面是index.js ==> App.js


0.554版本会遇到  'config.h' file not found
解决方法: 
cd node_modules/react-native/third-party/glog-0.3.4
../../scripts/ios-configure-glog.sh


error: Build input file cannot be found: WebSocket/libfishhook.a
解决方法:
libfishhook.a 删除再重新导入一次
~~~
## props state讲解
[React 中的状态（State)](https://www.jianshu.com/p/583473c37db9 )
[在React组件unmounted之后setState的报错处理](https://www.jianshu.com/p/0078e6c986e9)
## 组件的生命周期
[React Native 中组件的生命周期](https://www.race604.com/react-native-component-lifecycle/)
## 样式
[样式](https://reactnative.cn/docs/0.51/style.html#content)
[宽高](https://reactnative.cn/docs/0.51/height-and-width.html#content)
[布局](https://reactnative.cn/docs/0.51/layout-with-flexbox.html#content)
[布局](https://weibo.com/1712131295/CoRnElNkZ?ref=collection&type=comment)
[react-native 屏幕尺寸和文字大小适配](http://blog.csdn.net/u011272795/article/details/73824558)
[屏幕适配的一个库(还没仔细看)](https://www.npmjs.com/package/react-native-device-screen-switcher)
##调试方法
[React Native调试技巧与心得](http://blog.csdn.net/quanqinyang/article/details/52215652)
[React Native指定设备运行](http://blog.csdn.net/u010359739/article/details/71221309)
## 组件库
[罗列了很多组件](http://blog.csdn.net/xiangzhihong8/article/details/53968573)
[react-natvigation-导航器](https://reactnavigation.org/docs/getting-started.html)
[折线图](https://www.jianshu.com/p/507ef455eb0e)
[摄像头](http://www.hangge.com/blog/cache/detail_1618.html)
[react native 百度地图](https://www.jianshu.com/p/7192eb0065a0) #编译出错
## 高德地图组件
[react-native 高德地图组件](https://github.com/qiuxiang/react-native-amap3d)
~~~
npm install
npm i react-native-amap3d --save
cd ios 创建podfile:
platform :ios, '8.0'

target 'reactNativeCompent' do
  pod 'yoga', path: '../node_modules/react-native/ReactCommon/yoga/'
  pod 'React', path: '../node_modules/react-native/', :subspecs => [
    'DevSupport',
    'Core',
	'ART',
	'RCTActionSheet',
	'RCTGeolocation',
	'RCTImage',
	'RCTNetwork',
	'RCTPushNotification',
	'RCTSettings',
	'RCTText',
	'RCTVibration',
	'RCTWebSocket',
	'RCTLinkingIOS',
  ]
  pod 'react-native-amap3d', path: '../node_modules/react-native-amap3d/lib/ios/'
end

pod install

# 注意事项
一定要先修改react-native版本为0.52.2，再先npm install，再install react-native-amap3d
podfile不能按照git文档中的写，缺少必要依赖，会导致编译出错
~~~

## 照片选择器(可拍照摄像类微信 但是只能选择一张照片)
[react-native-image-picker](https://github.com/react-community/react-native-image-picker)
~~~
npm install react-native-image-picker@latest --save
然后手动加入依赖，千万不要和文档中描述的做react-native link操作，会导致编译错误
只能选择一张照片
~~~

## 照片选择器(可多选)
[react-native-syan-image-picker](https://github.com/syanbo/react-native-syan-image-picker)
~~~
npm install react-native-syan-image-picker --save
然后手动加入依赖，千万不要和文档中描述的做react-native link操作，会导致编译错误
~~~


## toast悬浮窗(几秒就消失的那种)
[react-native-easy-toast](https://github.com/crazycodeboy/react-native-easy-toast)
~~~
npm install react-native-easy-toast --save 
#不涉及混编的依赖，上面这条命令就够了
~~~

## 启动页设置
[启动页设置](https://github.com/crazycodeboy/react-native-splash-screen)
[启动页设置讲解](https://www.jianshu.com/p/4540ac17dfd4)
~~~
npm i react-native-splash-screen --save
手动导入依赖
修改xcode中LaunchImages，注意，如果图片尺寸不对xcode会报资源编译错误

iphone LaunchImages尺寸列表
各种尺寸启动图：640 × 960，640 × 1136，750 × 1334，1242 × 2208，（横平需要2208 ×1242）
iPhone Portrait iOS5,6(1x:320 × 480 pixels, 2x:640 × 960 pixels, Retina 4:640 × 1136 pixels)
iPhone Portrait iOS8,9(Retina HD 5.5”:1242 × 2208 pixels, Retina HD 4.7”:750 × 1334 pixels)
iPhone Landscape iOS 8,9(Retina HD 5.5”:2208 × 1242 pixels)
iPhone Portrait iOS7,9(2x:640 × 960 pixels, Retina 4:640 × 1136 pixels)
iPhone X Portrait  iOS 11+ (3x:1125 x 2436 pixels)
~~~

## 表单验证(登录页面一样的干净的页面)
[表单验证](https://github.com/gcanti/tcomb-form-native)
~~~
npm install tcomb-form-native # 不需要额外的命令
~~~

## 表单验证(适合用户信息输入)
[表单验证](https://github.com/FaridSafi/react-native-gifted-form)
~~~
npm install tcomb-form-native # 不需要额外的命令
问题: 多选框页面跳转有问题，我没看明白，还不知道如何处理
~~~

## 文本输入框(酷炫)
[文本输入框,多种样式很漂亮](https://github.com/halilb/react-native-textinput-effects)
[文本输入框，很简单干净](https://github.com/zbtang/React-Native-TextInputLayout)
~~~
npm install react-native-textinput-effects --save  
npm install rn-textinputlayout  --save

问题: react-native-textinput-effects的demo依赖了react-native-vector-icons
~~~

## 矢量图图标
[第三方网站提供了丰富的矢量图icon,这些图可以在我们的应用中被引入使用，比如react-native-vector-icons控件](https://github.com/oblador/react-native-vector-icons)
[图片名在这里查询](http://ionicons.com/) 使用时去掉前面的 "icon-" 
~~~
安装:
npm install react-native-vector-icons --save

配置:
ios:
看git上的readme，手动配置

android:
看官网的readme，使用Gradle配置
~~~

## 视频组件
[视频组件](https://github.com/react-native-community/react-native-video)
~~~
npm i -S react-native-video

tips:
手动添加xcode的依赖(添加xcodeproj到工程内、添加.a到link binary with li'braries)，不要link
source使用{uri: uri , type: type}的形式
~~~

## 弹出窗口组件
[弹出窗口](https://github.com/jacklam718/react-native-popup-dialog)
~~~
npm install --save react-native-popup-dialog
~~~

## 轮播图
[轮播图](https://github.com/leecade/react-native-swiper)
~~~
npm install --save react-native-swiper
~~~

## 网络请求
[网络请求讲解](https://reactnative.cn/docs/0.51/network.html#content)


## 数据库
[AsyncStorage存储详解及实例](http://blog.csdn.net/sinat_17775997/article/details/70170947)
[Realm数据库](https://www.jianshu.com/p/50e0efb66bdf)
[sqlite](https://github.com/andpor/react-native-sqlite-storage)

## 推送
### 相关链接
[极光推送](https://www.jiguang.cn/jpush)
[ios sdk链接](https://docs.jiguang.cn/jpush/client/iOS/ios_sdk/)
[jpush-react-native-github链接](https://github.com/jpush/jpush-react-native)
1. 安装配置
~~~
npm install jpush-react-native jcore-react-native --save
cd ios
在podfile中添加以下两行:
pod 'JPushRN', :path => '../node_modules/jpush-react-native'
pod 'JCoreRN', :path => '../node_modules/jcore-react-native'

在xcode中，target->capabilities->push notifications->on
修改AppDelegate.h, AppDelegate.m如下(在reatnative link自动生成的代码中添加ios sdk中的代码):

tips:
千万不要react-native link，也不要看github上的安装过程，也不要看极光官网的文档做配置
ios的api很少(比如有停止推送的但是没有回复推送的，坑多)
~~~
2. AppDelegate.h
~~~
/**
 * Copyright (c) 2015-present, Facebook, Inc.
 * All rights reserved.
 *
 * This source code is licensed under the BSD-style license found in the
 * LICENSE file in the root directory of this source tree. An additional grant
 * of patent rights can be found in the PATENTS file in the same directory.
 */

#import <UIKit/UIKit.h>

@interface AppDelegate<JPUSHRegisterDelegate> : UIResponder <UIApplicationDelegate>

@property (nonatomic, strong) UIWindow *window;

@end
~~~
3. AppDelegate.m
~~~
/**
 * Copyright (c) 2015-present, Facebook, Inc.
 * All rights reserved.
 *
 * This source code is licensed under the BSD-style license found in the
 * LICENSE file in the root directory of this source tree. An additional grant
 * of patent rights can be found in the PATENTS file in the same directory.
 */

#import "AppDelegate.h"
#import <RCTJPushModule.h>
#ifdef NSFoundationVersionNumber_iOS_9_x_Max
#import <UserNotifications/UserNotifications.h>
#endif


#import <React/RCTBundleURLProvider.h>
#import <React/RCTRootView.h>

#import <AMapFoundationKit/AMapFoundationKit.h>
#import "SplashScreen.h"

@implementation AppDelegate

// 注册APNs成功并上报DeviceToken
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
{
  [JPUSHService registerDeviceToken:deviceToken];
  NSLog(@"deviceToken: %@", deviceToken);
}

// 实现注册APNs失败接口（可选）
- (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error {
  // Optional
  NSLog(@"did Fail To Register For Remote Notifications With Error: %@", error);
}

#pragma mark- JPUSHRegisterDelegate
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
  // Required,For systems with less than or equal to iOS6
  [JPUSHService handleRemoteNotification:userInfo];
  
  [[NSNotificationCenter defaultCenter] postNotificationName:kJPFDidReceiveRemoteNotification object:userInfo];
}

- (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification
{
  [[NSNotificationCenter defaultCenter] postNotificationName:kJPFDidReceiveRemoteNotification object: notification.userInfo];
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)   (UIBackgroundFetchResult))completionHandler
{
  // Required,For systems with less than or equal to iOS6
  [JPUSHService handleRemoteNotification:userInfo];
  
  [[NSNotificationCenter defaultCenter] postNotificationName:kJPFDidReceiveRemoteNotification object:userInfo];
}

- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(NSInteger))completionHandler
{
  NSDictionary * userInfo = notification.request.content.userInfo;
  if ([notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
    [JPUSHService handleRemoteNotification:userInfo];
    [[NSNotificationCenter defaultCenter] postNotificationName:kJPFDidReceiveRemoteNotification object:userInfo];
  }
  
  completionHandler(UNNotificationPresentationOptionAlert);
}

- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)())completionHandler
{
  NSDictionary * userInfo = response.notification.request.content.userInfo;
  if ([response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
    [JPUSHService handleRemoteNotification:userInfo];
    [[NSNotificationCenter defaultCenter] postNotificationName:kJPFOpenNotification object:userInfo];
  }
  
  completionHandler();
}


- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  
  //Required
  //notice: 3.0.0及以后版本注册可以这样写，也可以继续用之前的注册方式
  JPUSHRegisterEntity * entity = [[JPUSHRegisterEntity alloc] init];
  entity.types = JPAuthorizationOptionAlert|JPAuthorizationOptionBadge|JPAuthorizationOptionSound;
  if ([[UIDevice currentDevice].systemVersion floatValue] >= 8.0) {
    // 可以添加自定义categories
    // NSSet<UNNotificationCategory *> *categories for iOS10 or later
    // NSSet<UIUserNotificationCategory *> *categories for iOS8 and iOS9
  }
  [JPUSHService registerForRemoteNotificationConfig:entity delegate:self];
  
  // apsForProduction: 0 (默认值)表示采用的是开发证书，1 表示采用生产证书发布应用。
  [JPUSHService setupWithOption:launchOptions
                        appKey:@"eaa4fa06d4d5bf359436bbfa"
                        channel:@"App Store"
                        apsForProduction:0
                        advertisingIdentifier:false];
  
  NSURL *jsCodeLocation;
  
  jsCodeLocation = [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
  
  RCTRootView *rootView = [[RCTRootView alloc] initWithBundleURL:jsCodeLocation
                                                      moduleName:@"reactNativeCompent"
                                               initialProperties:nil
                                                   launchOptions:launchOptions];
  rootView.backgroundColor = [[UIColor alloc] initWithRed:1.0f green:1.0f blue:1.0f alpha:1];
  
  self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
  UIViewController *rootViewController = [UIViewController new];
  rootViewController.view = rootView;
  self.window.rootViewController = rootViewController;
  [self.window makeKeyAndVisible];
  
  [AMapServices sharedServices].apiKey = @"9b0f44b9325584c14b3351f2f689478a";
  [SplashScreen show];
  
  return YES;
}

@end
~~~

## 热更新
[热更新](https://github.com/reactnativecn/react-native-pushy)

## 与原生代码交互及发布app
[App Store 审核指南](https://developer.apple.com/app-store/review/guidelines/cn/)
[React-Native调用iOS原生方法](http://blog.csdn.net/zww1984774346/article/details/71167775)

## 错误记录
1. react-native run-ios发生错误一般处理方法
[react-native-run-ios发生错误一般处理方法](https://segmentfault.com/q/1010000009729921)
2. Unable to resolve module
~~~
bundling failed: Error: Unable to resolve module `react-native-vector-icons/FontAwesome` from ...: Module does not exist in the module map

This might be related to https://github.com/facebook/react-native/issues/4968
To resolve try the following:
  1. Clear watchman watches: `watchman watch-del-all`.
  2. Delete the `node_modules` folder: `rm -rf node_modules && npm install`.
  3. Reset Metro Bundler cache: `rm -rf /tmp/metro-bundler-cache-*` or `npm start -- --reset-cache`.  4. Remove haste cache: `rm -rf /tmp/haste-map-react-native-packager-*`.
以上错误是因为react-native-vector-icons这个组件没有被install
解决方式: npm install react-native-vector-icons
~~~

# React native禁止字体跟随系统字体大小缩放
## 必要性
~~~
app内的字体大小在适配的时候已经设定好了，如果app内字体大小随系统字体大小变化，那么样式就会发生很大的变化或bug
~~~
## android
~~~
android >app >src >main >java > 包名 >MainActivity.java

import android.content.res.Configuration;
import android.content.res.Resources; 

后面类要加入
@Override
public Resources getResources() {
  Resources res = super.getResources();
  Configuration config=new Configuration();
  config.setToDefaults();
  res.updateConfiguration(config,res.getDisplayMetrics() );
  return res;
}
~~~

## ios
~~~
node_modules/react-native/React/Views/RCTFont.mm中查找如下代码:
if (scaleMultiplier > 0.0 && scaleMultiplier != 1.0) {
    fontSize = round(fontSize * scaleMultiplier);
}
替换成下面的代码:
if(scaleMultiplier >0.0&& scaleMultiplier !=1.0) {
  fontSize =round(fontSize);
}
~~~

# Context api
~~~
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

const enStrings = {
  submit: "Submit",
  cancel: "Cancel"
};

const cnStrings = {
  submit: "提交",
  cancel: "取消"
};
const data = "data";
const LocaleContext = React.createContext(enStrings, data);

class LocaleProvider extends React.Component {
  state = { locale: cnStrings };
  toggleLocale = () => {
    const locale =
      this.state.locale === enStrings
        ? cnStrings
        : enStrings;
    this.setState({ locale });
  };
  render() {
    return (
      <LocaleContext.Provider value={this.state.locale}>
        <button onClick={this.toggleLocale}>
          切换语言
        </button>
        {this.props.children}
      </LocaleContext.Provider>
    );
  }
}

class App extends Component {
  render() {
    return (
    <LocaleContext.Consumer>
        {
            (local, data) => (
                <div className="App">
                    <header className="App-header">
                      <img src={logo} className="App-logo" alt="logo" />
                      <h1 className="App-title">Welcome to React</h1>
                    </header>
                    <p className="App-intro">
                      To get started, edit <code>src/App.js</code> and save to reload.
                    </p>
                    <p> {local.cancel}</p>
                    <p> {data + " dasd "}</p>
                  </div>
            )
        }
     </LocaleContext.Consumer>
    );
  }
}

class AppWarpper extends Component {
  render() {
    return (
      <LocaleProvider>
        <App/>
      </LocaleProvider>
    );
  }
}

export default AppWarpper;
~~~


