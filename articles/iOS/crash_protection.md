
# Crash 防护

## 相关文章

* [iOS崩溃异常处理(使用篇)](https://www.jianshu.com/p/4d32664dcfdb)
* [使用Bugly收集并分析App的崩溃信息](https://www.jianshu.com/p/b0afae74d34b)
* [AvoidCrash -- 远离常见的崩溃](https://www.jianshu.com/p/b7a7ae0c9243)
* [iOS面试题：你认为开发中那些导致crash?](https://www.jianshu.com/p/cde95701266d)
    
* [iOS程序异常Crash友好化处理](https://mp.weixin.qq.com/s/rVPVUwFdsn8nYBJZM8YGyg)
* [Instruments 检测 App 耗电量实战](https://mp.weixin.qq.com/s/jWTXynxJ2qEUGO_b6GzbjQ)
* [iOS笔记-记录一次内存泄漏发现过程](https://mp.weixin.qq.com/s/g5WxKO0eOf6ho_9s8nSrgw)
* [JJException保护iOS App不闪退](https://mp.weixin.qq.com/s/s5256sLLdMQv94n4tN1bQw)
* [iOS监控-启动crash](https://mp.weixin.qq.com/s?__biz=MjM5OTM0MzIwMQ==&mid=2652560466&idx=1&sn=1fe5bf728754f6028d026c231b34f5c2&chksm=bcd2995c8ba5104ace0a6a685451bed12f1eb0745b70d791c2d360ba8e41ab9f7859b4af1cb3&scene=21#wechat_redirect)

## 一、关于崩溃的避免

崩溃的严重性不言而喻，对于崩溃的处理，这里有一个不错的三方：`AvoidCrash`，原文点[这里](https://www.jianshu.com/p/b7a7ae0c9243)。

### AvoidCrash简介

* 这个框架利用runtime技术对一些常用并且容易导致崩溃的方法进行处理，可以有效的防止崩溃。
* 并且打印出具体是哪个方法会导致崩溃，让你快速定位导致崩溃的代码。
* 你可以获取到原本导致崩溃的主要信息<由于这个框架的存在，并不会崩溃>，进行相应的处理。比如：
    * 你可以将这些崩溃信息发送到自己服务器。
    * 你若集成了第三方崩溃日志收集的SDK,比如你用了腾讯的Bugly,你可以上报自定义异常。
* 或许你会问有JSPatch就可以下发补丁来修复bug,为什么要用AvoidCrash？我只能说，AvoidCrash可以有效防止部分常见崩溃，JSPatch可以快速修复bug.推荐将两者都集成到项目中去。

### 使用方法

在AppDelegate的didFinishLaunchingWithOptions方法中添加如下代码，让AvoidCrash生效

```
//这句代码会让AvoidCrash生效，若没有如下代码，则AvoidCrash就不起作用
[AvoidCrash becomeEffective];

   /*
    *  [AvoidCrash becomeEffective]，是全局生效。若你只需要部分生效，你可以单个进行处理，比如:
    *  [NSArray avoidCrashExchangeMethod];
    *  [NSMutableArray avoidCrashExchangeMethod];
    *  .................
    */
```

若你想要获取崩溃日志的所有详细信息，只需添加通知的监听，监听的通知名为:AvoidCrashNotification

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    [AvoidCrash becomeEffective];
    
    //监听通知:AvoidCrashNotification, 获取AvoidCrash捕获的崩溃日志的详细信息
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(dealwithCrashMessage:) name:AvoidCrashNotification object:nil];
    return YES;
}

- (void)dealwithCrashMessage:(NSNotification *)note {
    //注意:所有的信息都在userInfo中
    //你可以在这里收集相应的崩溃信息进行相应的处理(比如传到自己服务器)
    NSLog(@"%@",note.userInfo);
}
```

## 二、关于崩溃的统计

这里用腾讯的Bugly进行介绍。

### 集成

[集成1](https://www.jianshu.com/p/4d32664dcfdb)，[集成2](https://www.jianshu.com/p/b0afae74d34b)

### 使用

到我们的APP的appDelegate中didFinishLaunchingWithOptions方法内调用初始化方法即可。

```
// 头文件
#import <Bugly/Bugly.h>

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    [Bugly startWithAppId:@"此处替换为你的AppId"];

    return YES;
}

```

### 进阶

#### 自定义上报异常

```
/**
*  上报自定义异常
*  @param exception 异常信息
*/
+ (void)reportException:(nonnull NSException *)exception;
/**
*  上报错误
*  @param error 错误信息
*/
+ (void)reportError:(NSError *)error;
```

** 配合AvoidCrash使用 **

配合上AvoidCrash，使用上报自定义异常方法，我们就既能避免崩溃，又能监听异常！

```
//AvoidCrash异常通知监听方法，在这里我们可以调用reportException方法进行上报
- (void)dealwithCrashMessage:(NSNotification *)notification {
    NSLog(@"\n🚫\n🚫监测到崩溃信息🚫\n🚫\n");
    
    NSException *exception = [NSException exceptionWithName:@"AvoidCrash" reason:[notification valueForKeyPath:@"userInfo.errorName"] userInfo:notification.userInfo];
    [Bugly reportException:exception];
}
```











