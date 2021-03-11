## 函数

* `isdigit(<#int _c#>)` 判断字符型是否是数字
* `int isspace(int c) ` 检查所传的字符是否是空白字符。
  * 标准的空白字符包括：
  ```
  ' '     (0x20)    space (SPC) 空格符
  '\t'    (0x09)    horizontal tab (TAB) 水平制表符    
  '\n'    (0x0a)    newline (LF) 换行符
  '\v'    (0x0b)    vertical tab (VT) 垂直制表符
  '\f'    (0x0c)    feed (FF) 换页符
  '\r'    (0x0d)    carriage return (CR) 回车符
  ```

## queue（队列）

**相关链接**
* [C++ 中queue（队列）的用法](https://www.cnblogs.com/yoke/p/6080092.html)

**头文件**
* `#include <queue>` 

**初始化**  
* `queue<int> mQueue;`
  
**用法**  
* 从已有元素后面增加元素   M.push()
* 清除第一个元素          M.pop() 
* 显示最后一个元素        M.back()
* 显示第一个元素          M.front()
* 查看是否为空范例         M.empty()    是的话返回1，不是返回0;
* 输出现有元素的个数       M.size()

## vector（容器）

**相关链接**
* [C++ vector 容器浅析](https://www.runoob.com/w3cnote/cpp-vector-container-analysis.html)
* [C++ vector用法解析](https://zhuanlan.zhihu.com/p/130249122)

**头文件**
* `#include <vector> `

**初始化**
* `vector<vector<int>> res;`

**用法**  
* 在vector的末尾插入新元素  vec.push_back(1);
* 删除最后一个元素          vec.pop_back();
* 清除所有元素              vec.clear();
* 判断该数组是否为空        vec.empty();
* 遍历数组
```
//向数组一样利用下标进行访问
vector<int> a;
for(int i=0;i<a.size();i++){
     cout<<a[i];
}

//利用迭代器进行访问
vector<int>::iterator it;
for(it=a.begin();it!=a.end();it++){
   cout<<*it;
}
```

