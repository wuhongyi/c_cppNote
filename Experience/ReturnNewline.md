<!-- ReturnNewline.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 六 3月 17 16:37:16 2018 (+0800)
;; Last-Updated: 六 3月 17 16:46:57 2018 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 2
;; URL: http://wuhongyi.cn -->

# 回车符和换行符

```text
Unix系统里，每行结尾只有“<换行>”，即“\n”；
Windows系统里面，每行结尾是“<换行><回车>”，即“\n\r”；
Mac系统里，每行结尾是“<回车>”。
一个直接后果是，Unix/Mac系统下的文件在Windows里打开的话，所有文字会变成一行；
而Windows里的文件在Unix/Mac下打开的话，在每行的结尾可能会多出一个^M符号。

Linux中遇到换行符("\n")会进行回车+换行的操作，回车符反而只会作为控制字符("^M")显示，不发生回车的操作。而windows中要回车符+换行符("\r\n")才会回车+换行，缺少一个控制符或者顺序不对都不能正确的另起一行。
```



----

> http://blog.csdn.net/xiaofei2010/article/details/8458605

<!-- ReturnNewline.md ends here -->
