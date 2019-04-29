Shell学习链接（超详细）http://c.biancheng.net/shell/
> shell 是内核与应用程序之间的桥梁

> 常见的 Shell 有 sh、csh、tcsh、ash、bash

> bash shell 是 Linux 的默认 shell

> 现代的 Linux 上，sh 已经被 bash 代替，/bin/sh往往是指向/bin/bash的符号链接

> 对于普通用户，bash 默认的提示符是美元符号$；对于超级用户（root 用户），bash 默认的提示符是井号#


###

    #!/bin/bash
    echo "Hello World !"  #这是一条语句
    
第 1 行的#!是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种Shell；后面的/bin/bash就是指明了解释器的具体位置。

第 2 行的 echo 命令用于向标准输出文件（Standard Output，stdout，一般就是指终端）输出文本。在.sh文件中使用命令与在终端直接输入命令的效果是一样的。

第 2 行的#及其后面的内容是注释。Shell 脚本中所有以#开头的都是注释（当然以#!开头的除外）。
    
    #!/bin/bash
    echo "What is your name?"
    read PERSON
    echo "Hello, $PERSON"
第 3 行中表示从终端读取用户输入的数据，并赋值给 PERSON 变量。read 命令用来从标准输入文件（Standard Input，stdin，一般就是指终端）读取用户输入的数据。

第 4 行表示输出变量 PERSON 的内容。注意在变量名前边要加上$，否则变量名会作为字符串的一部分处理。

    . ./test.sh

点号用于执行某个脚本，甚至脚本没有可执行权限也可以运行。有时候在测试运行某个脚本时可能并不想为此修改脚本权限，这时候就可以使用.来运行脚本，非常方便

    source test.sh

与点号类似，source 命令也可读取并在当前环境中执行脚本，同时还可返回脚本中最后一个命令的返回状态；如果没有返回值则返回 0，代表执行成功；如果未找到指定的脚本则返回 false。