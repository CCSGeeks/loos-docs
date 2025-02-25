# SHELL 

## Linux常见命令
如ls、cd 、mkdir、torch、rm、mv、clear、grep、find、cat、shutdown等。
请自行练习必要的Linux命令，达到初步熟练掌握的程度（不需要死记硬背，可以利用man命令、网络搜索和大语言模型）

## I/O 重定向与管道
一般情况下命令从标准输入（stdin）读取输入，并输出到标准输出（stdout），默认情况下两者都是你的终端。使用重定向可以让命令从文件读取输入/输出到文件。下面是以 echo 为例的重定向输出：

```shell
$ echo "Hello Linux!" > output_file # 将输出写入到文件（覆盖原有内容）
$ cat output_file
Hello Linux!
$ echo "rewrite it" > output_file
$ cat output_file # 可以看到原来的 Hello Linux! 被覆盖了。
rewrite it
$ echo "append it" >> output_file # 将输出追加到文件（不会覆盖原有内容）
$ cat output_file
rewrite it
append it
```

无论是 > 还是 >>，当输出文件不存在时都会创建该文件。

重定向输入使用符号 <：

```shell
command < inputfile
command < inputfile > outputfile
```
除了 stdin 和 stdout 还有标准错误（stderr），他们的编号分别是 0、1、2。stderr 可以用 2> 重定向（注意数字 2 和 > 之间没有空格）。使用 2>&1 可以将 stderr 合并到 stdout。

## 管道
管道（pipe），操作符 |，作用为将符号左边的命令的 stdout 接到之后的命令的 stdin。管道不会处理 stderr。管道是类 UNIX 操作系统中非常强大的工具。通过管道，我们可以将实现各类小功能的程序拼接起来干大事。

示例如下：
```shell
$ ls / | grep bin  # 筛选 ls / 输出中所有包含 bin 字符串的行
bin
sbin
```

## 文本处理

在进行文本处理时，我们有一些常见的需求：

- 获取文本的行数、字数
- 比较两段文本的不同之处
- 查看文本的开头几行和最后几行

下面介绍如何在 shell 中做到这些事情。

### 文本统计：wc

wc 是文本统计的常用工具，它可以输出文本的行数、单词数与字符（字节）数。
```shell
$ wc file
     427    2768   20131 file
```

### 文本查找：grep

grep 命令可以查找文本中的字符串：
```shell
$ grep 'hello' file  # 查找文件 file 中包含 hello 的行
$ ls | grep 'file'  # 查找当前目录下文件名包含 file 的文件
$ grep -i 'Systemd' file  # 查找文件 file 中包含 Systemd 的行（忽略大小写）
$ grep -R 'hello' .  # 递归查找当前目录下内容包含 hello 的文件
```

### 文本比较：diff

diff 工具用于比较两个文件的不同，并列出差异。
```shell
$ echo hello > file1
$ echo hallo > file2
$ diff file1 file1
$ diff file1 file2
1c1
< hello
---
> hallo
```

## Shell 脚本

### Shell 脚本概述
Shell 脚本是基于文本的命令集合，通过 Shell 解释器（如 Bash）执行，能够自动化完成系统管理、文件操作、任务调度等复杂任务。其特点包括：
- 灵活高效：直接调用系统命令，无需编译；
- 跨平台性：适配类 UNIX 系统（如 Linux、macOS）；
- 开源文化：与 Linux 生态深度集成，支持社区工具链扩展。

---

### Shell 脚本基础
#### 1. 脚本结构
```shell
#!/bin/bash          # Shebang 行，指定解释器
# 注释：打印 Hello World
echo "Hello World"   # 输出命令
```
#### 2. 变量与参数

- 变量定义：无需声明类型，直接赋值（如 name="Alice"）；

- 环境变量：通过 $PATH、$HOME 等访问系统变量；

- 脚本参数：$0 为脚本名，$1、$2 为传递的参数12。

#### 3. 控制结构
```shell
# 条件判断
if [ $num -gt 10 ]; then
    echo "大于 10"
else
    echo "小于等于 10"
fi

# 循环
for file in *.txt; do
    echo "处理文件: $file"
done

# while 循环
while read line; do
    echo "读取行: $line"
done < input.txt
```
### 应用场景示例
#### 1. 自动化任务
```shell
#!/bin/bash
# 批量重命名文件
for file in *.log; do
    mv "$file" "${file%.log}.bak"
done
```

#### 2. 系统管理

修改软件源（使用 sed 替换文本）：
```shell
sudo sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list  
```