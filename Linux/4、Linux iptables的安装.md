### iptables安装
> 注：CentOS 7默认使用firewall作为防火墙


```bash
1、systemctl stop firewalld.service #停掉firewall
2、systemctl disable firewalld.service #取消开机启动
3、yum install iptables-services #安装iptables
4、vim /etc/sysconfig/iptables #编译防火墙文件，开放相应的端口
5、systemctl start iptables.service #启动iptables防火墙
6、systemctl enable iptables.service #添加开机启动
```
