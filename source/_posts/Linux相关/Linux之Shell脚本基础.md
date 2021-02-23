---
title: Linux之Shell脚本基础
categories: 
  - Linux相关
tags:
  - Shell脚本
about: 无
date: 2021-02-23 13:35:04
---

# Linux之Shell脚本基础

<!--more-->

## 简介

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

Shell脚本（Shell script），是一种为shell编写的脚本程序 ，是一种解释器语言，与JavaScript、python、php类似。

## Shell脚本编程

### 创建脚本

```shell
vi script.sh

# 内容
echo "My first script ..."

# 运行脚本
chmod a+x script.sh
./script.sh
```

### 基础

```shell
# 注释：使用#开头

# 变量使用
VAR="test"
echo "$VAR"
echo "${VAR}"

# 可以把命令执行后的输入结果赋值给一个变量
LIST=$(ls)

```

### 测试

测试主要用于条件判断。`[ condition-to-test-for ]` ，如`[ -e /etc/passwd ]`，注意的是`[]`**前后必须有空格**，如`[-e /etc/passwd]`是错误的写法。

```shell
# 文件测试
-d FILE_NAM  # True if FILE_NAM is a directory 是否目录
-e FILE_NAM  # True if FILE_NAM exists 文件是否存在
-f FILE_NAM  # True if FILE_NAM exists and is a regular file 文件是否存在且是普通文件
-r FILE_NAM  # True if FILE_NAM is readable 文件是否可读
-s FILE_NAM  # True if FILE_NAM exists and is not empty 文件是否存在且不为空
-w FILE_NAM  # True if FILE_NAM has write permission 文件是否有可写权限
-x FILE_NAM  # True if FILE_NAM is executable 文件是否可执行

# 字符串测试
-z STRING  # True if STRING is empty
-n STRING  # True if STRING is not empty
STRING1 = STRIN2 # True if strings are equal
STRING1 != STRIN2 # True if strings are not equal

# 算术测试
var1 -eq var2  # True if var1 is equal to var2 等于
var1 -ne var2  # True if var1 not equal to var2 不等于
var1 -lt var2  # True if var1 is less than var2 小于
var1 -le var2  # True if var1 is less than or equal to var2 小于等于
var1 -gt var2  # True if var1 is greater than var2 大于
var1 -ge var2  # True if var1 is greater than or equal to var2 大于等于
```

### 条件判断

注意：判断语句中[]两边仅左右空格，且中间语句不要有空格。

#### if-elif-else语句

```shell
if [ condition-is-true ]
then
  command 1
elif [ condition-is-true ]
then
  command 2
elif [ condition-is-true ]
then
  command 3
else
  command 4
fi

# 示例
if [ $1="one" ]
then
        echo "one"
elif [ $1="two" ]
then
        echo "two"
else
        echo "none"
fi

```

#### case语句

```shell
case "$VAR" in
  pattern_1)
    # commands when $VAR matches pattern 1
    ;;
  pattern_2)
    # commands when $VAR matches pattern 2
    ;;
  *)
    # This will run if $VAR doesnt match any of the given patterns
    ;;
esac

# 示例
case "$1" in
        [yY] | [yY][eE][sS])
                echo "Yes"
                ;;
        [nN] | [nN][oO])
                echo "No"
                ;;
        *)
                echo "none"
                ;;
esac
```

#### for循环

((表达;条件;末尾循环))，注意是两个括号，否则会报错。

```shell
for VARIABLE_NAME in ITEM_1 ITEM_N
do
  command 1
  command 2
    ...
    ...
  command N
done

for (( VAR=1;VAR<N;VAR++ ))
do
  command 1
  command 2
    ...
    ...
  command N
done

# 示例一
COLORS="red green blue"
for COLOR in $COLORS
do
        echo "The color is: ${COLOR}"
done

#示例二
for (( VAR=0;VAR<3;VAR++ ))
do
        echo "VAR is : ${VAR}"
done
```

#### while循环

```shell
while [ CONNDITION_IS_TRUE ]
do
  # Commands will change he entry condition
  command 1
  command 2
    ...
    ...
  command N
done

# 示例
VAR_TWO=0
while [ $VAR_TWO -le 3 ]
do
        echo "VAR_TWO is : ${VAR_TWO}"
        ((VAR_TWO++))
done
```

#### 参数传递

当我们运行脚本的时候，可以传递参数供脚本内部使用`$ ./script.sh param1 param2 param3 param4`这些参数将被存储在特殊的变量中

```shell
$0 -- "script.sh"
$1 -- "param1"
$2 -- "param2"
$3 -- "param3"
$4 -- "param4"
$# -- 代表参数个数
$@ -- array of all positional parameters "param1""param2""param3""param4"
$* -- 代表"param1 param2 param3 param4"，整体中间以空格分开
```

#### 退出码

可以自定义

```shell
exit 0
exit 1
exit 2
  ...
  ...
exit 255
```

### 函数

可以把一些列的命令或语句定义在一个函数内，从程序的其他地方调用。

```shell
function function_name() {
    command 1
    command 2
    command 3
      ...
      ...
    command N
}

# 示例
function myFunc () {
        echo "My function ..."
        if [ $# -eq 1 ]
        then
                echo "My function one"
                return $[$#]
        elif [ $# -eq 2 ]
        then
                echo "My function two"
                return $[$#]
        else
                echo "My function none"
                return $[$#]
        fi
}
myFunc
echo "one is : $?"
myFunc one
echo "two is : $?"
myFunc one two
echo "three is : $?"
```

