<!-- sys.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 六 5月 18 15:42:15 2019 (+0800)
;; Last-Updated: 六 5月 18 15:51:40 2019 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 1
;; URL: http://wuhongyi.cn -->

# sys


## shm_open函数实例及说明

```cpp
int shm_open(const char *name, int oflag, mode_t mode);

// shm_open的第二个参数oflag，可被设置为：O_RDONLY， O_RDWR， O_CREAT， O_EXCL， O_TRUNC
// 注意其属性设置是针对所有将会访问到它的进程而言的，而不仅仅是针对开辟它的这一进程的。
// 例如，一块共享内存，进程A创建它，只有读它的需求，进程B访问它，有写它的需求。那么，进程A创建它的时候，就应该把oflag设置为O_RDWR，而不是O_RDONLY
```



**使用shm_open来操作共享内存**

shm_open最主要的操作也是默认的操作就是在/dev/shm/下面，建立一个文件。

文件名字是用户自己输入的。要点一定要用ftruncate把文件大小于设置为共享内存大小。

服务端：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/mman.h>
#include <sys/types.h>
#include <fcntl.h>
#include <sys/stat.h>
char buf[10];
char *ptr;
int main()
{
int fd;
fd = shm_open("region", O_CREAT | O_RDWR, S_IRUSR | S_IWUSR);
if (fd<0) {
printf("error open region\n");
return 0;
}
ftruncate(fd, 10);
ptr = mmap(NULL, 10, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
if (ptr == MAP_FAILED) {
printf("error map\n");
return 0;
}
*ptr = 0x12;
return 0;
}
```

客户端：

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/mman.h>
#include <sys/types.h>
#include <fcntl.h>
#include <sys/stat.h>
char buf[10];
char *ptr;
int main()
{
int fd;
fd = shm_open("region", O_CREAT | O_RDWR, S_IRUSR | S_IWUSR);
if (fd<0) {
printf("error open region\n");
return 0;
}
ftruncate(fd, 10);
ptr = mmap(NULL, 10, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
if (ptr == MAP_FAILED) {
printf("error map\n");
return 0;
}
while (*ptr != 0x12);
printf("ptr : %d\n", *ptr);
return 0;
}
```


----

> https://blog.csdn.net/boygrass/article/details/8884502   
> https://blog.csdn.net/huntinux/article/details/51382679  
> https://blog.csdn.net/sessos/article/details/78451067  

<!-- sys.md ends here -->
