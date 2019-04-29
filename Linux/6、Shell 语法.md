### 变量
> bash 默认把所有变量类型都视为字符串

> =号的周围不能有空格，这可能和你熟悉的大部分编程语言都不一样

> 以单引号' '包围变量的值时，单引号里面是什么就输出什么，即使内容中有变量和命令（命令需要反引起来）也会把它们原样输出

> 以双引号" "包围变量的值时，输出时会先解析里面的变量和命令，而不是把双引号中的变量名和命令原样输出

> 变量名命名规范：由数字、字母、下划线组成；必须以字母或者下划线开头；不能使用 Shell 里的关键字

> 变量分为环境变量、全局变量、局部变量 

##### 变量定义
```bash
title='hello world！' #全局变量
local title="hello world！" #局部变量
readonly title=hello world！#只读变量
```

##### 变量引用
```bash
echo $title #变量引用一
echo ${title}A #变量引用二
```

##### 变量删除

```bash
unset title #变量删除，只读变量不生效
```

##### 变量作用域

```bash
title='hello world！' #作用域：当前Shell线程有效，对其它 Shell 进程和子进程都无效
export title #此时，该变量变为环境变量；对其子进程也有效
```
> 如果进程不为父子关系却任然想要共享，可以写入一些特定的配置文件.如/etc/profile

### 命令替换
> 命令替换是指将命令的输出结果赋值给某个变量

##### 命令替换的两种格式
```bash
ll_command=`ll`$title #不支持支持嵌套
ll_command=$(ll) #支持嵌套
```

### 位置参数
> bash 里没有实参和形参的概念。函数传参就靠位置参数

> 参数超过了 10 个，那么就得用${n}的形式，如${11}接收11号参数

```bash
# 函数声明（后面再说，主要了解位置参数的使用）
function test(){
    echo "Title: $1"
    echo "Content: $2"
}

test 'Hello World！' 'hello world' #调用函数
```

### 特殊变量

```bash
$0 #当前脚本的文件名
$n(n≥1) #传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是 $1，第二个参数是 $2
$# #传递给脚本或函数的参数个数
$* #传递给脚本或函数的所有参数.当被双引号" "包含时,会将所有的参数从整体上看做一份数据，而不是把每个参数都看做一份数据
$@ #传递给脚本或函数的所有参数。当被双引号" "包含时，会将每个参数都看作一份数据，彼此之间是独立的
$? #上个命令的退出状态，或函数的返回值
$$ #当前 Shell 进程 ID
```

### 字符串API
##### 字符串长度

```bash
${#title}
```

##### 字符串拼接

```
${title1}${title2}
"${title1} ${title2}"
```

