# gdb
本节会对 C/C++ 常用的 GDB (GNU Project debugger) 进行介绍。

例如，在以下的程序中，运行到 throw 20 时会抛出一个异常
```c++
#include <iostream>
void foo()
{
    throw 20;
}
int main()
{
    std::cout << "testing";
    foo();
    return 0;
}
```
如果你直接编译（g++ example.cpp）并运行，会得到以下输出：

```shell
./a.out
terminate called after throwing an instance of 'int'
[1]    12079 IOT instruction (core dumped)  ./a.out
```

此时只知道程序发生了错误，但并不知道是哪里出了问题。这时候就可以使用 gdb 来进行调试（取代简单的 print）：

在编译时使用 g++ example.cpp -g，完成编译后，运行 gdb a.out，会进入以下界面：
```
<some intro here>
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from a.out...
(gdb)
```

此时输入 run 然后回车，gdb 就会运行你的程序，得到：
```
Starting program: /home/catoverflow/Projects/tmp/a.out
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/usr/lib/libthread_db.so.1".
terminate called after throwing an instance of 'int'
Program received signal SIGABRT, Aborted.
0x00007ffff7ae534c in __pthread_kill_implementation () from /usr/lib/libc.so.6
(gdb)
```

说明程序发生了错误，并回到终端。输入 bt (back trace) 并回车就可以看到程序的调用栈：
```
(gdb) bt
#0  0x00007ffff7ae534c in __pthread_kill_implementation () from /usr/lib/libc.so.6
#1  0x00007ffff7a984b8 in raise () from /usr/lib/libc.so.6
#2  0x00007ffff7a82534 in abort () from /usr/lib/libc.so.6
#3  0x00007ffff7dfc7ee in __gnu_cxx::__verbose_terminate_handler () at /usr/src/debug/gcc/libstdc++-v3/libsupc++/vterminate.cc:95
#4  0x00007ffff7e08c4c in __cxxabiv1::__terminate (handler=<optimized out>) at /usr/src/debug/gcc/libstdc++-v3/libsupc++/eh_terminate.cc:48
#5  0x00007ffff7e08cb9 in std::terminate () at /usr/src/debug/gcc/libstdc++-v3/libsupc++/eh_terminate.cc:58
#6  0x00007ffff7e08f5e in __cxxabiv1::__cxa_throw (obj=<optimized out>, tinfo=0x555555557db0 <typeinfo for int@CXXABI_1.3>, dest=0x0)
    at /usr/src/debug/gcc/libstdc++-v3/libsupc++/eh_throw.cc:95
#7  0x00005555555551a4 in foo () at example.cpp:5
#8  0x00005555555551c6 in main () at example.cpp:10
```

调用栈将函数调用自底向上显示，最上面的就是最后被调用的函数，在这里上面的都是链接的系统库，下面的两个则是我们自己的函数。这时就很容易发现错误发生在 foo 中，代码的第五行。

最后，输入 q 并回车就可以退出了。除了上面的 bt，常用的指令还有 break（添加断点），用 attach 命令连接到线程（在多线程调试中非常有用）等等，具体的用法可以查阅相关文档。