<!-- fftw3.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 日 5月 14 21:14:40 2017 (+0800)
;; Last-Updated: 日 5月 14 22:57:34 2017 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 1
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
fftw_plan fftw_plan_dft_1d(int n, fftw_complex *in, fftw_complex *out, int sign, unsigned flags);
fftw_plan fftw_plan_dft_2d(int n0, int n1, fftw_complex *in, fftw_complex *out, int sign, unsigned flags);
fftw_plan fftw_plan_dft_3d(int n0, int n1, int n2, fftw_complex *in, fftw_complex *out, int sign, unsigned flags);
fftw_plan fftw_plan_dft(int rank, const int *n, fftw_complex *in, fftw_complex *out, int sign, unsigned flags);


fftw_plan fftw_plan_dft_r2c_1d(int n, double *in, fftw_complex *out, unsigned flags);
fftw_plan fftw_plan_dft_r2c_2d(int n0, int n1, double *in, fftw_complex *out, unsigned flags);
fftw_plan fftw_plan_dft_r2c_3d(int n0, int n1, int n2, double *in, fftw_complex *out, unsigned flags);
fftw_plan fftw_plan_dft_r2c(int rank, const int *n, double *in, fftw_complex *out, unsigned flags);

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





<!-- fftw3.md ends here -->