##### 字符串截取
格式 | 说明
---|---
${string: start :length} | 从 string 字符串的左边第 start 个字符开始，向右截取 length 个字符。
${string: start} | 从 string 字符串的左边第 start 个字符开始截取，直到最后。
${string: 0-start :length} | 从 string 字符串的右边第 start 个字符开始，向右截取 length 个字符。
${string: 0-start} | 从 string 字符串的右边第 start 个字符开始截取，直到最后。
${string#*chars} | 从 string 字符串第一次出现 *chars 的位置开始，截取 *chars 右边的所有字符。
${string##*chars} | 从 string 字符串最后一次出现 *chars 的位置开始，截取 *chars 右边的所有字符。
${string%*chars} | 从 string 字符串第一次出现 *chars 的位置开始，截取 *chars 左边的所有字符。
${string%%*chars} | 从 string 字符串最后一次出现 *chars 的位置开始，截取 *chars 左边的所有字符。

### 数组API

##### 数组定义及元素获取
```
nums=(29 100 13 8 91 44) #数组定义
nums[10]=66  #给第10个元素赋值（此时会增加数组长度）
echo ${nums[@]}  #输出所有数组元素
echo ${nums[*]}  #输出所有数组元素
echo ${nums[4]}  #输出第4个元素
```
##### 数组长度

```
echo ${#nums[*]}
```

##### 数组拼接

```
(${array1[@]}  ${array2[@]})
```

##### 数组元素删除

```
unset arr[1]
```

### 内建命令


命令 | 说明
---|---
: | 扩展参数列表，执行重定向操作
. | 读取并执行指定文件中的命令（在当前 shell 环境中）
alias | 为指定命令定义一个别名（常用）
bg | 将作业以后台模式运行
bind | 将键盘序列绑定到一个 readline 函数或宏
break | 退出 for、while、select 或 until 循环
builtin | 执行指定的 shell 内建命令
caller | 返回活动子函数调用的上下文
cd | 将当前目录切换为指定的目录
command | 执行指定的命令，无需进行通常的 shell 查找
compgen | 为指定单词生成可能的补全匹配
complete | 显示指定的单词是如何补全的
compopt | 修改指定单词的补全选项
continue | 继续执行 for、while、select 或 until 循环的下一次迭代
declare | 声明一个变量或变量类型。（常用）
dirs | 显示当前存储目录的列表
disown | 从进程作业表中刪除指定的作业
echo | 将指定字符串输出到 STDOUT（用户）
enable | 启用或禁用指定的内建shell命令
eval | 将指定的参数拼接成一个命令，然后执行该命令
exec | 用指定命令替换 shell 进程
exit | 强制 shell 以指定的退出状态码退出（常用）
export | 设置子 shell 进程可用的变量
fc | 从历史记录中选择命令列表
fg | 将作业以前台模式运行
getopts | 分析指定的位置参数
hash | 查找并记住指定命令的全路径名
help | 显示帮助文件
history | 显示命令历史记录
jobs | 列出活动作业
kill | 向指定的进程 ID(PID) 发送一个系统信号
let | 计算一个数学表达式中的每个参数
local | 在函数中创建一个作用域受限的变量
logout | 退出登录 shell
mapfile | 从 STDIN 读取数据行，并将其加入索引数组
popd | 从目录栈中删除记录
printf | 使用格式化字符串显示文本
pushd | 向目录栈添加一个目录
pwd | 显示当前工作目录的路径名
read | 从 STDIN 读取一行数据并将其赋给一个变量（常用）
readarray | 从 STDIN 读取数据行并将其放入索引数组
readonly | 从 STDIN 读取一行数据并将其赋给一个不可修改的变量
return | 强制函数以某个值退出，这个值可以被调用脚本提取
set | 设置并显示环境变量的值和 shell 属性
shift | 将位置参数依次向下降一个位置
shopt | 打开/关闭控制 shell 可选行为的变量值
source | 读取并执行指定文件中的命令（在当前 shell 环境中）
suspend | 暂停 Shell 的执行，直到收到一个 SIGCONT 信号
test | 基于指定条件返回退出状态码 0 或 1
times | 显示累计的用户和系统时间
trap | 如果收到了指定的系统信号，执行指定的命令
type | 显示指定的单词如果作为命令将会如何被解释
typeset | 声明一个变量或变量类型。（常用）
ulimit | 为系统用户设置指定的资源的上限
umask | 为新建的文件和目录设置默认权限
unalias | 刪除指定的别名
unset | 刪除指定的环境变量或 shell 属性
wait | 等待指定的进程完成，并返回退出状态码

### 计算

##### 算术运算符

算术运算符 | 说明/含义
---|---
+、- | 加法（或正号）、减法（或负号）
*、/、% | 乘法、除法、取余（取模）
** | 幂运算
++、-- | 自增和自减，可以放在变量的前面也可以放在变量的后面
!、&&、\|\| | 逻辑非（取反）、逻辑与（and）、逻辑或（or）
<、<=、>、>= | 比较符号（小于、小于等于、大于、大于等于）
==、!=、= | 比较符号（相等、不相等；对于字符串，= 也可以表示相当于）
<<、>> | 向左移位、向右移位
~、\|、 &、^ | 按位取反、按位或、按位与、按位异或
=、+=、-=、*=、/=、%= | 赋值运算符，例如 a+=1 相当于 a=a+1，a-=1 相当于 a=a-1

##### 数学计算命令
> (()) 可以用于整数计算，bc 可以小数计算

```
((表达式))

#两种变量赋值方式
((a=10+66) 
a=$((10+66)
```

### 逻辑控制

##### if else

```
#第一种（写法）
if  condition;  then
    statement(s)
fi

#第二种
if  condition
then
   statement1
else
   statement2
fi

#第三种
if  condition1
then
   statement1
elif condition2
then
    statement2
elif condition3
then
    statement3
……
else
   statementn
fi
```

##### for
>熟悉java或者js的对于这两种类似的写法应该不陌生 
> break和continue的使用一样

```
#第一种（写法）
for((exp1; exp2; exp3))
do
    statements
done

#第二种
for variable in value_list
do
    statements
done
```

### 函数

##### 写法
```
#第一种
function name() {
    statements
    [return value]
}

#第二种
name() {
    statements
    [return value]
}

#第三种
function name {
    statements
    [return value]
}
```

### 退出状态
> 每一条 Shell 命令，不管是 Bash 内置命令（例如 cd、echo），还是外部的 Linux 命令（例如 ls、awk），还是自定义的 Shell 函数，当它退出（运行结束）时，都会返回一个比较小的整数值给调用（使用）它的程序，这就是命令的退出状态（exit statu）。

> 按照惯例来说，退出状态为 0 表示“成功”；也就是说，程序执行完成并且没有遇到任何问题。除 0 以外的其它任何退出状态都为“失败”。

### test命令（有用，[[]]是其升级版，自行了解吧 滑稽.jpg）

### 待续...滑稽保命.jpg