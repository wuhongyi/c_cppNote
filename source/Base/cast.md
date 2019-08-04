<!-- cast.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 六 5月 13 12:07:20 2017 (+0800)
;; Last-Updated: 日 8月  4 20:35:45 2019 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 2
;; URL: http://wuhongyi.cn -->

# cast

C++中的类型转换分为两种：  
- 隐式类型转换；
- 显式类型转换。

而对于隐式变换，就是标准的转换，在很多时候，不经意间就发生了，比如int类型和float类型相加时，int类型就会被隐式的转换位float类型，然后再进行相加运算。而关于隐式转换不是今天总结的重点，重点是显式转换。在标准C++中有四个类型转换符：**static_cast、dynamic_cast、const_cast和reinterpret_cast**

----

## static_cast

```
static_cast的转换格式：static_cast <type-id> (expression)
```

将expression转换为type-id类型，主要用于非多态类型之间的转换，不提供运行时的检查来确保转换的安全性。主要在以下几种场合中使用：

- 用于类层次结构中，基类和子类之间指针和引用的转换； 当进行上行转换，也就是把子类的指针或引用转换成父类表示，这种转换是安全的； 当进行下行转换，也就是把父类的指针或引用转换成子类表示，这种转换是不安全的，也需要程序员来保证；
- 用于基本数据类型之间的转换，如把int转换成char，把int转换成enum等等，这种转换的安全性需要程序员来保证；
- 把void指针转换成目标类型的指针，是及其不安全的；

**注：static_cast不能转换掉expression的const、volatile和__unaligned属性。**


## dynamic_cast

```
dynamic_cast的转换格式：dynamic_cast <type-id> (expression)
```

将expression转换为type-id类型，type-id必须是类的指针、类的引用或者是void *；如果type-id是指针类型，那么expression也必须是一个指针；如果type-id是一个引用，那么expression也必须是一个引用。

dynamic\_cast主要用于类层次间的上行转换和下行转换，还可以用于类之间的交叉转换。在类层次间进行上行转换时，dynamic\_cast和static\_cast的效果是一样的；在进行下行转换时，dynamic\_cast具有类型检查的功能，比static\_cast更安全。在多态类型之间的转换主要使用dynamic\_cast，因为类型提供了运行时信息。下面我将分别在以下的几种场合下进行dynamic\_cast的使用总结：

*最简单的上行转换*  
比如B继承自A，B转换为A，进行上行转换时，是安全的，如下：

```cpp
#include <iostream>
using namespace std;
class A
{
     // ......
};
class B : public A
{
     // ......
};
int main()
{
     B *pB = new B;
     A *pA = dynamic_cast<A *>(pB); // Safe and will succeed
}
```

*多重继承之间的上行转换*  
C继承自B，B继承自A，这种多重继承的关系；但是，关系很明确，使用dynamic_cast进行转换时，也是很简单的：

```cpp
class A
{
     // ......
};
class B : public A
{
     // ......
};
class C : public B
{
     // ......
};
int main()
{
     C *pC = new C;
     B *pB = dynamic_cast<B *>(pC); // OK
     A *pA = dynamic_cast<A *>(pC); // OK
}
```

而上述的转换，static\_cast和dynamic\_cast具有同样的效果。而这种上行转换，也被称为隐式转换；比如我们在定义变量时经常这么写：B *pB = new C;这和上面是一个道理的，只是多加了一个dynamic\_cast转换符而已。

*转换成void **  
可以将类转换成void *，例如：

```cpp
class A
{
public:
     virtual void f(){}
     // ......
};
class B
{
public:
     virtual void f(){}
     // ......
};
int main()
{
     A *pA = new A;
     B *pB = new B;
     void *pV = dynamic_cast<void *>(pA); // pV points to an object of A
     pV = dynamic_cast<void *>(pB); // pV points to an object of B
}
```

