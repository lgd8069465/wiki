[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

在此感谢 [@chrony](https://chrony.tuxfamily.org/download.html) [@ntp]()

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0

~~~
chrony
[root@docker opt]# yum install -y chrony
[root@docker opt]# vi /etc/chrony.conf
server ntp.aliyun.com iburst
stratumweight 0
[root@docker opt]# systemctl restart chronyd
[root@docker opt]# systemctl enable chronyd
[root@docker opt]# cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone

ntp
[root@docker opt]# yum install -y ntp
[root@docker opt]# vi /etc/ntp.conf
server ntp.aliyun.com iburst minpoll 4 maxpoll 10
restrict ntp.aliyun.com nomodify notrap nopeer noquery
[root@docker opt]# systemctl start ntpd
[root@docker opt]# systemctl enable ntpd
[root@docker opt]# cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone

time1.cloud.tencent.com 
time2.cloud.tencent.com 
time3.cloud.tencent.com
time4.cloud.tencent.com
time5.cloud.tencent.com
ntp.tuna.tsinghua.edu.cn
cn.ntp.org.cn
time.facebook.com
time.google.com
time.apple.com
ie.pool.ntp.com
~~~