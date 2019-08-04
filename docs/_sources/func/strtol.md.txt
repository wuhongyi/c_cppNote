<!-- strtol.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 三 7月  5 19:38:14 2017 (+0800)
;; Last-Updated: 三 7月  5 19:40:24 2017 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 1
;; URL: http://wuhongyi.cn -->

# strtol

strtol函数会将参数nptr字符串根据参数base来转换成长整型数。


## 函数定义

```cpp
long int strtol(const char *nptr,char **endptr,int base);
```

## 函数说明


参数base范围从2至36，或0。参数base代表采用的进制方式，如base值为10则采用10进制，若base值为16则采用16进制等。当base值为0时则是采用10进制做转换，但遇到如’0x’前置字符则会使用16进制做转换、遇到’0’前置字符而不是’0x’的时候会使用8进制做转换。

一开始strtol()会扫描参数nptr字符串，跳过前面的空格字符，直到遇上数字或正负符号才开始做转换，再遇到非数字或字符串结束时('\0')结束转换，并将结果返回。若参数endptr不为NULL，则会将遇到不合条件而终止的nptr中的字符指针由endptr返回；若参数endptr为NULL，则会不返回非法字符串。


- 不仅可以识别十进制整数，还可以识别其它进制的整数，取决于base参数，比如strtol("0XDEADbeE~~", NULL, 16)返回0xdeadbee的值，strtol("0777~~", NULL, 8)返回0777的值。
- endptr是一个传出参数，函数返回时指向后面未被识别的第一个字符。例如char *pos; strtol("123abc", &pos, 10);，strtol返回123，pos指向字符串中的字母a。如果字符串开头没有可识别的整数，例如char *pos; strtol("ABCabc", &pos, 10);，则strtol返回0，pos指向字符串开头，可以据此判断这种出错的情况，而这是atoi处理不了的。
- 如果字符串中的整数值超出long int的表示范围（上溢或下溢），则strtol返回它所能表示的最大（或最小）整数，并设置errno为ERANGE，例如strtol("0XDEADbeef~~", NULL, 16)返回0x7fffffff并设置errno为ERANGE


```cpp
#include<stdlib.h>
#include<stdio.h>
int main()
{
    char *string, *stopstring;
    double x;
    int base;
    long l;
    unsigned long ul;
    string = "3.1415926 This stopped it";
    x = strtod(string, &stopstring);
    printf("string = %s\n", string);
    printf("strtod = %f\n", x);
    printf("Stopped scan at: %s\n", stopstring);
    string = "-1011 This stopped it";
    l = strtol(string, &stopstring, 10);
    printf("string = %s\n", string);
    printf("strtol = %ld\n", l);
    printf("Stopped scan at: %s\n", stopstring);
    string = "10110134932";
    printf("string = %s\n", string);
    /*Convertstringusingbase2,4,and8:*/
    for(base = 2; base <= 8; base *= 2)
    {
        /*Convertthestring:*/
        ul = strtoul(string, &stopstring, base);
        printf("strtol = %ld(base %d)\n", ul, base);
        printf("Stopped scan at: %s\n", stopstring);
    }
    return 0;
}
```




<!-- strtol.md ends here -->