但是，在类A和类B中必须包含虚函数，为什么呢？因为类中存在虚函数，就说明它有想让基类指针或引用指向派生类对象的情况，此时转换才有意义；由于运行时类型检查需要运行时类型信息，而这个信息存储在类的虚函数表中，只有定义了虚函数的类才有虚函数表。


如果expression是type-id的基类，使用dynamic\_cast进行转换时，在运行时就会检查expression是否真正的指向一个type-id类型的对象，如果是，则能进行正确的转换，获得对应的值；否则返回NULL，如果是引用，则在运行时就会抛出异常；例如：

```cpp
class B
{
     virtual void f(){};
};
class D : public B
{
     virtual void f(){};
};
void main()
{
     B* pb = new D;   // unclear but ok
     B* pb2 = new B;
     D* pd = dynamic_cast<D*>(pb);   // ok: pb actually points to a D
     D* pd2 = dynamic_cast<D*>(pb2);   // pb2 points to a B not a D, now pd2 is NULL
}
```

这个就是下行转换，从基类指针转换到派生类指针。
对于一些复杂的继承关系来说，使用dynamic_cast进行转换是存在一些陷阱的；

```cpp
class A
{
     virtual void Func() = 0;
};
class B : public A
{
     void Func(){};
};
class C : public A
{
     void Func(){};
};
class D : public B, public C
{
     void Func(){}
};
int main()
{
     D *pD = new D;
     A *pA = dynamic_cast<A *>(pD); // You will get a pA which is NULL
}
```

如果进行上面的直接转，你将会得到一个NULL的pA指针；这是因为，B和C都继承了A，并且都实现了虚函数Func，导致在进行转换时，无法进行抉择应该向哪个A进行转换。正确的做法是：

```cpp
int main()
{
     D *pD = new D;
     B *pB = dynamic_cast<B *>(pD);
     A *pA = dynamic_cast<A *>(pB);
}
```


## const_cast

```
const_cast的转换格式：const_cast <type-id> (expression)
```

const\_cast用来将类型的const、volatile和__unaligned属性移除。常量指针被转换成非常量指针，并且仍然指向原来的对象；常量引用被转换成非常量引用，并且仍然引用原来的对象。

```cpp
/*
** FileName     : ConstCastDemo
** Author       : Jelly Young
** Date         : 2013/12/27
** Description  : More information, please go to http://www.jellythink.com
*/
#include <iostream>
using namespace std;
class CA
{
public:
     CA():m_iA(10){}
     int m_iA;
};
int main()
{
     const CA *pA = new CA;
     // pA->m_iA = 100; // Error
     CA *pB = const_cast<CA *>(pA);
     pB->m_iA = 100;
     // Now the pA and the pB points to the same object
     cout<<pA->m_iA<<endl;
     cout<<pB->m_iA<<endl;
     const CA &a = *pA;
     // a.m_iA = 200; // Error
     CA &b = const_cast<CA &>(a);
     b.m_iA = 200;
     // Now the a and the b reference to the same object
     cout<<b.m_iA<<endl;
     cout<<a.m_iA<<endl;
}
```

**注：你不能直接对非指针和非引用的变量使用const_cast操作符去直接移除它的const、volatile和__unaligned属性。**


## reinterpret_cast

```
reinterpret_cast的转换格式：reinterpret_cast <type-id> (expression)
```

允许将任何指针类型转换为其它的指针类型；听起来很强大，但是也很不靠谱。它主要用于将一种数据类型从一种类型转换为另一种类型。它可以将一个指针转换成一个整数，也可以将一个整数转换成一个指针，在实际开发中，先把一个指针转换成一个整数，在把该整数转换成原类型的指针，还可以得到原来的指针值；特别是开辟了系统全局的内存空间，需要在多个应用程序之间使用时，需要彼此共享，传递这个内存空间的指针时，就可以将指针转换成整数值，得到以后，再将整数值转换成指针，进行对应的操作。





参考资料：

> http://www.jellythink.com/archives/205

<!-- cast.md ends here -->
