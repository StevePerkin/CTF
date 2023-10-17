# Computer System: A Programmer's Perspective

---

CMU15-213 AKA. CMU zip code

## Chapter 1  计算机系统漫游

* 课程主题Course theme
* 五大现实Five realities
* 该课程如何融入 CS/ECE 课程How the course fits into the CS/ECE curriculum
* 学术诚信Academic integrity

### 课程主题 Course Theme

---

Abstraction is Good But Don't Forget Reality

## Chapter 2 信息的表示和处理

### 数值系统

**原码：**

**反码：**

**补码：**

浮点数：

```c++
# inital
int x = foo();
int y = bar();
unsigned ux = x;
unsigned uy = y;

x < 0	=>	((x*2)<0)
ux > -1
x > y	=>	-x < -y
x * x >= 0

```

```c++
define flip2(a)
	(((a) & 0xFF) << 8 + ((a) & 0xFF00) >> 8)
	
define flip2(a)
	((a) << 8 | ((a) >> 8) & 0xFF)
```

```c++
typedef unsigned char *pointer;

void show_bytes(pointer start, size_t len){
    size_t i;
    for (i = 0; i < len; i++)
    printf("%p\t0x%.2x\n",start+i, start[i]);
    printf("\n");
}

int main(){
    int a = 0x01234567;
    show_bytes((pointer) &a, sizeof(int));
}
```



当进行0.1 + 0.2这样的浮点数运算时，由于0.1和0.2无法在二进制中精确表示，计算结果会受到舍入误差的影响。这是因为0.1和0.2在二进制表示中都是无限循环小数。



![image-20230708140020874](C:\Users\ChenYifan\Desktop\CSAPP_learningnotes\image-20230708140020874.png)

```c++
#include <assert.h>

int main(){
    assert(+0. == -0.);      	// 断言成功
    assert(1.0/+0. == 1.0/-0.); // 断言失败
    return 0;
}
```

带符号数产生意外结果的例子。这个例子会造成无限循环，因为sizeof会返回unsigned int 类型，由此带来的结果是，i - sizeof(char)这个表达式的求值结果将会是 unsigned int (隐式转换 !!)，无符号数 0 减 1 会变成 0xFFFFFFFF，从而产生无限循环，有时候你需要特别留心这种不经意的错误 !

```c++
int n = 10, i; 
for (i = n - 1 ; i - sizeof(char) >= 0; i--)
    printf("i: 0x%x\n",i);

if (-1 > 0U)
    printf("You Surprised me!\n"); 
```

二分法

```c++
int binary_search(int a[], int len, int key){
    int low = 0; 
    int high = len - 1; 

    while ( low <= high ) {
        int mid = (low + high)/2;   // 提示：这里有溢出Bug！
        if (a[mid] == key) {
            return mid;
        }
        if (key < a[mid]) {
            high = mid - 1;
        }else{
            low = mid + 1;
        }
    }
    return -1;
}
```

```c++
int binary_search(int a[], int len, int key){
    int low = 0; 
    int high = len - 1; 

    while ( low <= high ) {
        int mid = low + (high - low) / 2;   // 在原始范围内不溢出
        if (a[mid] == key) {
            return mid;
        }
        if (key < a[mid]) {
            high = mid - 1;
        }else{
            low = mid + 1;
        }
    }
    return -1;
}
```

当 `low` 和 `high` 的值很大时，它们的和可能会超过整型的最大表示范围，从而导致溢出。溢出后的结果会被截断为一个错误的值，从而导致 mid 的计算错误。

通过修改为 `low + (high - low) / 2` 的方式，我们先计算 `high` 和 `low` 之间的差值，再除以 2 并加上 `low`，这样可以避免溢出。因为差值不会超过原始范围，所以不会引发溢出问题。

