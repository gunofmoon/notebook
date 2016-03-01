### 重新封装CPP函数
封装CPP的函数，保证兼容C函数风格（模版，重载的函数针对每种类型重新封装）
假设源码文件为foocpp.cpp
编写C和CPP共享的头文件foocpp.h
```c
#ifdef __cplusplus
extern “C”{
#endif
	void foo(int);

#ifdef __cplusplus
}
#endif
```
这是因为

1. 在cpp中，需要声明函数为extern “C”
2. 在c中，关键字extern “C”不支持
3. 所以需要通过编译器的宏__cplusplus判断当前c还是cpp编译器在调用该头文件。

在foo.c中
```c
#include “foo.h”
```
分别编译:
```
clang -c foo.c
clang++ -c foocpp.cpp
```
链接：
```
clang -o foo *.o -lstdc++
```
