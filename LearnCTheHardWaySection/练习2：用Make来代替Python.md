[[GNU]] [[make]]
C语言是一种需要先编译而后才能能运行的程序语言，这个python不同。make是一种能够能用来编译C语言代码的工具。（这个点我是了解的，但基本上完全不会使用make）
# 使用Make
练习1中使用了命令行调用make编译一个C语言可执行程序：
```bash
$ make ex1
# or this one too
$ CFLAGS="-Wall" make ex1
```
首先调用命令行运行make生成一个名为ex1的可执行程序，这里make都干了什么呢？
1. 检查ex1是否已经存在
2. 没有，有没有以ex1开头的文件。检查到有一个叫做ex1.c的文件
3. C语言文件需要调用`cc ex1.c -o ex1`来生成目标文件
4. 等待cc程序完成目标构建
第二条命令行`CFLAGS="-Wall"`则是[[Unix]] [[shell]]，命令行创建一个[[环境变量]]供当前一条命令行使用。
"-Wall"变量会作用到make 和make调用的cc，但当开启吓一条命令行时，这个变量是不会生效的。
```bash
$ make ex1    # -Wall不生效
or this one too
$ CFLAGS="-Wall" make ex1    #-Wall生效
$ make ex1    # -Wall不生效
```
[Makefile] 是make程序读取的配置文件，make每次运行时会首先搜索当前文件夹中的Makefile文件，以按照Makefile的指示编译目标文件。

 首先创建一个原始的Makefile文件 ：
```
CFLAG=-Wall -g

clean:
	rm -f ex1
```
**注意其的缩进，是一个tab，不是4个空格。虽然这个和make的变种有关，但是一个tab是最严格的写法**

# 附加题
- 创建目标`all:ex1`，可以以单个命令`make`构建`ex1`。 --完成
- 阅读`man make`来了解关于如何执行它的更多信息。 --完成
- 阅读`man cc`来了解关于`-Wall`和`-g`行为的更多信息。--完成
- 在互联网上搜索Makefile文件，看看你是否能改进你的文件。
问文心一言给了一个：
```makefile
CC = gcc  
CFLAGS = -Wall -Werror  
  
TARGET = $(shell echo $$ 1)  
  
$(TARGET): $(TARGET).o  
 $(CC) $(CFLAGS) -o $(TARGET) $(TARGET).o  
  
$(TARGET).o: $(TARGET).c  
 $(CC) $(CFLAGS) -c $(TARGET).c  
  
clean:  
 rm -f *.o $(TARGET)
```

- 在另一个C语言项目中找到`Makefile`文件，并且尝试理解它做了什么。
1、我找到了一个使用C语言实现算法的仓库，但是使用的是[CMake]。暂时记下这个仓库[C-Plus-Plus](https://github.com/TheAlgorithms/C-Plus-Plus/tree/master)
2、文心一言给出的示例：
文件结构：
|-- object
|    |-- include
|    |    |-- example1.h
|    |    |-- example2.h
|    |-- src
|    |    |-- example1.c
|    |    |-- example2.c
|    |-- main.c
|    |-- Makefile
```makefile
CC = gcc  
CFLAGS = -I include  
  
all: main  
  
main: src/main.o src/example1.o src/example2.o  
 $(CC) -o main src/main.o src/example1.o src/example2.o  
  
src/main.o: src/main.c include/example1.h include/example2.h  
 $(CC) $(CFLAGS) -c src/main.c -o src/main.o  
  
src/example1.o: src/example1.c include/example1.h  
 $(CC) $(CFLAGS) -c src/example1.c -o src/example1.o  
  
src/example2.o: src/example2.c include/example2.h  
 $(CC) $(CFLAGS) -c src/example2.c -o src/example2.o  
  
clean:  
 rm -f src/*.o main
```
`CC = gcc` 指定编译器为gcc
`CFLAGS = -I include` 使用cc指令`-I`指定头文件查找目录