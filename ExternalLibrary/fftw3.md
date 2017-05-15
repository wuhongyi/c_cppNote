<!-- fftw3.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 日 5月 14 21:14:40 2017 (+0800)
;; Last-Updated: 一 5月 15 21:48:05 2017 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 2
;; URL: http://wuhongyi.cn -->

# fftw3

## 基本使用入门

The basic usage of FFTW to compute a one-dimensional DFT of size N is simple, and it
typically looks something like this code:

```cpp
// -lfftw3 -lm
#include <fftw3.h>
//...
{
fftw_complex *in, *out;
fftw_plan p;
//...
in = (fftw_complex*) fftw_malloc(sizeof(fftw_complex) * N);
out = (fftw_complex*) fftw_malloc(sizeof(fftw_complex) * N);
p = fftw_plan_dft_1d(N, in, out, FFTW_FORWARD, FFTW_ESTIMATE);
//...
fftw_execute(p); /* repeat as needed */
//...
fftw_destroy_plan(p);
fftw_free(in); fftw_free(out);
}
```

You must link this code with the fftw3 library. On Unix systems, link with -lfftw3 -lm.
The example code first allocates the input and output arrays. You can allocate them in
any way that you like, but we recommend using **fftw_malloc**, which behaves like malloc
except that it properly aligns the array when SIMD instructions (such as SSE and Altivec)
are available. [Alternatively, we provide a convenient wrapper function **fftw_alloc_complex(N)** which has the same effect.]

The data is an array of type **fftw_complex**, which is by default a double[2] composed of
the real (in[i][0]) and imaginary (in[i][1]) parts of a complex number.

The next step is to create a plan, which is an object that contains all the data that FFTW needs to compute the FFT. This function creates the plan:
```cpp
fftw_plan fftw_plan_dft_1d(int n, fftw_complex *in, fftw_complex *out, int sign, unsigned flags);
```
The first argument, n, is the size of the transform you are trying to compute. The size n
can be any positive integer, but sizes that are products of small factors are transformed
most efficiently (although prime sizes still use an O(n log n) algorithm).

The next two arguments are pointers to the input and output arrays of the transform.
These pointers can be equal, indicating an in-place transform.

The fourth argument, **sign**, can be either **FFTW_FORWARD (-1)** or **FFTW_BACKWARD (+1)**, and
indicates the direction of the transform you are interested in; technically, it is the sign of the exponent in the transform.

The **flags** argument is usually either **FFTW_MEASURE** or **FFTW_ESTIMATE**. **FFTW_MEASURE** instructs FFTW to run and measure the execution time of several FFTs in order to find the best way to compute the transform of size n. This process takes some time (usually a few seconds), depending on your machine and on the size of the transform. **FFTW_ESTIMATE**, on the contrary, does not run any computation and just builds a reasonable plan that is probably sub-optimal. In short, if your program performs many transforms of the same size and initialization time is not important, use **FFTW_MEASURE**; otherwise use the estimate.

*You must create the plan before initializing the input*, because **FFTW_MEASURE** overwrites the in/out arrays. (Technically, **FFTW_ESTIMATE** does not touch your arrays, but you should always create plans first just to be sure.)

Once the plan has been created, you can use it as many times as you like for transforms on the specified in/out arrays, computing the actual transforms via **fftw_execute(plan)**:
```cpp
void fftw_execute(const fftw_plan plan);
```
The DFT results are stored in-order in the array out, with the zero-frequency (DC) component in out[0]. If in != out, the transform is out-of-place and the input array in is not
modified. Otherwise, the input array is overwritten with the transform.

If you want to transform a different array of the same size, you can create a new plan with **fftw_plan_dft_1d** and FFTW automatically reuses the information from the previous plan, if possible. Alternatively, with the “guru” interface you can apply a given plan to a different array, if you are careful.

When you are done with the plan, you deallocate it by calling **fftw_destroy_plan(plan)**:
```cpp
void fftw_destroy_plan(fftw_plan plan);
```
If you allocate an array with **fftw_malloc()** you must deallocate it with **fftw_free()**. Do not use free() or, heaven forbid, delete.

FFTW computes an unnormalized DFT. Thus, computing a forward followed by a backward transform (or vice versa) results in the original array scaled by n. 

