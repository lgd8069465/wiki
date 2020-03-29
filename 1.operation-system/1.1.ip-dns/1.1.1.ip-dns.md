[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0
~~~
[root@docker opt]# vi /etc/sysconfig/network-scripts/ifcfg-ens33
BOOTPROTO="static"
ONBOOT="yes"
IPADDR=192.168.29.130
NETMASK=255.255.255.0
GATEWAY=192.168.29.2
DNS1=223.5.5.5
DNS2=8.8.8.8
[root@docker opt]# systemctl restart network
[root@docker opt]# ip addr

[root@docker opt]# vi /etc/resolv.conf
nameserver 223.5.5.5
nameserver 8.8.8.8

// 可能出现如下问题
IPv4 forwarding is disabled. Networking will not work.
[root@docker opt]# vi /etc/sysctl.conf
net.ipv4.ip_forward=1
[root@docker opt]# sysctl -p
[root@docker opt]# systemctl restart network
[root@docker opt]# sysctl net.ipv4.ip_forward

IPv4 
223.6.6.6
1.1.1.1
101.6.6.6                            # 清华
180.76.76.76                         # 百度

IPv6 
2001:da8::666                        # 清华
2400:3200::1                         # 阿里
2400:3200:baba::1                    # 阿里
2400:da00::6666                      # 百度
~~~