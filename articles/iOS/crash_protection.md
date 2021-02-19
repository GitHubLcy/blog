
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

## 一、关于崩溃

崩溃的严重性不言而喻，对于崩溃的处理，这里有一个不错的三方：`AvoidCrash`，原文点[这里](https://www.jianshu.com/p/b7a7ae0c9243)。  

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



