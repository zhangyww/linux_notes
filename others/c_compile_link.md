# C编译链接(gcc)

## 编译,静态库(.a),动态库(.so)
现在假设有如下几个文件main.c, add.h, add.c, sub.h, sub.c
```C
//main.c
#include "add.h"
#include "sub.h"

#include <stdio.h>

int main(void){
        setNumber(10);
        printf("add=%d\n", add(12));
        setSubNumber(20);
        printf("sub=%d\n", sub(8));

        return 0;
}


//add.h
void setNumber(int n);
int add(int n2);


//add.c
#include "add.h"

static int gNumber;

void setNumber(int n){
        gNumber = n;
}

int add(int n2){
        return gNumber + n2;
}


//sub.h
void setSubNumber(int n);
int sub(int n2);


//sub.c
#include "sub.h"

static int gNumber;

void setSubNumber(int n){
        gNumber = n;
}

int sub(int n2){
        return gNumber - n2;
}
```

直接编译,链接成可执行文件
```shell
gcc main.c add.c sub.c -o app
# -o 选项为指定输出可执行文件的名字为app 
```

编译成静态库

生成add和sub两个的静态库 libaddsub.o，再链接生成可执行文件
```shell
# 编译add.c文件, -c选项为只编译,不链接
# 生成对象文件 add.o
gcc -c add.c -o add.o

# 编译sub.c文件, -c选项为只编译,不链接
# 生成对象文件 sub.o
gcc -c sub.c -o sub.o

# 将两个对象文件add.o和sub.o合成静态库文件libaddsub.a
# 注意静态库文件必须以lib开头, .a为扩展名
ar rcs libaddsub.a 
#   ar用于创建,修改和提取静态库文件(archives,对象文件的集合)
#     r 是指插入对象文件,如果有同名对象文件已经存在则替换
#     c 是指创建archives文件
#     s 是指archives文件中创建对象文件的索引，或者更新对象文件的索引

# 编译main.c文件, -c选项为只编译,不链接
# 生成对象文件 main.o
gcc -c main.c -o main.o

# 链接main.o和libaddsub.a,生成可执行文件app
gcc main.o -L. -laddsub -o app
#   -L 指在目录.(当前目录)寻找链接库
#   -l 指要链接的库文件会自动加上lib前缀和.a扩展名搜索
#   -o 指定生成的文件名
```

编译成动态链接库

```shell
# 编译add.c文件, -c选项为只编译,不链接
# 生成对象文件 add.o
gcc -c -fPIC add.c -o add.o
# -f 指定编译标识。这里指定了标识PIC(position independent code)
#    位置无关代码。生成动态链接库的对象文件必须是位置无关的。


# 编译sub.c文件, -c选项为只编译,不链接
# 生成对象文件 sub.o
gcc -c -fPIC sub.c -o sub.o
# -f 指定编译标识。这里指定了标识PIC(position independent code)
#    位置无关代码。生成动态链接库的对象文件必须是位置无关的。


# 生成动态链接库libaddsub.so
gcc -shared add.o sub.o -o libaddsub.so
# -shared 指定生成对象链接库(.so)


# 编译main.c为main.o
gcc -c main.c -o main.o

# 链接动态链接库，生成可执行文件app
gcc main.o -L. -laddsub -o app
```

链接动态链接库生成的可执行程序，在运行的时候需要在指定路径找到动态链接库，
并进行加载。
**ld.so和ld-linux.so\*** 负责找到并加载动态链接库，其对动态链接库的搜索顺序为
1. 可执行文件中DT_RPATH指定的搜索目录。在ELF格式，DT_RPATH和DT_RUNPATH用于指定搜索目录。
2. 使用环境变量LD_LIBRARY_PATH指定的搜索目录，以冒号(:)分隔多个目录。
   如果可执行文件设置了set-user-id或set-group-id，那么这个环境变量会被忽略。
3. 可执行文件中DT_RUNPATH执行的搜索目录。
4. 缓存文件/etc/ld.so.cache中缓存的动态链接库列表。如果链接时指定了 -z nodeflib，
   那么跳过这一步。
5. 搜索默认路径/lib和/usr/lib。如果链接时指定了 -z nodeflib，那么跳过这一步。
