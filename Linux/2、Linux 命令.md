### 通用

```bash
date #查看当前时间
ps -ef #查看进程
nestat -anp #查看端口占用
kill -9 <pid> #强杀进程
ping -c <count> <ip> #测试连通性
grep <str> #筛选,配合管道使用
grep -v <str> #过滤,配合管道使用
wc -l #统计行数
awk '{print $<num>}' #默认空格划分，打印出相应位置的信息，难以说明，实操吧...
source <file> #可用来执行一堆命令的shell脚本,也可以让一些配置生效
```

### 文件基本操作

```bash
cd ~
cd .
cd ..
cd -

mkdir <dir> #创建单级目录（增）
mkdir -p <dir> #创建多级目录（增）
touch <file> #创建空文件（增）
vim <file> #也可以用来创建文件（增、改、查）
rm -rf <dir> #强制删除目录及以下文件（删）
mv <src> <dst> #移动目录或文件，也可重命名（改）
cp -a <src> <dst> #cp -dpR的简写，拷贝目录或文件，包括目录或文件的链接、属性（改）
ls -a #查看所有目录或文件，包含隐藏目录或文件（查）
ll #ls -l的简写，信息稍微详细点，以列表形式展示所有目录或文件，不包含隐藏目录或文件（查）
pwd #查看当前目录（查）
find #根据文件属性查找文件，参数丰富，不做详细介绍；使用实例在附录（查）
cat|less|more|tail #查看文件（查）
```

### 文件属性操作

```bash
chmod 777 <dir> #数字形式，改变目录或文件权限
chmod u=rwx,g=rwx,o=rwx <dir> #字母形式，与上面一样(r=4=读权限,w=2=写权限,x=1=可执行权限;u=用户,g=用户组,o=其它)
chmod +r <dir> #当没有指定u、g、o时，全部添加上读权限
chmod -w <dir> #与上同理，取消写权限
chmod =x <dir> #这个应该也就不用我说了
chgrp <grp> <dir> #改变文件的所属用户组
chown <usr> <dir> #改变文件的所属用户
lsattr <file> #ls attr...很语义化，列出文件的隐藏属性;想要看目录加上-d，递归-R

chattr <file> #改变文件的隐藏属性，我所认识的只有i，加上后文件无敌，谁都动不了，包括所属用户;递归-R
```

### 压缩与解压

```bash
科普：linux文件后缀名跟文件真实类型没有必然关系，想看具体的文件类型可以通过file

tar -cvf <dst> <src> #打包成.tar格式
tar -xvf <file> #上面的反向操作
tar -zcvf <dst> <src> #压缩成.tar.gz格式
tar -zxvf <file> #上面的反向操作
tar -jcvf <dst> <src> #压缩成.tar.bz2格式
tar -jxvf <file> #上面的反向操作
```

### 用户及用户组

```bash
groupadd -g <gid> #创建用户组
useradd -u <uid> -g <gid> -s <shell> -d <指定根目录> -c <评论> #创建用户 
adduser <usr> #上面的如果不指定参数会创建“三无”用户，无密码，无home目录，无shell;相比而言这个更不用记忆
userdel -r <usr> #完全删除用户
passwd <usr> 修改密码
```

### 包管理

```bash
注：个人更喜欢yum，不使用npm...所以没讲npm

yum list #列出所有可安装的软件包
yum list installed  #列出所有已安装的软件包
yum install package #安装
yum remove package #卸载
yum update #更新
yum clean all #清理所有yum缓存
yum makecache #缓存
yum -y install yum-utils #安装yum-utils包，包含一些yum命令
yum repolist all #列出所有仓库
yum-config-manager --disable mysql80-community #禁用该版本
yum-config-manager --enable mysql57-community #启用该版本
```

### 服务

```bash
systemctl list-units --type=service #查看所有已启动服务
systemctl status iptables.service #查看服务状态
systemctl start iptables.service #开启iptables防火墙服务
systemctl stop iptables.service #停止服务
systemctl restart iptables.service #重启服务
systemctl mask iptables.service #注销服务（服务链接至/dev/null，且禁止开机自启）
systemctl unmask iptables.service #恢复服务
systemctl enable iptables.service #设置为开机自启动
systemctl disable iptables.service #设置为不开机自启动
```
### 文件传输
>注：就介绍了scp，因为命令简单、ssh加密、不可断点续传、cpu占用少；sftp没介绍，它命令复杂、ssh加密、可断点续传 、cpu占用多；至于rz、sz也没介绍，有兴趣自己去了解下

```bash
scp -r <src> <usr>@<ip>:<dst> #将本地文件复制到远程服务器
scp -r <src> <dst> #本地文件拷贝
```

### 配置文件

```bash
/etc/sysconfig/iptables #防火墙文件，说道防火墙，插一句：CentOS 7默认使用firewall作为防火墙
/etc/profile #全用户环境变量
/$HOME/.bash_profile #当前用户环境变量
/etc/passwd #记录用户密码
/etc/group #记录用户组信息
/etc/ssh/sshd_config #ssh服务配置
/etc/hosts.allow #ip白名单
/etc/hosts.deny #ip黑名单
```

### 附录
##### find使用实例

```
// 将目前目录及其子目录下所有延伸档名是 c 的文件列出来
find . -name "*.c"
// 将目前目录其其下子目录中所有一般文件列出
find . -type f
// 将目前目录其其下子目录中所有目录文件列出
find . -type d
// 将目前目录及其子目录下所有最近 20 天内更新过的文件列出
find . -ctime -20
// 查找/var/log目录中更改时间在7日以前的普通文件，并在删除之前询问它们
find /var/log -type f -mtime +7 -ok rm {} \;
// 查找前目录中文件属主具有读、写权限，并且文件所属组的用户和其他用户具有读权限的文件
find . -type f -perm 644 -exec ls -l {} \;
// 为了查找系统中所有文件长度为0的普通文件，并列出它们的完整路径
find / -type f -size 0 -exec ls -l {} \;
```
> -atime 访问时间；-ctime 变更时间（不包括文件内容变更，如vim）；-mtime 修改时间（vim）；可以通过stat查看文件的这些属性