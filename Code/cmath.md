<!-- cmath.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 三 8月 10 19:54:12 2016 (+0800)
;; Last-Updated: 三 8月 10 19:54:56 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 1
;; URL: http://wuhongyi.github.io -->

# cmath

```cpp
#include <cmath> //(math.h)

abs 绝对值
acos 反余弦
acosh 反双曲余弦值（C++11）
asin 反正弦
asinh 反双曲正弦值（C++11）
atan 反正切
atan2 y/x的反正切
atanh 反双曲正切值（C++11）
cbrt 立方根（C++11）
ceil 上取整，不小于给定值的最近整数
copysign 以第二个参数y的符号（正或负）返回第一个参数x
cos 余弦
cosh 双曲余弦值
erf 误差函数（也称之为高斯误差函数）（C++11）
erfc 余补误差函数1-erf() （C++11）
exp 以自然常数e为底的指数函数
exp2 以2为底的指数函数（C++11）
expm1 函数返回 exp(x) - 1（C++11）
fabs 返回值为浮点型的绝对值
fdim 如果X>Y返回X-Y,否则返回0（C++11）
floor 下取整，不大于给定值的最近整数
fma 返回x*y+z,整个运算完成之后，只进行一次舍入（C++11）
fmax 返回参数较大值（C++11）
fmin 返回参数较小值（C++11）
fmod 求余，获得浮点数除法操作的余数
fpclassify （C++11）
frexp 把一个浮点数分解为尾数和指数，其中 x = 尾数 * 2^指数，尾数为[0.5，1） 
hypot 计算两个数平方的和的平方根。对于给定的直角三角形的两个直角边,求其斜边的长度（C++11）
ilogb 把一个浮点数分解为尾数和指数，返回指数值。其中 x = 尾数 * 2^指数，尾数为[1，2）（C++11）
isfinite 返回x是否是一个有限值。一个有限值是任何浮点值，既不是无限的也不是非数字（C++11）
isgreater 返回是否x大于y（C++11）
isgreaterequal 返回是否x大于或等于y（C++11）
isinf 返回是否x是一个无穷大的值（正无穷大或负无穷大）。（C++11）
isless 返回是否x小于y（C++11）
islessequal 返回是否x小于或等于y（（C++11）
islessgreater 返回是否x小于或大于y（C++11）
isnan 返回是否x是非数字（C++11）
isnormal 返回是否x是一个正常值，即不是无穷的、非数字、零、低于正常的（C++11）
isunordered 返回是否x或y是非数字。检测两个浮点数是否是无序的，只要有一个是 NaN，就不能比较（C++11）
ldexp 计算value乘以2的exp次幂 （ value * ( 2^exp ) ），其中exp为整型
lgamma 返回X的伽玛函数的绝对值的自然对数（C++11）
llrint 返回最靠近x的整数？？？（C++11）
llround 返回最靠近x的整数？？？（C++11）
log 计算自然（以 e 为底）对数（ln(x)）
log10 计算普通（以 10 为底）对数（log10(x)）
log1p 计算 1 与给定值 x 的和（1+x）的自然对数（ln(1+x)）（C++11）
log2 计算给定数的以 2 为底的对数（log2(x)）（C++11）
logb 计算给定数的以 2 为底的对数，x取绝对值（log2(|x|)）（C++11）
lrint 返回最靠近x的整数？？？（C++11）
lround 返回最靠近x的整数？？？（C++11）
modf 将一个浮点数分解为整数及小数部分
nan 将执行时定义的字符串作为静态化非数型（Quiet NaN）操作所需的值（C++11）
nanf 将执行时定义的字符串作为静态化非数型（Quiet NaN）操作所需的值（C++11）
nanl 将执行时定义的字符串作为静态化非数型（Quiet NaN）操作所需的值（C++11）
nearbyint （C++11）
nextafter （C++11）
nexttoward （C++11）
pow 幂运算
remainder 获得浮点数除法操作的带符号余数。取到最近的，最小的余数（C++11）
remquo 获得浮点数除法操作的带符号余数，且返回符号及操作结果的最后三位组成的整数（C++11）
rint 返回最靠近x的整数？？？（C++11）
round 返回最靠近x的整数？？？（C++11）
scalbln 计算scalbln(x,n) = x * 2^n,其中n为long int 型（C++11）
scalbn 计算scalbn(x,n) = x * 2^n,其中n为 int 型（C++11）
signbit 返回是否x的符号是负的，可以应用到无穷、非数字、零；如果零无符号，它是正的（C++11）
sin 正弦函数
sinh 双曲正弦
sqrt 计算平方根
tan 正切函数
tanh 双曲正切
tgamma 返回X的伽玛函数（C++11）
trunc 幅度（到 0 的距离，即绝对值）不大于给定值的最近整数（C++11）
```

<!-- cmath.md ends here -->