If you have a C compiler, such as gcc, that supports the C99 standard, and you **#include <complex.h>** before **<fftw3.h>**, then **fftw_complex** is the native double-precision complex type and you can manipulate it with ordinary arithmetic. Otherwise, FFTW defines its own complex type, which is bit-compatible with the C99 complex type. **(The C++ <complex> template class may also be usable via a typecast.)**

----

## 基本函数

Since **fftw_malloc** only ever needs to be used for real and complex arrays, we provide two convenient wrapper routines **fftw_alloc_real(N)** and **fftw_alloc_complex(N)** that are equivalent to **(double*)fftw_malloc(sizeof(double) * N)** and **(fftw_complex*)fftw_malloc(sizeof(fftw_complex) * N)**, respectively (or their equivalents in other precisions).


```cpp
// 复数DFT
fftw_plan fftw_plan_dft_1d(int n, fftw_complex *in, fftw_complex *out, int sign, unsigned flags);
fftw_plan fftw_plan_dft_2d(int n0, int n1, fftw_complex *in, fftw_complex *out, int sign, unsigned flags);
fftw_plan fftw_plan_dft_3d(int n0, int n1, int n2, fftw_complex *in, fftw_complex *out, int sign, unsigned flags);
fftw_plan fftw_plan_dft(int rank, const int *n, fftw_complex *in, fftw_complex *out, int sign, unsigned flags);

// 实输入数据，复Hermitian输出，正变换。
fftw_plan fftw_plan_dft_r2c_1d(int n, double *in, fftw_complex *out, unsigned flags);
fftw_plan fftw_plan_dft_r2c_2d(int n0, int n1, double *in, fftw_complex *out, unsigned flags);
fftw_plan fftw_plan_dft_r2c_3d(int n0, int n1, int n2, double *in, fftw_complex *out, unsigned flags);
fftw_plan fftw_plan_dft_r2c(int rank, const int *n, double *in, fftw_complex *out, unsigned flags);

// 复Hermitian输入数据，实输出数据，逆变换。
fftw_plan fftw_plan_dft_c2r_1d(int n, fftw_complex *in, double *out, unsigned flags);
fftw_plan fftw_plan_dft_c2r_2d(int n0, int n1, fftw_complex *in, double *out, unsigned flags);
fftw_plan fftw_plan_dft_c2r_3d(int n0, int n1, int n2, fftw_complex *in, double *out, unsigned flags);
fftw_plan fftw_plan_dft_c2r(int rank, const int *n, fftw_complex *in, double *out, unsigned flags);

fftw_plan fftw_plan_r2r_1d(int n, double *in, double *out, fftw_r2r_kind kind, unsigned flags);
fftw_plan fftw_plan_r2r_2d(int n0, int n1, double *in, double *out, fftw_r2r_kind kind0, fftw_r2r_kind kind1, unsigned flags);
fftw_plan fftw_plan_r2r_3d(int n0, int n1, int n2, double *in, double *out, fftw_r2r_kind kind0, fftw_r2r_kind kind1, fftw_r2r_kind kind2, unsigned flags);
fftw_plan fftw_plan_r2r(int rank, const int *n, double *in, double *out, const fftw_r2r_kind *kind, unsigned flags);

fftw_plan fftw_plan_many_dft(int rank, const int *n, int howmany, fftw_complex *in, const int *inembed, int istride, int idist, fftw_complex *out, const int *onembed, int ostride, int odist, int sign, unsigned flags);
fftw_plan fftw_plan_many_dft_r2c(int rank, const int *n, int howmany, double *in, const int *inembed, int istride, int idist, fftw_complex *out, const int *onembed, int ostride, int odist, unsigned flags);
fftw_plan fftw_plan_many_dft_c2r(int rank, const int *n, int howmany, fftw_complex *in, const int *inembed, int istride, int idist, double *out, const int *onembed, int ostride, int odist, unsigned flags);
fftw_plan fftw_plan_many_r2r(int rank, const int *n, int howmany, double *in, const int *inembed, int istride, int idist, double *out, const int *onembed, int ostride, int odist, const fftw_r2r_kind *kind, unsigned flags);


// 如果想对新数组，比如大小相等的一批数组执行变换，可以使用以下接口
void fftw_execute_dft(const fftw_plan p, fftw_complex *in, fftw_complex *out);
void fftw_execute_split_dft(const fftw_plan p, double *ri, double *ii, double *ro, double *io);
void fftw_execute_dft_r2c(const fftw_plan p, double *in, fftw_complex *out);
void fftw_execute_split_dft_r2c(const fftw_plan p, double *in, double *ro, double *io);
void fftw_execute_dft_c2r(const fftw_plan p, fftw_complex *in, double *out);
void fftw_execute_split_dft_c2r(const fftw_plan p, double *ri, double *ii, double *out);
void fftw_execute_r2r(const fftw_plan p, double *in, double *out);




void *fftw_malloc(size_t n);
void fftw_free(void *p);
double *fftw_alloc_real(size_t n);
fftw_complex *fftw_alloc_complex(size_t n);


void fftw_execute(const fftw_plan plan);

void fftw_destroy_plan(fftw_plan plan);


void fftw_fprint_plan(const fftw_plan plan, FILE *output_file);
void fftw_print_plan(const fftw_plan plan);
char *fftw_sprint_plan(const fftw_plan plan);


extern void fftw_set_timelimit(double seconds);

```

