
# cocoapods相关

## 相关链接
[RVM 解决 Ruby 的版本问题](http://www.cnblogs.com/Ray-liang/p/5012637.html)
[rvm官方](https://rvm.io/)
[iOS - rvm、Ruby环境CocoaPods安装使用及相关错误处理](http://www.jianshu.com/p/a7cbae01ad6c)
[Cocoapods 版本升级](http://www.jianshu.com/p/82a6d6c7b000)
[mac下查看ip地址](http://jingyan.baidu.com/article/d5c4b52bcf0408da560dc502.html)


* ruby版本号为2.3.0时，pod版本号为0.39.0时 更新（pod update --verbose --no-repo-update）报错  
* 将ruby版本号改为2.2.4，pod版本号为0.39.0时 更新（pod update --verbose --no-repo-update）成功
* 列出ruby版本列表：rvm list known
* 列出已安装ruby版本列表：rvm list
* ruby user 版本号：切换ruby版本
* ruby -v 查看ruby版本号


```
pod install --verbose --no-repo-update
pod install --repo-update
pod --version
```

```
https://gems.ruby-china.com/
https://rubygems.org/
https://ruby.taobao.org/
gem sources --add https://ruby.taobao.org/ --remove https://gems.ruby-china.com/
```
