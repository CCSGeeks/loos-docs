# make和Makefile

GNU make(简称make)是一种代码维护工具，在大中型项目中，它将根据程序各个模块的更新情况，自动的维护和生成目标代码。make命令执行时，需要一个 makefile （或Makefile）文件，以告诉make命令需要怎么样的去编译和链接程序。

## 1. make 执行逻辑

执行逻辑

- 首次编译：所有文件均被编译并链接。
- 部分文件更新：仅重新编译更新的文件及其依赖项。
- 头文件更新：重新编译所有依赖该头文件的源码文件。

## 2. makefile 的规则
在讲述这个makefile之前，还是让我们先来粗略地看一看makefile的规则。
```makefile
target ... : prerequisites ...
    command
    ...
    ...
```
target 也就是一个目标文件，可以是object file，也可以是执行文件。还可以是一个标签（label）。prerequisites就是，要生成那个target所需要的文件或是目标。command也就是make需要执行的命令（任意的shell命令）。 这是一个文件的依赖关系，也就是说，target这一个或多个的目标文件依赖于prerequisites中的文件，其生成规则定义在 command中。如果prerequisites中有一个以上的文件比target文件要新，那么command所定义的命令就会被执行。这就是makefile的规则。也就是makefile中最核心的内容。