## Complex numbers

The default FFTW interface uses double precision for all floating-point numbers, and defines a **fftw_complex** type to hold complex numbers as:
```cpp
typedef double fftw_complex[2];
```
Here, the [0] element holds the real part and the [1] element holds the imaginary part.

Alternatively, if you have a C compiler (such as gcc) that supports the C99 revision of the ANSI C standard, you can use C’s new native complex type (which is binary-compatible
with the typedef above). In particular, if you **#include <complex.h>** before **<fftw3.h>**, then **fftw_complex** is defined to be the native complex type and you can manipulate it with ordinary arithmetic (e.g. x = y * (3+4*I), where x and y are **fftw_complex** and I is the standard symbol for the imaginary unit);
C++ has its own **complex<T>** template class, defined in the standard **<complex>** header file. Reportedly, the C++ standards committee has recently agreed to mandate that the storage format used for this type be binary-compatible with the C99 type, i.e. an array T[2] with consecutive real [0] and imaginary [1] parts. Although not part of the official standard as of this writing, the proposal stated that: “This solution has been tested with all current major implementations of the standard library and shown to be working.” To the extent that this is true, if you have a variable **complex<double> *x**, you can pass it directly to FFTW via **reinterpret_cast<fftw_complex*>(x)**.

----

```
因为涉及C/C++混编，fftw3的函数接口又为C接口。对于C++的编程，可先定义C的变量，然后通过类型转换成C++的，即可按照C++的方式才操作该数据。

fftw\_complex默认由两个double组成，在内存中顺序排列，实部在前，虚部在后，即typedef double fftw_complex[2]。

FFTW文档指出如果有一个支持C99标准的C编译器（如gcc），可以在#include <fftw3.h> 前加入#include <complex.h>，这样一来fftw_complex就被定义为本机复数类型，而且与上述typedef二进制兼容（指内存排列）。

C++有一个复数模板类complex<T>，在头文件<complex>下定义。C++标准委员会同意该类的存储方式与C99二进制兼容，即顺序存储，实部在前，该解决方案在所有主流标准库实现中都能正确工作。所以实际上可以用complex <double> 来代替fftw_complex，比如有一个复数数组complex<double> *x，则可以将其类型转换后作为参数传递给 fftw：reinterpret_cast<fftw_complex*>(x)。

测试如下：开 两个数组 fftw_complex x1[2] 和 complex<double> x2[2]，然后赋相同值，在调试模式下可以看到它们的内存排列是相同的。complex<T>类数据赋值的方式不是很直接，必须采用无名对象方式 x2[i] = complex <double>(1,2) 或成员函数方式x2[i].real(1);x2[i].imag(2);不能直接写x2[i].real=1;x2[i].imag=2。 fftw_complex 赋值方式比较直接：x1[i][0]=1;x1[i][1]=2。最后，考虑到数据对齐，最好使用 fftw_malloc 分配内存，所以可以将其返回的指针转换为complex <double> *类型使用（比如赋值或读取等），变换时再将其转换为fftw_complex*。

```

