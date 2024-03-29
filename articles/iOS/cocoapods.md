

# cocoapods相关

## 相关链接
[RVM 解决 Ruby 的版本问题](http://www.cnblogs.com/Ray-liang/p/5012637.html)  
[rvm官方](https://rvm.io/)  
[iOS - rvm、Ruby环境CocoaPods安装使用及相关错误处理](http://www.jianshu.com/p/a7cbae01ad6c)  
[Cocoapods 版本升级](http://www.jianshu.com/p/82a6d6c7b000)  
[CocoaPods安装和使用](https://www.jianshu.com/p/c2f9491485ec)

## cocoapods 创建

**创建 podfile**

可执行文件同级目录执行：
```
vi podfile
```

podfile文件中：
```
target '项目名称' do
platform:ios , '8.0'
pod 'SDWebImage'                 
end
```

执行命令
```
pod install
```

因为墙，国内建议下面命令：
```
pod install --verbose --no-repo-update
pod install --repo-update
```


## 命令

### 切换源
* 查看源：gem sources -l
* ruby源：https://gems.ruby-china.com/
* ruby源：https://rubygems.org/
* 淘宝源：https://ruby.taobao.org/
* 切换命令：gem sources --add https://ruby.taobao.org/ --remove https://gems.ruby-china.com/

### rvm
* rvm list known：列出ruby版本列表
* rvm list：列出已安装ruby版本列表

### ruby
* ruby user 版本号：切换ruby版本
* ruby -v：查看ruby版本号

### pod
* pod --version：pod版本号

## 报错处理

### ruby,pod 版本号不匹配报错
* ruby版本号为2.3.0时，pod版本号为0.39.0时 更新（pod update --verbose --no-repo-update）报错  
* 将ruby版本号改为2.2.4，pod版本号为0.39.0时 更新（pod update --verbose --no-repo-update）成功

