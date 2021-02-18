
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

## 卡顿优化 -GPU
* 尽量避免短时间内大量图片的显示，尽可能将多张图片合成一张进行显示
* 尽量减少视图数量和层次
* 减少透明的视图（alpha<1），不透明的就设置opaque为YES
* 尽量避免出现离屏渲染

## 离屏渲染
在OpenGL中，GPU有2种渲染方式
1. On-Screen Rendering：当前屏幕渲染，在当前用于显示的屏幕缓冲区进行渲染操作
2. Off-Screen Rendering：离屏渲染，在当前屏幕缓冲区以外新开辟一个缓冲区进行渲染操作

### 离屏渲染消耗性能的原因
1. 需要创建新的缓冲区
2. 离屏渲染的整个过程，需要多次切换上下文环境，先是从当前屏幕（On-Screen）切换到离屏（Off-Screen）；等到离屏渲染结束以后，将离屏缓冲区的渲染结果显示到屏幕上，又需要将上下文环境从离屏切换到当前屏幕

### 哪些操作会触发离屏渲染？
1. 光栅化，layer.shouldRasterize = YES
2. 遮罩，layer.mask
3. 圆角，同时设置layer.masksToBounds = YES、layer.cornerRadius大于0。考虑通过CoreGraphics绘制裁剪圆角，或者叫美工提供圆角图片
4. 阴影，layer.shadowXXX。如果设置了layer.shadowPath就不会产生离屏渲染

## 定位优化
1. 如果只是需要快速确定用户位置，最好用CLLocationManager的requestLocation方法。定位完成后，会自动让定位硬件断电
2. 如果不是导航应用，尽量不要实时更新位置，定位完毕就关掉定位服务
3. 尽量降低定位精度，比如尽量不要使用精度最高的kCLLocationAccuracyBest
4. 需要后台定位时，尽量设置pausesLocationUpdatesAutomatically为YES，如果用户不太可能移动的时候系统会自动暂停位置更新
5. 尽量不要使用startMonitoringSignificantLocationChanges，优先考虑startMonitoringForRegion:

## 启动优化
1. 清理Xcode项目中没有使用到的图片资源：https://www.cnblogs.com/chao8888/p/6690001.html（LSUnusedResources）
2. 合并或者删减一些OC类和函数；关于清理项目中没用到的类，使用工具AppCode代码检查功能，查到当前项目中没有用到的类（也可以用根据linkmap文件来分析，但是准确度不算很高）；有一个叫做[FUI](https://github.com/dblock/fui)的开源项目能很好的分析出不再使用的类，准确率非常高，唯一的问题是它处理不了动态库和静态库里提供的类，也处理不了C++的类模板。
3. 删减一些无用的静态变量
4. 减少非系统库的依赖、合并非系统库
5. 减少Objc类数量、减少selector数量、减少C++虚函数数量
6. 将不必须在+load方法中做的事情延迟到+initialize中，尽量不要用C++虚函数(创建虚函数表有开销)；
7. 减少依赖不必要的库，不管是动态库还是静态库；如果可以的话，把动态库改造成静态库；如果必须依赖动态库，则把多个非系统的动态库合并成一个动态库；
8. 不使用xib加载首页
9. 友盟的分享服务，没有必要在启动的时候去初始化，初始化任务丢到后台线程解决，大概600-800ms；
10. 类和方法名不要太长
    * iOS每个类和方法名都在__cstring段里都存了相应的字符串值，所以类和方法名的长短也是对可执行文件大小是有影响的；因还是object-c的动态特性，因为需要通过类/方法名反射找到这个类/方法进行调用，object-c对象模型会把类/方法名字符串都保存下来；
11. 通过添加环境变量可以打印出APP的启动时间分析（Edit scheme -> Run -> Arguments）
DYLD_PRINT_STATISTICS设置为1
如果需要更详细的信息，那就将DYLD_PRINT_STATISTICS_DETAILS设置为1

## 卡顿优化
* 减少视图数量和层次
* 页面关闭后，取消没必要的网络请求
* 将耗时的工作放到子线程中
    * 启动时友盟分享服务
    * 本地存储
    * 网络请求
* 循环创建大量临时对象时用Autorelease Pool
* 懒加载的使用
    * vc中创建view
    * 计算高度等
* 避免离屏渲染
    * 设计切图或者绘制裁剪圆角
    * 阴影，layer.shadowXXX。如果设置了layer.shadowPath就不会产生离屏渲染
    * iOS面试题：如何高性能的给 UIImageView 加个圆角?:https://www.jianshu.com/p/d568a7e078e9

## 其他
懒加载（什么时候用什么时候去创建）
* 对象的创建，放到getter方法中，用到的时候再去创建
* 项目中的一些遮层view，用到时再去创建消耗更少的内存