```cpp
// #include <complex.h> //在低版本GCC中不要include该C语言头文件！！！
#include <complex>
#include <fftw3.h>

  int L = 256;
  fftw_complex *in,*out;

  in=(fftw_complex*)fftw_malloc(sizeof(fftw_complex)*L);
  out=(fftw_complex*)fftw_malloc(sizeof(fftw_complex)*L);

  std::complex<double> *inin = reinterpret_cast<std::complex<double>*>(in);
  std::complex<double> *outout = reinterpret_cast<std::complex<double>*>(out);

  // 对 inin 进行操作即对 in 进行处理
  for (int i = 0; i < L; ++i)
    {
      inin[i].real() = i;
      inin[i].imag() = 0;
    }
```

```cpp
#include<fftw3.h>
#include <complex.h>
// ...
{
  fftw_complex*in,*out;
  fftw_plan p;
  int N = 1024;
  in=(fftw_complex*)fftw_malloc(sizeof(fftw_complex)*N);
  out=(fftw_complex*)fftw_malloc(sizeof(fftw_complex)*N);

  //对输入数据in赋值
  in[0][0] = 1; in[0][1] = 0;
  in[1][0] = 1; in[1][1] = 0;
  in[2][0] = 1; in[2][1] = 0;
  // ...
  in[1023][0] = 1; in[1023][1] = 0;  


  p=fftw_plan_dft_1d(N,in,out,FFTW_FORWARD, FFTW_ESTIMATE);//生成策略
  fftw_execute(p);//执行变换
  
  fftw_destroy_plan(p);
  fftw_free(in);
  fftw_free(out);
}
```

FFTW 总的来说就是先输入，然后构造策略plan，最后执行plan就可以了。

内存空间的申请与释放 以in的分配为例，空间分配有三种可以选择的方式：  
- 直接in[N]；
- 使用ANSI C或者C++语言中的malloc，new等动态分配；
- 使用FFTW提供的fftw_malloc函数动态分配。

第一种方法的问题在于，这种方法申请的空间由编译器在栈内存里分配，栈内存是非常有限的，在windows上为2M大小，所以对于大的数据容易导致Stack Overflow的致命性错误。当然，你可以在编译器设置里边它调大一些。  
第二种方法虽然解决了第一种方法存在的问题，但是由于不同的编译器内存分配策略的不同，可能使分配的数组并没有内存对齐，而内存没有对齐，对FFTW性能上的影响将是显著的。当然，你可以使用一些语言的特性让内存对齐。  
第三种方法则是由FFTW提供的内存分配接口，它在堆内存上申请空间，并且能够保证内存对齐，同时可以在不同的平台之间顺利的移植。  
空间的分配问题解决了，空间的释放问题当然也就使用对应的方式，如果我们使用了fftw\_malloc()来分配空间，那么使用fftw\_free()释放相应的空间就行了。在内存方面值得注意的问题是，fftw\_plan是一种声明了一个变量就需要使用fftw\_destroy\_plan()来销毁的类型。


在复数据的DFT中，sign表示了是做DFT变换(FFTW\_FORWARD)还是IDFT变换(FFTW\_BACKWARD)。另外，在每一个函数中，我们都能找到一个变量叫flags，这个变量就是策略生成方案。

flags 参数一般情况下常用的为FFTW\_MEASURE 或 FFTW\_ESTIMATE。FFTW_MEASURE 表示 FFTW 会先计算一些 FFT 并测量所用的时间，以便为大小为 n 的变换寻找最优的计算方法。依据机器配置和变换的大小（n），这个过程耗费约数秒。FFTW\_ESTIMATE 则相反，它直接构造一个合理的但可能是次最优的方案。总体来说，如果你的程序需要进行大量相同大小的 FFT，并且初始化时间不重要，可以使用FFTW\_MEASURE，否则应使用 FFTW\_ESTIMATE。FFTW\_MEASURE 模式下 in 和 out 数组中的值会被覆盖，所以该方式应该在用户初始化输入数据 in 之前完成。

从以上我们知道生成一个plan很费时间，可以通过生成一个plan，然后在其他的情况下重用这个plan。当然，重用也是有条件的：

- 输入输出数据的大小相等。
- 输入输出的数据对齐不变。按照前面所说，都是用fftw_malloc()就不会有问题了。
- 变换类型、是否原位变换不变。


