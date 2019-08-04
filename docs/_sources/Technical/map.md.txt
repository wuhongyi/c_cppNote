<!-- map.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 五 11月 25 18:21:14 2016 (+0800)
;; Last-Updated: 五 11月 25 18:27:26 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 1
;; URL: http://wuhongyi.cn -->

# map

map是一类关联式容器。它的特点是增加和删除节点对迭代器的影响很小，除了那个操作节点，对其他的节点都没有什么影响。对于迭代器来说，可以修改实值，而不能修改key。
自动建立Key － value的对应。key 和 value可以是任意你需要的类型。  
根据key值快速查找记录，查找的复杂度基本是Log(N)，如果有1000个记录，最多查找10次，1,000,000个记录，最多查找20次。  
快速插入Key - Value 记录。  
快速删除记录。  
根据Key 修改value记录。  
遍历所有记录。  

## map根据second进行排序

```cpp
#include<map>
#include <vector>
#include <algorithm>
#include<iostream>
using namespace std;

typedef pair<int, double> PAIR;

struct CmpByValue {
  bool operator()(const PAIR& lhs, const PAIR& rhs) {
    return lhs.second < rhs.second;
  }
};

//输出重载
ostream& operator<<(ostream& out, const PAIR& p) {
  return out << p.first << "\t" << p.second;
}

int main(int argc, char *argv[])
{
  map<int, double> aaa;
  aaa[0] = 90.1;
  aaa[1] = 79.9;
  aaa[2] = 92.69;
  aaa.insert(make_pair(3,99.12));
  aaa.insert(make_pair(4,86.09));
  //把map中元素转存到vector中 
  vector<PAIR> bbb(aaa.begin(), aaa.end());
  sort(bbb.begin(), bbb.end(), CmpByValue());
  //输出
  for (int i = 0; i != bbb.size(); ++i) {
    cout << bbb[i]<< endl;
  }
  return 0;
}
```

## map查找first所对应的second

```cpp
#include <map>
#include <cstring>
#include <iostream>

  map<string,double> radius;
  map <string,double>::iterator cp;
  
  radius.insert(pair<string,double> (".OH+.OH",0.1416));
  radius.insert(pair<string,double> (".OH+eaq-",0.4525));
  radius.insert(pair<string,double> (".OH+H.",0.2697));
  radius.insert(pair<string,double> (".OH+H2",0.00076));
  radius.insert(pair<string,double> (".OH+H2O2",0.00061));
  radius.insert(pair<string,double> ("eaq-+eaq-",0.0807));
  radius.insert(pair<string,double> ("eaq-+H.",0.2873));
  radius.insert(pair<string,double> ("eaq-+H3O+",0.1664));
  radius.insert(pair<string,double> ("eaq-+O",0.3804));
  radius.insert(pair<string,double> ("H.+H.",0.0944));
  radius.insert(pair<string,double> ("H.+H2O2",0.00144));
  radius.insert(pair<string,double> ("H.+O",0.2904));

  cp=radius.find(".OH+H.");
  cout<<cp->second<<endl;
```

## 在map中插入元素

改变map中的条目非常简单，因为map类已经对[]操作符进行了重载

```cpp
enumMap[1] = "One";
enumMap[2] = "Two";
.....
```

这样非常直观，但存在一个性能的问题。插入2时,先在enumMap中查找主键为2的项，没发现，然后将一个新的对象插入enumMap，键是2，值是一个空字符串，插入完成后，将字符串赋为"Two"; 该方法会将每个值都赋为缺省值，然后再赋为显示的值，如果元素是类对象，则开销比较大。我们可以用以下方法来避免开销：
enumMap.insert(map<int, string> :: value_type(2, "Two"))


## 查找并获取map中的元素

下标操作符给出了获得一个值的最简单方法：
string tmp = enumMap[2];

但是,只有当map中有这个键的实例时才对，否则会自动插入一个实例，值为初始化值。

我们可以使用find()和count()方法来发现一个键是否存在。

查找map中是否包含某个关键字条目用find()方法，传入的参数是要查找的key，在这里需要提到的是begin()和end()两个成员，分别代表map对象中第一个条目和最后一个条目，这两个数据的类型是iterator.

```cpp
int nFindKey = 2; //要查找的Key
```

//定义一个条目变量(实际是指针)

```cpp
UDT_MAP_INT_CSTRING::iterator it= enumMap.find(nFindKey);
if(it == enumMap.end()) {
//没找到
}
else {
//找到
}
```

通过map对象的方法获取的iterator数据类型是一个std::pair对象，包括两个数据 iterator->first 和 iterator->second 分别代表关键字和存储的数据


## map的插入，查找，遍历及删除的例子

```cpp
#include <map>
#include <string>
#include <iostream>
using namespace std;

void map_insert(map < string, string > *mapStudent, string index, string x)
{
mapStudent->insert(map < string, string >::value_type(index, x));
}

int main(int argc, char **argv)
{
char tmp[32] = "";
map < string, string > mapS;

//insert element
map_insert(&mapS, "192.168.0.128", "xiong");
map_insert(&mapS, "192.168.200.3", "feng");
map_insert(&mapS, "192.168.200.33", "xiongfeng");

map < string, string >::iterator iter;

cout << "We Have Third Element:" << endl;
cout << "-----------------------------" << endl;

//find element
iter = mapS.find("192.168.0.33");
if (iter != mapS.end()) {
cout << "find the elememt" << endl;
cout << "It is:" << iter->second << endl;
} else {
cout << "not find the element" << endl;
}

//see element
for (iter = mapS.begin(); iter != mapS.end(); iter ) {

cout << "| " << iter->first << " | " << iter->
second << " |" << endl;

}
cout << "-----------------------------" << endl;

map_insert(&mapS, "192.168.30.23", "xf");

cout << "After We Insert One Element:" << endl;
cout << "-----------------------------" << endl;
for (iter = mapS.begin(); iter != mapS.end(); iter ) {

cout << "| " << iter->first << " | " << iter->
second << " |" << endl;
}

cout << "-----------------------------" << endl;

//delete element
iter = mapS.find("192.168.200.33");
if (iter != mapS.end()) {
cout << "find the element:" << iter->first << endl;
cout << "delete element:" << iter->first << endl;
cout << "=================================" << endl;
mapS.erase(iter);
} else {
cout << "not find the element" << endl;
}
for (iter = mapS.begin(); iter != mapS.end(); iter ) {

cout << "| " << iter->first << " | " << iter->
second << " |" << endl;

}
cout << "=================================" << endl;

return 0;
}
```



<!-- map.md ends here -->