```c++
// 初学者版本
void remove_list_node(List *list, Node *target)
{
    Node *prev = NULL;
    Node *cur = list->head;
    // Walk the list
    while (cur != target) {
        prev = cur;
        cur = cur->next;
    }
    // Remove the target by updating the head or the previous node.
    if (!prev)
        list->head = target->next;
    else
        prev->next = target->next;
}
```

```c++
// 有品位的版本，消除特例，简单优雅的代码
void remove_list_node(List *list, Node *target)
{
    // The "indirect" pointer points to the *address*
    // of the thing we'll update.
    Node **indirect = &list->head;
    // Walk the list, looking for the thing that 
    // points to the node we want to remove.
    while (*indirect != target)
        indirect = &(*indirect)->next;
    *indirect = target->next;
}
```

通过 `indirect` 指针追踪前一个节点的 `next` 指针的地址。通过这种方式，我们可以在不使用特例情况（如头节点的处理）的情况下，简洁地删除链表中的目标节点

## Chapter 3 程序的机器级表示

**基本概念：**

* **Instructure Set Architecture**：指令集架构 (包括指令规格，寄存器等)，简称ISA，它是软硬件之间的“合同”
* **Mircoarchitecture**：指令集架构的具体实现方式 (比如流水线级数，缓存大小等)，它是可变的
* **Machine Code**：机器码，也就是机器可以直接执行的二进制指令
* **Assembly Code**：汇编码，也就是机器码的文本形式 (主要是给人类阅读)

GCC：GNU Compiler Collection

ARM：Acorn RISC Machine

RISC：reduced instruction set computer

CISC：complex instruction set computer

Acorn：a company in British Cambridge which designed and created ARM







## Chapter 10 系统级I/O

**概念**

输入输出是在主存和外部设备之间复制数据的过程：

1. 输入从IO设备复制数据到主存
2. 输出从主存复制数据到IO设备

在Linux系统中，C/C++等高级语言的I/O 函数都是通过内核提供的系统及 Unix I/O 实现的

**UNIX I/O**

所有的IO设备都被模型化为文件，网络、磁盘，所有的输入和输出都被当做对相应文件的读和写来执行

**文件**

Linux文件类型：

1. 普通文件：文本文件和二进制文件，对Linux内核而言没有区别
2. 目录：包含一组链接的文件，其中每一个链接都是一个文件名，每个目录至少含有两个条目：`.`表示到目录自身的链接，`...` 表示到目录其父目录的链接
3. 套接字：与另一个进程进行网络通信的文件
4. 其他文件：命令通道、符号链接，以及字符和块设备

Linux 将所有的文件组织成一个目录层次结构，由根目录 (/) 确定