wisdom是另一种减少生成策略所花费的时间的方法。wisdom的大体思路就是把生成好的策略相关的配置信息存储在磁盘里，然后在下次重新运行程序的时候，把策略相关的配置信息重新载入到内存中，这样在重新生成plan的时候就可以节约大量的时间。wisdom的大体思路就是把生成好的策略相关的配置信息存储在磁盘里，然后在下次重新运行程序的时候，把策略相关的配置信息重新载入到内存中，这样在重新生成plan的时候就可以节约大量的时间。

- 每一个wisdom都是针对某一个确定的processor而言的。每次更换了运行的硬件环境，都应该重新生成wisdom。
- 每一个wisdom都是针对某一个确定的程序而言的。每次修改了程序，即最后的二进制文件不同，都需要重新生成wisdom。如果不重新生成wisdom，相对于硬件环境的改变引起的效率下降，这里的效率下降没有那么明显。
- 对相同的processor和相同的program binary，在每次运行的时候，也会因为虚拟内存使用的不同而导致程序执行效率的改变。


根据以上的描述，下面说说针对我们的需求应该采用的最佳方法：



```cpp
// 首先，使用FFTW提供的fftw\_malloc函数动态分配输入、输出变量

  int N = 1024;
  fftw_complex *x,*y;
  x=(fftw_complex*)fftw_malloc(sizeof(fftw_complex)*N);
  y=(fftw_complex*)fftw_malloc(sizeof(fftw_complex)*N);

// 然后申请一个plan策略

fftw_plan p = fftw_plan_dft_1d(N, x, y, FFTW_FORWARD, FFTW_ESTIMATE);
```

生成策略是费时的。

FFTW\_ESTIMATE生成对于1024个数据点的情况需要10us多，执行一次变换需要的时间只是2us左右。对2048个数据点，执行一次变换时间不到5us，对4096个点，执行一次变换时间不到11us。因此，不应该采用xcorr代码中那样的封装，对每次调用都生成一个策略，大量的时间都花费在生成策略上了。我们的函数封装应该传递fftw\_plan p策略以及需要变换的数据，这样运算时间可大大减小。

对FFTW\_MEASURE的生成需要的时间不同的N差异应该较大（从纳秒到秒），但是其执行一次变换的时间小于FFTW\_ESTIMATE，对我们来说采用该方式更合算。

对于不同的N，因为其采用的策略不同，所以时间上差异会很大。应该是2的次幂时候速度快。非2的次幂时候速度至少慢好几倍。但是做互相关、自相关时候，变换的长度为2N-1，不是2的次幂，应该多给加个点，让其为2N个点，速度快。

以上策略生成好之后，就是对数据的批量处理了，

```cpp
// 先对输入变量x进行赋值
// 然后执行以下变换
fftw_execute(p);//执行变换
// 执行变换之后，输出变量y内即为变换后的结果。

//我们可以重新对输入变量x进行赋值，然后再执行变换，生成的变换结果仍然在输出变量y中。这样就重复利用了该策略。


//对于封装的函数，函数内采用以下执行方式
fftw_execute_dft(p,x, y);
//也可达到重复利用策略的目的
```

----

## 示例代码


