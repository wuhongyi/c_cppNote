<!-- sstream.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 五 11月 25 18:43:02 2016 (+0800)
;; Last-Updated: 五 11月 25 18:43:32 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 1
;; URL: http://wuhongyi.cn -->

# sstream

## string型转为double型

```cpp
#include<vector>
#include<cstring>
#include<sstream>
#include <iostream>
using namespace std;

int main( ) {
    string real = "12.32 12 35 25.3 36.366";
    stringstream ss( real );
    vector< double > vd;

    // Collect all real numbers.
    double temp;
    while( ss >> temp )
      { vd.push_back( temp );
    	cout<<temp<<endl;}
    // Create the array.
    double *dbl_array = new double[ vd.size( ) ];
    for( int i = 0; i < vd.size( ); ++i )
        dbl_array[ i ] = vd[ i ];
  string a="$12.m";
  cout<<a[0]<<" "<<a[1]<< " "<<a[2]<<" "<<a[3]<<endl;
}
```



<!-- sstream.md ends here -->
