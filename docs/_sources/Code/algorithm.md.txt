<!-- algorithm.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 三 8月 10 19:52:44 2016 (+0800)
;; Last-Updated: 三 8月 10 19:53:35 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 1
;; URL: http://wuhongyi.cn -->

# algorithm

```cpp
#include <algorithm>

adjacent_find 查找相邻的两个相同(或者有其他关联)元素
all_of 如果对于所有元素的谓词测试都为true,则返回true （C++11）
any_of 如果对于任意元素的谓词测试都为true,则返回true （C++11）
binary_search 确定容器中是否存在某个元素
copy 拷贝元素到新的位置
copy_backward 逆序拷贝元素
copy_if （C++11）
copy_n （C++11）
count 返回匹配给定值的元素数目
count_if 返回符合条件的元素数目 （C++11）
equal 确定两个集合中的所有元素皆相同
equal_range 搜索序列中的由相同元素组成的子序列
fill 为一个序列赋值
fill_n 为序列中给定数目的元素赋值
find 在序列中查找一个匹配值的元素
find_end 在序列中查找最后出现的序列
find_first_of 在序列中查找给定集合的任一元素
find_if 在序列中查找第一个符合条件的元素
find_if_not （C++11）
for_each 为序列中的每个元素应用指定的函数
generate 将函数的运行结果储存在一个序列中
generate_n 将N次驱动函数的结果储存在一个序列中
includes 检查一个集合是否是另外一个集合的子集
inplace_merge 内置式归并
is_heap 检查给定的序列是否是堆 （C++11）
is_heap_until （C++11）
is_partitioned （C++11）
is_permutation （C++11）
is_sorted （C++11）
is_sorted_until （C++11）
iter_swap 交换两个迭代器指向的元素
lexicographical_compare 按字典顺序检查一个序列是否小于另外一个序列
lower_bound 查找第一个插入元素但不影响序列有序性的位置
make_heap 创建一个堆并以序列的形式输出
max 返回两个元素间的较大者 
max_element 返回序列中的最大者
merge 对两个有序序列进行归并处理
min 返回两个元素间的较小者 
minmax （C++11）
minmax_element （C++11）
min_element
mismatch 查找两个序列的第一个不相同的位置
move （C++11）
move_backward （C++11）
next_permutation 依照字典顺序生成序列的下一个稍大的排列
none_of 如果对于所有元素的谓词测试都为false,则返回true （C++11）
nth_element 插入一个元素至它的排序位置并确保它左边的元素都不大于它右边的元素
partial_sort 将序列中的前N个元素排序
partial_sort_copy 拷贝并部分排序
partition 将元素序列分成两组
partition_copy （C++11）
partition_point （C++11）
pop_heap 从一个堆中移除最大的元素
prev_permutation 依照字典顺序生成序列的下一个稍小的排列
push_heap 添加一个元素至堆
random_shuffle 随机生成元素的一个排列
remove 移除给定值的所有元素
remove_copy 拷贝一个序列中元素的同时忽略那些匹配给定值的元素
remove_copy_if 拷贝一个序列中元素的同时忽略那些符合条件的元素
remove_if 移除序列中所有符合条件的元素
replace 将序列中的一些元素以另外一个值替换
replace_copy 拷贝一个序列并将其中一些替换为新值
replace_copy_if 拷贝一个序列的元素并替换掉那些符合条件的元素
replace_if 替换掉符合条件的元素
reverse 将给定序列反转顺序
reverse_copy 以逆序拷贝元素的方式创建序列的副本
rotate 调换一些元素到序列的左边
rotate_copy 拷贝并调换元素
search 搜索子序列
search_n 搜索N个连续的元素拷贝
set_difference 计算两个集合的差集
set_intersection 计算两个集合的并集
set_symmetric_difference 计算两个集合的对称差
set_union 计算两个集合的交集
shuffle （C++11）
sort 将序列按升序排序
sort_heap 将堆转变成有序序列
stable_partition 将元素划分成两组且维持原来的元素次序
stable_sort 将序列排序并且维持相等元素的原始次序
swap 交换两个对象的值
swap_ranges 交换两个序列的元素
transform 通过给定函数转换序列元素
unique 移除连续的重复元素
unique_copy 拷贝序列并忽略相同元素以创建一个无重复元素的集合
upper_bound 搜索最后一个插入元素并能维持序列有序性的位置(第一个稍大于给定值的位置)
```

<!-- algorithm.md ends here -->