```cpp
// 假设采样频率Fs，信号频率F，信号长度L，采样点数N。那么FFT之后结果就是一个为N点的复数。每一个点就对应着一个频率点。这个点的模值，就是该频率值下的幅度特性。
// 假设原始信号的峰值为A，那么FFT的结果的每个点（除了第一个点直流分量之外）的模值就是A的N/2倍，而第一个点就是直流分量（即0Hz），它的模值是直流分量的N倍。
// 采样频率Fs,被N-1个点平均分成N等份，每个点的频率依次增加。为了方便进行FFT运算，通常N取大于信号长度L的2的整数次方。
// 例如某点n所表示的频率为：Fn=(n-1)*Fs/N。由上面的公式可以看出，Fn所能分辨到频率为为Fs/N。如果采样频率Fs为1024Hz，采样点数为1024点，则可以分辨到1Hz。1024Hz的采样率采样1024点，刚好是1秒，也就是说，采样1秒时间的信号并做FFT，则结果可以分析到1Hz。如果采样2秒时间的信号，则N为2048，并做FFT，则结果可以分析到0.5Hz。
// 如果要提高频率分辨力，则必须增加采样点数，也即采样时间。频率分辨率和采样时间是倒数关系。
// 由于FFT结果的对称性，通常我们只使用前半部分的结果，即小于采样频率一半的结果。

// 假设我们有一个信号，它含有5V的直流分量，频率为15Hz、相位为-30度、幅度为7V的交流信号以及一个频率为40Hz、相位为90度、幅度为3V的交流信号。数学表达式为：
// x = 5 + 7*cos(2*pi*15*t - 30*pi/180) + 3*cos(2*pi*40*t - 90*pi/180)。
// 我们以128Hz的采样率对这个信号进行采样，总共采样256点。按照我们上面的分析，Fn=(n-1)*Fs/N，我们可以知道，每两个点之间的间距就是0.5Hz。我们的信号有3个频率：0Hz、15Hz、40Hz
// 出于编程方便，因为直流分量的幅值A1/N，其他点幅值为An/(N/2)，故直流分量最后要除以2才是对的。
// 一般FFT所用数据点数N与原含有信号数据点数L相同，这样的频谱图具有较高的质量，可减小因补零或截断而产生的影响。

// 编译方式
// g++ `root-config --cflags --glibs`  -lfftw3 -lm main.cc -o 123

#include <complex>
#include <fftw3.h>
#include "TRandom.h"
#include <cmath>
#include "TRint.h"
#include "TGraph.h"
#include <iostream>
#include "TBenchmark.h"
#include "TCanvas.h"

int main(int argc, char *argv[])
{
  TRint *theApp = new TRint("Rint", &argc, argv);
  TCanvas *c1 = new TCanvas("c1","",600,400);
  c1->Divide(2/*col*/,1/*raw*/);

  int Fs = 128;
  double T = 1.0/Fs;
  int L = 256;

  fftw_complex *in,*out;
  in=(fftw_complex*)fftw_malloc(sizeof(fftw_complex)*L);
  out=(fftw_complex*)fftw_malloc(sizeof(fftw_complex)*L);

  double data[1024];
  for (int i = 0; i < L; ++i)
    {
      data[i] = 5+7*std::cos(2*3.14159*15*(i*T)-30*3.14159/180)+3*std::cos(2*3.14159*40*(i*T)-90*3.14159/180)+gRandom->Uniform();
      in[i][0] = data[i];
      in[i][1] = 0;
    }

  fftw_plan p;
  
  gBenchmark->Start("test");//计时开始

  p=fftw_plan_dft_1d(L,in,out,FFTW_FORWARD,FFTW_ESTIMATE);
  fftw_execute(p);//执行变换
  fftw_destroy_plan(p);
  
  for (int i = 0; i < L; ++i)//除以N乘以2才是真实幅值，N越大，幅值精度越高
    {
      out[i][0] = out[i][0]/L*2;
      out[i][1] = out[i][1]/L*2;
    }
  out[0][0]/=2.;
  out[0][1]/=2.;

  gBenchmark->Show("test");//计时结束并输出时间
  
  TGraph *g = new TGraph();
  for (int i = 0; i < L/2; ++i)
    {
      g->SetPoint(i,double(Fs)*i/L,std::sqrt(out[i][0]*out[i][0]+out[i][1]*out[i][1]));
      // std::cout<<i<<"  "<<std::sqrt(out[i][0]*out[i][0]+out[i][1]*out[i][1])<<std::endl;
    }

  c1->cd(1);
  g->Draw();

  p=fftw_plan_dft_c2r_1d(L,out,data,FFTW_ESTIMATE);//这里做个反变换，看看变换后的与原始数据的差异
  fftw_execute(p);//执行变换
  fftw_destroy_plan(p);

  TGraph *gg = new TGraph();
  for (int i = 0; i < L; ++i)
    {
      gg->SetPoint(i,i,data[i]/2);
    }
  c1->cd(2);
  gg->Draw();

  c1->Update();
  
  theApp->Run();

  delete theApp;

  return 0;
}
```

### 互相关计算

