## 在Linux安装DHCP服务器

### 安装

```bash
sudo apt-get install isc-dhcp-server
```

### 配置

#### /etc/default/isc-dhcp-server

```properties
INTERFACES="eth0"
```

#### sudo vi /etc/dhcp/dhcpd.conf

```nginx
option domain-name "inotseeyou.com";
option domain-name-servers 114.114.115.115, 8.8.8.8;
default-lease-time 3600; 
max-lease-time 7200;authoritative;

# 动态IP分配
subnet 192.168.2.0 netmask 255.255.255.0
{  
  option routers                  192.168.2.1;
  option subnet-mask              255.255.255.0;
  option domain-search            "inotseeyou.com";
  option domain-name-servers      192.168.2.1;
  range   192.168.2.100   192.168.10.150;
  range   192.168.10.160   192.168.10.200;
}

# 固定IP分配
host centos-node
{  
  hardware ethernet 00:f0:m4:6y:89:0g;
  fixed-address 192.168.10.105;
}
host fedora-node
{
  hardware ethernet 00:4g:8h:13:8h:3a;
  fixed-address 192.168.10.106;
}
```

#### 检查配置

```bash
sudo dhcpd -t
```

#### 查看是否有dhcp服务

```bash
sudo netstat -uap | grep 'dhcp*'
```

## 搭建网关服务器

#### 开启forward

```properties
# sudo vi /etc/sysctl.conf
net.ipv4.ip_forward = 1
```

#### 配置转发

```bash
# 转发网段172.16.0.0/24传过来的包
iptables -t nat -A POSTROUTING -s 172.16.0.0/24 -j MASQUERADE
# 转发指定特定的ip(172.16.0.10)传过来的包。例如：
iptables -t nat -A POSTROUTING -s 172.16.0.10/24 -j MASQUERADE
# 重启生效
sudo service iptables restart
# 查看结果
sudo iptables --table nat --list
sudo iptables -t nat -nL --line
# 其他用法
# 禁止指定IP访问外网
sudo iptables -A OUTPUT -d 203.16.1.89 -j REJECT
# 访问一个外网IP重定向到一个内网IP
sudo iptables -t nat -A OUTPUT -d any -j DNAT --to-destination 172.16.0.1
# flush iptables
sudo iptables -F
sudo iptables -t nat -F
sudo iptables --table nat --list
```

## 参考

* [Linux下dhcpd服务中主配置文件/etc/dhcp/dhcpd.conf文件中参数详解](https://blog.csdn.net/qq_42303254/article/details/89477125)
* [Debian的dhcp服务器安装、配置及启动失败问题](https://blog.csdn.net/bell_love/article/details/105680281)
* [在 Ubuntu 中安装 DHCP 服务器](https://cloud.tencent.com/developer/article/1877573)
* [linux+iptables搭建网关服务器](https://www.cnblogs.com/chenshoubiao/p/4782276.html)
* [iptables实现IP地址重定向](https://www.cnblogs.com/EasonJim/p/7589394.html)