![linux系统目录层次](https://img-blog.csdnimg.cn/img_convert/24915cbcc1f3f3fbef6758681afecb07.png)



**打开和关闭文件**

`open`函数返回一个文件描述符，返回的描述符总是在进程中当前没有打开的最小描述符

`int open(char *filename, int flags, mode_t mode);`

`flag` 参数指明进程如何访问这个文件：

1. O_RDONLY：只读
2. O_WRONLY：只写
3. O_RDWR：可读可写
4. O_CREAT：如果文件不存在，就创建它的一个截断的（空）文件
5. O_TRUNC：如果文件已存在，就截断它
6. O_APPEND：在写操作前，设置文件位置到文件的结尾处。

进程通过 `close` 函数关闭一个打开的文件，关闭一个已关闭的描述符会出错

`int close(int fd); `



**读和写文件**

```c
ssize_t read(int fd, void *buf, size_t n); 
ssize_t write(int fd, const void *buf, size_t n);
```

read 函数从描述符为 fd 的当前文件位置复制最多 n 个字节到内存位置 buf。

write 函数从内存位置 buf 复制最多 n 个字节到描述符 fd 的当前文件位置。

ssize_t和 size_t 的区别：

1. size_t 是 unsigned long
2. ssize_t 是 long



**读取目录和内容**

`opendir`以路径名为参数，返回指向目录流（即目录项的列表）的指针。

`readdir`返回指向流 dirp 中下一个目录项的指针。若没有更多的目录项或出错，返回 NULL。其中如果出错 readdir 还会设置 errno。

可以通过检查 errno 是否被修改过来区分是出错还是没有更多的目录项

`closedir`关闭流并释放所有的资源。

```c
#include<sys/types.h>
#include<dirent.h>
DIR *opendir(const char *name);
struct dirent *readdir(DIR *dirp);
int closedir(DIR *dirp);
```





**文件描述符**

`> /dev/null 2>&1`

就比如上面这种用法，`>`表示将输出结果重定向到`/dev/null`，`/dev/null`表示空文件设备，就是将打印信息丢弃，屏幕上什么都不显示

1:表示stdout标准输出

2:表示stderr标准错误

&:表示等同于的意思

`2>&1`表示2的输出重定向等同于1，也就是标准错误输出重定向到标准输出

文件描述符（file descriptor）就是内核为了高效管理这些已经被打开的文件所创建的索引，用于指代被打开的文件

所有执行I/O操作的系统调用都通过文件描述符来实现。同时还规定系统刚刚启动的时候，0是标准输入，1是标准输出，2是标准错误，打开一个新的文件，它的文件描述符会是3，再打开一个文件文件描述符就是4

PS：Linux打开文件原理是这样的，但是实际上vim这种编辑器打开文件的文件描述符是4，其原理是先打开源文件并拷贝，然后关闭源文件再打开自己的副本，修改完文件保存的时候直接将副本重命名覆盖源文件。所以打开源文件的时候用的文件描述符3，然后打开自己的副本是时候就该用文件描述符4了，然后关闭源文件，文件描述符3就被释放了，我们查看的时候就只剩下了4，这里4指向的是vim创建的副本文件。



c语言里面的文件描述符的使用

```c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
 
int main(int argc, char* argv[]) {
        int fd = open("test.py", O_RDONLY);
        if (fd == -1) {
                return -1;
        }
        printf("test.py fd = %d \n", fd);
        close(fd);
        return 0;
}
```

`fd = 3`





Python中文件描述符的使用

```python
import sys
 
print('stdin fd = ', sys.stdin.fileno())
print('stdout fd = ', sys.stdout.fileno())
print('stderr fd = ', sys.stderr.fileno())
 
with open("test.py", "w") as f:
    print('test.py fd = ', f.fileno())
```

```
stdin fd = 0
stdout fd = 1
stderr fd = 2
test.py fd = 3
```



Linux配置系统最大打开文件描述符个数

(1)系统级限制

理论上系统内存有多少就可以打开多少的文件描述符，但是在实际中内核是会做相应的处理，一般最大打开文件数会是系统内存的10%（以KB来计算）

(2)用户级限制

同时为了控制每个进程消耗的文件资源，内核也会对单个进程最大打开文件数做默认限制，即用户级限制。32位系统默认值一般是1024，64位系统默认值一般是65535，可以使用 `ulimit -n` 命令查看。



**标准IO**

C语言定义了一个标准IO库，是UnixIO的高位替代，包括（但不止以下这些）：

1. `fopen` 和 `fclose` ：打开和关闭文件
2. `fread`和 `fwrite` ：读字节和写字节
3. `fgets` 和 `fputs` ：读字符串和写字符串
4. `scanf` 和 `printf` ：复杂的格式化的 I/O 函数

这里的流就和计算机组成原理里面讲的差不多了

[Build Your Own Text Editor (viewsourcecode.org)](https://viewsourcecode.org/snaptoken/kilo/)

kilo编辑器教程





![image-20230723150218973](C:\Users\ChenYifan\Desktop\CSAPP_learningnotes\image-20230723150218973.png)

![image-20230723152531432](C:\Users\ChenYifan\Desktop\CSAPP_learningnotes\image-20230723152531432.png)

![image-20230723152538855](C:\Users\ChenYifan\Desktop\CSAPP_learningnotes\image-20230723152538855.png)