```cpp
// 编译方式
// g++ `root-config --cflags --glibs`  -lfftw3 -lm main.cc -o 123

#include <complex>//不要出现#include <complex.h>
#include <fftw3.h>
#include "TRandom.h"
#include <cmath>
#include "TRint.h"
#include "TGraph.h"
#include <iostream>
#include "TBenchmark.h"
#include "TCanvas.h"
#include <cstring>
//....oooOO0OOooo........oooOO0OOooo........oooOO0OOooo........oooOO0OOooo......

int main(int argc, char *argv[])
{
  TRint *theApp = new TRint("Rint", &argc, argv);
  TCanvas *c1 = new TCanvas("c1","",600,400);
  c1->Divide(1/*col*/,3/*raw*/);

  int N = 1281;
  int N2 = N*2-1;

  fftw_complex *x,*y;
  x=(fftw_complex*)fftw_malloc(sizeof(fftw_complex)*N);
  y=(fftw_complex*)fftw_malloc(sizeof(fftw_complex)*N);
  
  for (int i = 0; i < N; ++i)
    {
      x[i][0] = 3*std::sin(0.1*i);
      x[i][1] = 0;
      y[i][0] = std::cos(3*0.1*i);
      y[i][1] = 0;
    }


  double testdata[2561];
  for (int i = 0; i < N2; ++i)//这里是按照公式编写计算
    {
      double sum = 0;
      for (int j = 0; j < N-1-std::abs(i-(N-1)); ++j)
        {
          sum += x[j][0]*y[j+std::abs(i-(N-1))][0];
        }
      testdata[i] = sum/N;//sum/(N-std::abs(i-(N-1)));
    }

  fftw_complex * signala_ext = (fftw_complex *) fftw_malloc(N2 * sizeof(fftw_complex));
  fftw_complex * signalb_ext = (fftw_complex *) fftw_malloc(N2 * sizeof(fftw_complex));
  fftw_complex * result = (fftw_complex *) fftw_malloc(N2 * sizeof(fftw_complex));


  memset (signala_ext, 0, sizeof(fftw_complex) * (N - 1));
  memcpy(signala_ext + (N - 1), x, sizeof(fftw_complex) * N);
  memcpy(signalb_ext, y, sizeof(fftw_complex) * N);
  memset (signalb_ext + N, 0, sizeof(fftw_complex) * (N - 1));

  fftw_complex * outa =(fftw_complex *) fftw_malloc(N2 * sizeof(fftw_complex));
  fftw_complex * outb = (fftw_complex *) fftw_malloc(N2 * sizeof(fftw_complex));
  fftw_complex * out = (fftw_complex *) fftw_malloc(N2 * sizeof(fftw_complex));
  
  fftw_plan pa = fftw_plan_dft_1d(N2, signala_ext, outa, FFTW_FORWARD, FFTW_ESTIMATE);
  fftw_plan pb = fftw_plan_dft_1d(N2, signalb_ext, outb, FFTW_FORWARD, FFTW_ESTIMATE);
  fftw_plan px = fftw_plan_dft_1d(N2, out, result, FFTW_BACKWARD, FFTW_ESTIMATE);


  fftw_execute(pa);
  fftw_execute(pb);

  // 不清楚c中的fftw_complex怎么进行复数的乘法，所以进行了类型转换，转为c++的complex<double>
  std::complex<double> *outatemp = reinterpret_cast<std::complex<double>*>(outa);
  std::complex<double> *outbtemp = reinterpret_cast<std::complex<double>*>(outb);
  std::complex<double> *outtemp = reinterpret_cast<std::complex<double>*>(out);
  
  std::complex<double> scale = 1.0/(2 * N -1);
  for (int i = 0; i < N2; ++i)
    outtemp[i] = outatemp[i] * std::conj(outbtemp[i]) * scale;

  fftw_execute(px);


  TGraph *gx = new TGraph();
  TGraph *gy = new TGraph();
  TGraph *gg = new TGraph();
  
  for (int i = 0; i < N; ++i)
    {
      gx->SetPoint(i,i,x[i][0]);
      gy->SetPoint(i,i,y[i][0]);      
    }

  for (int i = -(N-1); i < N; ++i)
  {
    gg->SetPoint(i+N-1,i,result[i+N-1][0]);
    // 如果画公式编写计算的，采用下面一行
    // gg->SetPoint(i+N-1,i,testdata[i+N-1]);
  }
  
  c1->cd(1);
  gx->Draw();
  c1->cd(2);
  gy->Draw();
  
  c1->cd(3);
  gg->Draw();

  c1->Update();
  
  theApp->Run();
  delete theApp;
  return 0;
}
```




<!-- fftw3.md ends here -->
