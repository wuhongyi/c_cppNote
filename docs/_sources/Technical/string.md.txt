<!-- string.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 五 11月 25 18:39:37 2016 (+0800)
;; Last-Updated: 五 11月 25 18:41:36 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 1
;; URL: http://wuhongyi.cn -->

# string

C++能够执行自动类型转换，所以可以将C字符串储存在string类型的变量中。例如以下代码可以很好地工作：

```cpp
char a_c_string[]=''This is my C string.'';
string string_variable;
string_nariable=a_c_string;
```

string对象不能自动转换为C字符串。为了获得对应于string对象的一个C字符串，必须执行显示的类型转换。这需要用到string成员函数c_str()。用strcpy来复制字符串。

```cpp
strcpy(a_c_string,string_variable.c_str());
```

注意，需要用strcpy函数来进行复制。成员函数c_str()返回与string调用对象对应的一个C字符串。




<!-- string.md ends here -->
