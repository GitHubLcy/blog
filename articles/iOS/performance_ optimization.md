
# iOS 性能优化

[iOS APP性能检测&优化（一）](https://www.jianshu.com/p/df6e281fff12)  
[iOS 保持界面流畅的技巧](https://blog.ibireme.com/2015/11/12/smooth_user_interfaces_for_ios/)  
[iOS-MLeaksFinder详解](https://www.jianshu.com/p/eb2edcd24791?utm_source=desktop&utm_medium=timeline)  
[iOS性能优化-UI卡顿检测](https://blog.csdn.net/u010262501/article/details/79616963)  

## 启动优化
[iOS启动时间优化](https://mp.weixin.qq.com/s/lRBdtwh7BmO6i3yDBRGTwA)
[iOS启动优化](http://lingyuncxb.com/2018/01/30/iOS%E5%90%AF%E5%8A%A8%E4%BC%98%E5%8C%96/)
[iOS面试题：App启动过慢，你可能想到的因素有哪些？](https://www.jianshu.com/p/998f06517eba)
[优化 App 的启动时间实践 iOS](https://mp.weixin.qq.com/s?__biz=MjM5OTM0MzIwMQ==&mid=2652560354&idx=2&sn=ccaa3ecbfa1018f8a3738ac886c8673d&chksm=bcd29eec8ba517fad5bcc7b67b38246edd985e1196f5ad432ed7f42d8df1e6c7df8b608598d7&scene=21#wechat_redirect)

## 原理篇
[iOS开发-视图渲染与性能优化](https://mp.weixin.qq.com/s/048AFZJYi-Tbf35NtGwRBg)
[iOS性能优化——图片加载和处理](https://mp.weixin.qq.com/s/YLUA4htPI360sd5PQjsayQ)
[iOS界面渲染流程分析](https://mp.weixin.qq.com/s/drWXwnLcEw656wt3kD4PUA)

## 整体优化
[iOS性能优化篇](https://mp.weixin.qq.com/s/Ri-5nk4igNnL4KBXEzi5yA)
[iOS-性能优化深入探究](https://mp.weixin.qq.com/s/653JVbLJx3zIfBoxR9WXKw)
[iOS 界面性能优化浅析](https://mp.weixin.qq.com/s/ZpwMCHIGPZviMyXiDWR_0g)

## 卡顿优化 -CPU
* 尽量用轻量级的对象，比如用不到事件处理的地方，可以考虑使用CALayer取代UIView
* 图片的size最好刚好跟UIImageView的size保持一致
* 控制一下线程的最大并发数量
* 不要频繁地调用UIView的相关属性，比如frame、bounds、transform等属性，尽量减少不必要的修改
* 尽量提前计算好布局，在有需要时一次性调整对应的属性，不要多次修改属性
* Autolayout会比直接设置frame消耗更多的CPU资源
* 尽量把耗时的操作放到子线程
* 高开销对象，顾名思义就是初始化很耗性能的对象。比如：NSDateFormatter , NSCalendar .为了避免频繁创建，我们可以使用一个全局单例强引用着这个对象，保证整个App 的生命周期只被初始化一次。







