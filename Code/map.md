<!-- map.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 三 8月 10 19:49:29 2016 (+0800)
;; Last-Updated: 三 8月 10 19:51:56 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 2
;; URL: http://wuhongyi.github.io -->

# map

```cpp
#include <map>

map::begin
功能：返回第一个元素的定位器（iterator）的地址。
说明：当返回的第一个元素的地址为一个常值定位器（_iterator),则map不会被修改。当返回的第一个元素的地址值为一个定位器（iterator），则map可被修改。
函数返回值：返回一个指向第一个元素的双向定位器地址。

map：：clear
功能：将一个map容器的全部元素删除。
说明：clear会删除map容器的全部元素。
函数返回值：无。

map：：count
功能：返回对应某个关键字的元素的个数。
说明：_Key是要进行匹配的关键字的值。该函数的返回值的范围是[low_bound(_Key),upper_bound(_Key)]。因为map容器的关键字是唯一的，故它只能取0或者1。
函数返回值：当map容器包含了关键字为_Key的这个元素时，返回1，否则返回0。

map::enpty
功能：测试一个map容器是否为空。
说明：empty函数用于测试一个map容器是否为空。
函数返回值：当容器map为空时，返回ture，否则返回false。

map::end
功能：返回最后一个元素后面的定位（iterator）的地址。
说明：end函数用于测试一个定位器是否已到达它的尾部。
函数返回值：返回一个指向最后一个元素后面的双向定位器地址。当map容器为空时，结果没定义。

map::equal_range
功能：返回一对定位器，它们分别指向第一个大于或等于给定的关键字的元素和第一个比给定的关键字大的元素。
说明：_Key是一个用于排序的关键字。
函数返回值：函数一对定位器。要从第一个定位器中取得数据，可用pr.first。从第二个定位器中取得数据，则用pr.second。

map::erase
功能：将一个或一定范围的元素删除。
说明：_Where表示要删除的元素的位置，_First第一个被删除的元素的位置，_Last第一个不被删除的元素的位置，_Key从map容器中删除的元素的关键字的值。因为没有重新分配空间，故只有对应于被删除元素的iterator和reference失效。注意：它不会抛出任何的exception。
函数返回值：前两个函数返回一个指向第一个没被删除的元素的双向定位器，如果不存在这样的元素，则返回map容器的末尾。第三个函数返回被删除的元素的个数。

map::find
功能：求出与给定的关键字相等的元素的定位器。
说明：_Key是要进行搜索的关键字的值。该函数会返回一个指向关键字为_Key的元素的定位器。当返回值的第一个元素的地址值为一个常值定位器（_iterator），则map可通过它不会被修改。当返回值的第一个元素的地址值为一个定位器（iterator），则map可通过它被修改。
函数返回值：找到该元素时，返回一个指向关键字为_Key的元素的定位器，否则返回一个指向map容器的结束的定位器。

map::get_allocator
功能：返回一个构造该map容器的allocator的一个副本。
说明：容器map的allocator指明一个类的存储管理。默认的allocator能提供STL容器高校的运行。
函数返回值：该容器的allocator。

map::insert
功能：将一个元素或者一定数量的元素插入到map的特定位置中去。
说明：_Where第一个被插入到map的元素的位置，_Val插入的参数的值，_First第一个被插入的位置，_Last第一个不被插入的位置。如果能在_Where后面迅速地插入，那么只要很短的时间。第三个成员函数将范围为[_First,_Last]中的元素插入。
函数返回值：第一个函数返回一对值，当插入成功时，bool=true，当要插入的元素的关键字与已有的参数的值相同，则bool=false，而iterator指向插入的位置或者已存在的元素的位置。第二个函数返回指向插入位置的定位器。

map::key_comp
功能：取得一个比较对象的副本以对map容器中的元素进行排序。
说明：存储对象定义了一个成员函数：bool operator(const Key&_Left,const Key&_Right);。当_Left与_Right在次序比较中不同时，返回true，否则返回false。
函数返回值：取得一个对map容器中的元素进行排序的比较对象。

map::lower_bound
功能：求出指向第一个关键字的值是大于等于一个给定值的元素的定位器。
说明：_Key是一个用于排序的关键字。当返回值为一个const_iterator，则map容器不会被修改。当返回值为一个iterator，则map容器可被修改。
函数返回值：返回一个指向第一个关键字的值是大于等于一个给定值的元素的定位器，或者返回指向map容器的结束的定位器。

map::map
功能：map的构造函数。
说明：_Al一个分配器类。_Comp一个用于比较的函数,它的默认值为hash_compare。_Right一个map的拷贝。_First被拷贝的第一个元素的位置。_Last被拷贝的最后一个元素的位置。所有的构造函数都存储一个分配器和初始化map容器。所有的构造函数都存储一个Traits类型的函数对象，它用于对map容器的元素进行排序。头三个构造函数创建一个空的初始map容器。第四个构造函数创建一个_Right容器的副本。后三个构造函数拷贝在范围_First~_Last内的一个map容器。
函数返回值：无。

map::max_size
功能：计算map容器的最大长度。
说明：max_size会返回map容器的最大长度。
函数返回值：返回map容器可能的最大长度。

map::operator[]
功能：将一个给定的值插入到map容器中去。
说明：_Key是被插入的元素的关键字的值。注意：当map容器中找不到_Key的值时，则插入到map容器中，且值是一个默认值。
函数返回值：返回一个_Key位置的元素的地址（reference）。

map::operator!=
功能：测试该map容器的左边与右边是否相同。
说明：_Left和_Right是待比较的两个map容器。两个map容器相等，当且仅当它们的元素个数相等且同一位置上的值相等。

map::operator<
功能：测试该map容器的左边是否小于右边。
说明：_Left和_Right是待比较的两个map容器。两个map容器的大小比较是基于第一个不相同的元素的大小比较。
函数返回值：当_Left<_Right时，返回True，否则返回False。

map::operator<=
功能：测试左边的map容器是否小于或等于右边。
说明：_Left和_Right是待比较的两个map容器。两个map容器的大小比较是基于第一个不相同的元素的大小比较。
函数返回值：当_Left<=_Right时，返回True，否则返回False。

map::operator==
功能：测试左边的map容器与右边是否相同。
说明：_Left和_Right是待比较的两个map容器。两个map容器相等，当且仅当它们的元素个数相等且同一个位置上的值相等。
函数返回值：当_Left和_Right相同时，返回True，否则返回False。

map::operator>
功能：测试左边的map容器是否大于右边。
说明：_Left和_Right是待比较的两个map容器。两个map容器的大小比较是基于第一个不相同的元素的大小比较。
函数返回值：当_Left>_Right时，返回True，否则返回False。

map::operator>=
功能：测试左边的map容器是否大于或等于右边。
说明：_Left和_Right是待比较的两个map容器。两个map容器的大小比较是基于第一个不相同的元素的大小比较。
函数返回值：当_Left>=_Right时，返回True，否则返回False。

map::rbegin
功能：返回一个指向反向map容器的第一个元素的定位器。
说明：rebgin与反向map容器一起使用，它的作用与map容器中的begin一样。当返回值为一个const_reverse_iterator，则map容器不会被修改。当返回值为一个reverse_iterator，则map容器可被修改。
函数返回值：返回一个指向反向map容器的第一个元素的反向双向定位器。

map::rend
功能：返回一个指向反向map容器的最后元素后面的定位器。
说明：rend与反向map容器一起使用，它的作用与map容器中的end一样。当返回值为一个const_reverse_iterator，则map容器不会被修改。当返回值为一个reverse_iterator，则map容器可被修改。
函数返回值：返回一个指向反向map容器中的最后一个元素后面的反向双向定位器。

map::size
功能：计算map容器的大小。
说明：size函数会计算出map容器的长度。
函数返回值：当前map容器的长度。

map::swap
功能：交换两个map容器的元素。
说明：_Right是与目标容器交换元素的map容器。
函数返回值：无。

map::upper_bound
功能：求出指向第一个关键字的值是大于一个给定值的元素的定位器。
说明：_Key是一个用于排序的关键字。当返回值为一个const_iterator，则map容器不会被修改。当返回值为一个iterator，则map容器可被修改。
函数返回值：返回一个指向第一个关键字的值是大于一个给定值的元素的定位器，或者返回指向map容器的结束的定位器。

map::value_comp
功能：返回一个能确定元素的次序的函数。
说明：对于容器m，当e1（k1，d1)和e2（k2，d2）是它的两个元素，则：m.valve_comp(e1,e2)=m.key_comp(k1,k2)
函数返回值：返回一个能确定元素的次序的函数。
```


<!-- map.md ends here -->
