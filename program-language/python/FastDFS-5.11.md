[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

在此感谢 [@happyfish100](https://github.com/happyfish100/) [@小Nemo诶](https://www.jianshu.com/p/67243fb2a434)

文档不定时更新，如有问题请及时联系本文作者，当前文档版本1.0.0

[fastdfs-5.11.tar.gz](https://github.com/happyfish100/fastdfs/releases)

[fastdfs-nginx-module-1.20.tar.gz](https://github.com/happyfish100/fastdfs-nginx-module/releases)

[libfastcommon-1.0.41.tar.gz](https://github.com/happyfish100/libfastcommon/releases)

[nginx-1.16.1.tar.gz](http://nginx.org/en/download.html)
~~~
// 操作系统版本
[root@kubernetes opt]# cat /etc/centos-release
CentOS Linux release 7.7.1908 (Core)
// 关闭selinux
[root@kubernetes opt]# vi /etc/selinux/config
SELINUX=disabled
// 重启
[root@kubernetes opt]# reboot
// 设置主机名
[root@kubernetes opt]# vi /etc/hostname
kubernetes
[root@kubernetes opt]# vi /etc/hosts
192.168.192.129 kubernetes
// 关闭防火墙
[root@kubernetes opt]# systemctl stop firewalld
// 关闭自启动防火墙
[root@kubernetes opt]# systemctl disable firewalld
// 查看IP
[root@kubernetes opt]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:1c:6e:f5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.192.129/24 brd 192.168.192.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::e7f8:8f77:9df8:ed25/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:85:25:9c:dc brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever

[root@kubernetes src]# cd /opt/
// 软件版本
[root@kubernetes opt]# ll
total 1528
drwx--x--x  4 root root      28 Oct  9 16:40 containerd
-rw-r--r--  1 root root  336939 Oct 23 15:11 fastdfs-5.11.tar.gz
-rw-r--r--  1 root root   19825 Oct 23 15:55 fastdfs-nginx-module-1.20.tar.gz
-rw-r--r--  1 root root  164111 Oct 23 14:59 libfastcommon-1.0.41.tar.gz
-rw-r--r--  1 root root 1032630 Oct 23 15:00 nginx-1.16.1.tar.gz
// 解压软件
[root@kubernetes opt]# tar zxf fastdfs-5.11.tar.gz 
[root@kubernetes opt]# tar zxf fastdfs-nginx-module-1.20.tar.gz
[root@kubernetes opt]# tar zxf libfastcommon-1.0.41.tar.gz 
[root@kubernetes opt]# tar zxf nginx-1.16.1.tar.gz 
[root@kubernetes opt]# ll
total 1528
drwx--x--x  4 root root      28 Oct  9 16:40 containerd
drwxrwxr-x 10 root root     258 Oct 23 15:12 fastdfs-5.11
-rw-r--r--  1 root root  336939 Oct 23 15:11 fastdfs-5.11.tar.gz
drwxrwxr-x  3 root root      47 Jul  5  2018 fastdfs-nginx-module-1.20
-rw-r--r--  1 root root   19825 Oct 23 15:55 fastdfs-nginx-module-1.20.tar.gz
drwxrwxr-x  5 root root     135 Oct 23 15:11 libfastcommon-1.0.41
-rw-r--r--  1 root root  164111 Oct 23 14:59 libfastcommon-1.0.41.tar.gz
drwxr-xr-x  9 1001 1001     186 Oct 23 15:48 nginx-1.16.1
-rw-r--r--  1 root root 1032630 Oct 23 15:00 nginx-1.16.1.tar.gz
// 安装依赖包
[root@kubernetes opt]# yum install -y gcc gcc-c++ make libstdc++-devel
// 安装libfastcommon-1.0.41
[root@kubernetes opt]# cd libfastcommon-1.0.41
[root@kubernetes libfastcommon-1.0.41]# ll
total 32
drwxrwxr-x 2 root root  114 Oct 15 21:47 doc
-rw-rw-r-- 1 root root 9945 Oct 15 21:47 HISTORY
-rw-rw-r-- 1 root root  566 Oct 15 21:47 INSTALL
-rw-rw-r-- 1 root root 1607 Oct 15 21:47 libfastcommon.spec
-rwxrwxr-x 1 root root 3253 Oct 15 21:47 make.sh
drwxrwxr-x 2 root root  191 Oct 15 21:47 php-fastcommon
-rw-rw-r-- 1 root root 2776 Oct 15 21:47 README
drwxrwxr-x 3 root root 4096 Oct 15 21:47 src
[root@kubernetes libfastcommon-1.0.41]# ./make.sh 
[root@kubernetes libfastcommon-1.0.41]# ./make.sh install
// 安装fastdfs-5.11
[root@kubernetes libfastcommon-1.0.41]# cd ../fastdfs-5.11
[root@kubernetes fastdfs-5.11]# ll
total 116
drwxrwxr-x 3 root root  4096 Jun  3  2017 client
drwxrwxr-x 2 root root   189 Jun  3  2017 common
drwxrwxr-x 2 root root   146 Jun  3  2017 conf
-rw-rw-r-- 1 root root 35067 Jun  3  2017 COPYING-3_0.txt
-rw-rw-r-- 1 root root  3171 Jun  3  2017 fastdfs.spec
-rw-rw-r-- 1 root root 33100 Jun  3  2017 HISTORY
drwxrwxr-x 2 root root    48 Jun  3  2017 init.d
-rw-rw-r-- 1 root root  7755 Jun  3  2017 INSTALL
-rwxrwxr-x 1 root root  5548 Jun  3  2017 make.sh
drwxrwxr-x 2 root root   320 Jun  3  2017 php_client
-rw-rw-r-- 1 root root  2380 Jun  3  2017 README.md
-rwxrwxr-x 1 root root  1768 Jun  3  2017 restart.sh
-rwxrwxr-x 1 root root  1680 Jun  3  2017 stop.sh
drwxrwxr-x 4 root root  4096 Jun  3  2017 storage
drwxrwxr-x 2 root root   317 Jun  3  2017 test
drwxrwxr-x 2 root root  4096 Jun  3  2017 tracker
[root@kubernetes fastdfs-5.11]# ./make.sh 
[root@kubernetes fastdfs-5.11]# ./make.sh install
[root@kubernetes fastdfs-5.11]#      
[root@kubernetes fastdfs-5.11]# cd /etc/fdfs/
[root@kubernetes fdfs]# ll
total 24
-rw-r--r-- 1 root root 1461 Oct 23 15:12 client.conf.sample
-rw-r--r-- 1 root root 7927 Oct 23 15:12 storage.conf.sample
-rw-r--r-- 1 root root  105 Oct 23 15:12 storage_ids.conf.sample
-rw-r--r-- 1 root root 7389 Oct 23 15:12 tracker.conf.sample
// 复制tracker配置文件
[root@kubernetes fdfs]# cp tracker.conf.sample tracker.conf
[root@kubernetes fdfs]# ll
total 32
-rw-r--r-- 1 root root 1461 Oct 23 15:12 client.conf.sample
-rw-r--r-- 1 root root 7927 Oct 23 15:12 storage.conf.sample
-rw-r--r-- 1 root root  105 Oct 23 15:12 storage_ids.conf.sample
-rw-r--r-- 1 root root 7389 Oct 23 15:13 tracker.conf
-rw-r--r-- 1 root root 7389 Oct 23 15:12 tracker.conf.sample
// 编辑tracker配置文件
[root@kubernetes fdfs]# vi tracker.conf
port=22122
base_path=/home/fastdfs/tracker
[root@kubernetes fdfs]# 
[root@kubernetes fdfs]# cd /etc/init.d/
[root@kubernetes init.d]# ll fdfs_
fdfs_storaged  fdfs_trackerd 
// 创建tracker主目录 
[root@kubernetes init.d]# mkdir -pv /home/fastdfs/tracker
mkdir: created directory ‘/home/fastdfs’
mkdir: created directory ‘/home/fastdfs/tracker’
// 查看tracker服务状态
[root@kubernetes init.d]# ./fdfs_trackerd status
fdfs_trackerd is stopped
// 启动tracker服务
[root@kubernetes init.d]# ./fdfs_trackerd start
Starting FastDFS tracker server: 
[root@kubernetes init.d]# ./fdfs_trackerd status
fdfs_trackerd (pid 3442) is running...
// 查看fdfs服务端口
[root@kubernetes init.d]# ss -lnp | grep fdfs
tcp    LISTEN     0      128       *:22122                 *:*                   users:(("fdfs_trackerd",pid=3442,fd=5))
// 开机自启动tracker服务
[root@kubernetes init.d]# vi /etc/rc.d/rc.local
/etc/init.d/fdfs_trackerd start
[root@kubernetes init.d]# chmod u+x /etc/rc.d/rc.local
[root@kubernetes init.d]# cd /etc/fdfs/
[root@kubernetes fdfs]# ll
total 32
-rw-r--r-- 1 root root 1461 Oct 23 15:12 client.conf.sample
-rw-r--r-- 1 root root 7927 Oct 23 15:12 storage.conf.sample
-rw-r--r-- 1 root root  105 Oct 23 15:12 storage_ids.conf.sample
-rw-r--r-- 1 root root 7390 Oct 23 15:24 tracker.conf
-rw-r--r-- 1 root root 7389 Oct 23 15:12 tracker.conf.sample
// 复制storage配置文件
[root@kubernetes fdfs]# cp storage.conf.sample storage.conf
[root@kubernetes fdfs]# ll
total 40
-rw-r--r-- 1 root root 1461 Oct 23 15:12 client.conf.sample
-rw-r--r-- 1 root root 7927 Oct 23 15:29 storage.conf
-rw-r--r-- 1 root root 7927 Oct 23 15:12 storage.conf.sample
-rw-r--r-- 1 root root  105 Oct 23 15:12 storage_ids.conf.sample
-rw-r--r-- 1 root root 7390 Oct 23 15:24 tracker.conf
-rw-r--r-- 1 root root 7389 Oct 23 15:12 tracker.conf.sample
// 编辑storage配置文件
[root@kubernetes fdfs]# vi storage.conf
port=23000
base_path=/home/fastdfs/storage
store_path0=/home/fastdfs/storage
tracker_server=192.168.192.129:22122                    # tracker服务所在宿主机实际IP，切勿写127.0.0.1/localhost
[root@kubernetes fdfs]# cd /etc/init.d/
// 创建storage主目录      
[root@kubernetes init.d]# mkdir -pv /home/fastdfs/storage
mkdir: created directory ‘/home/fastdfs/storage’
// 查看storage服务状态
[root@kubernetes init.d]# ./fdfs_storaged status
fdfs_storaged is stopped
// 启动storage服务
[root@kubernetes init.d]# ./fdfs_storaged start
Starting FastDFS storage server: 
// 查看fdfs服务端口
[root@kubernetes init.d]# ss -lnp | grep fdfs
tcp    LISTEN     0      128       *:23000                 *:*                   users:(("fdfs_storaged",pid=14133,fd=5))
tcp    LISTEN     0      128       *:22122                 *:*                   users:(("fdfs_trackerd",pid=3442,fd=5))
// 开机自启动storage服务
[root@kubernetes init.d]# vi /etc/rc.d/rc.local
/etc/init.d/fdfs_storaged start 
[root@kubernetes init.d]# chmod u+x /etc/rc.d/rc.local
// 通信测试
[root@kubernetes init.d]# /usr/bin/fdfs_monitor /etc/fdfs/storage.conf
[2019-10-23 15:36:23] DEBUG - base_path=/home/fastdfs/storage, connect_timeout=30, network_timeout=60, tracker_server_count=1, anti_steal_token=0, anti_steal_secret_key length=0, use_connection_pool=0, g_connection_pool_max_idle_time=3600s, use_storage_id=0, storage server id count: 0

server_count=1, server_index=0

tracker server is 192.168.192.129:22122

group count: 1

Group 1:
group name = group1
disk total space = 17394 MB
disk free space = 15146 MB
trunk free space = 0 MB
storage server count = 1
active server count = 1
storage server port = 23000
storage HTTP port = 8888
store path count = 1
subdir count per path = 256
current write server index = 0
current trunk file id = 0

	Storage 1:
		id = 192.168.192.129
		ip_addr = 192.168.192.129 (anantes-651-1-49-net.w2-0.abo.wanadoo.fr)  ACTIVE
		http domain = 
		version = 5.11
		join time = 2019-10-23 15:35:05
		up time = 2019-10-23 15:35:05
		total storage = 17394 MB
		free storage = 15146 MB
		upload priority = 10
		store_path_count = 1
		subdir_count_per_path = 256
		storage_port = 23000
		storage_http_port = 8888
		current_write_path = 0
		source storage id = 
		if_trunk_server = 0
		connection.alloc_count = 256
		connection.current_count = 0
		connection.max_count = 0
		total_upload_count = 0
		success_upload_count = 0
		total_append_count = 0
		success_append_count = 0
		total_modify_count = 0
		success_modify_count = 0
		total_truncate_count = 0
		success_truncate_count = 0
		total_set_meta_count = 0
		success_set_meta_count = 0
		total_delete_count = 0
		success_delete_count = 0
		total_download_count = 0
		success_download_count = 0
		total_get_meta_count = 0
		success_get_meta_count = 0
		total_create_link_count = 0
		success_create_link_count = 0
		total_delete_link_count = 0
		success_delete_link_count = 0
		total_upload_bytes = 0
		success_upload_bytes = 0
		total_append_bytes = 0
		success_append_bytes = 0
		total_modify_bytes = 0
		success_modify_bytes = 0
		stotal_download_bytes = 0
		success_download_bytes = 0
		total_sync_in_bytes = 0
		success_sync_in_bytes = 0
		total_sync_out_bytes = 0
		success_sync_out_bytes = 0
		total_file_open_count = 0
		success_file_open_count = 0
		total_file_read_count = 0
		success_file_read_count = 0
		total_file_write_count = 0
		success_file_write_count = 0
		last_heart_beat_time = 2019-10-23 15:36:05
		last_source_update = 1970-01-01 08:00:00
		last_sync_update = 1970-01-01 08:00:00
		last_synced_timestamp = 1970-01-01 08:00:00 
[root@kubernetes init.d]# 
[root@kubernetes init.d]# cd /etc/fdfs/
[root@kubernetes fdfs]# ll
total 40
-rw-r--r-- 1 root root 1461 Oct 23 15:12 client.conf.sample
-rw-r--r-- 1 root root 7929 Oct 23 15:33 storage.conf
-rw-r--r-- 1 root root 7927 Oct 23 15:12 storage.conf.sample
-rw-r--r-- 1 root root  105 Oct 23 15:12 storage_ids.conf.sample
-rw-r--r-- 1 root root 7390 Oct 23 15:24 tracker.conf
-rw-r--r-- 1 root root 7389 Oct 23 15:12 tracker.conf.sample
// 复制client配置文件
[root@kubernetes fdfs]# cp client.conf.sample client.conf
[root@kubernetes fdfs]# ll
total 44
-rw-r--r-- 1 root root 1461 Oct 23 15:37 client.conf
-rw-r--r-- 1 root root 1461 Oct 23 15:12 client.conf.sample
-rw-r--r-- 1 root root 7929 Oct 23 15:33 storage.conf
-rw-r--r-- 1 root root 7927 Oct 23 15:12 storage.conf.sample
-rw-r--r-- 1 root root  105 Oct 23 15:12 storage_ids.conf.sample
-rw-r--r-- 1 root root 7390 Oct 23 15:24 tracker.conf
-rw-r--r-- 1 root root 7389 Oct 23 15:12 tracker.conf.sample
// 编辑client配置文件
[root@kubernetes fdfs]# vi client.conf
base_path=/home/fastdfs/client
tracker_server=192.168.192.129:22122                    # tracker服务所在宿主机实际IP，切勿写127.0.0.1/localhost
// 创建client主目录
[root@kubernetes fdfs]# mkdir -pv /home/fastdfs/client
mkdir: created directory ‘/home/fastdfs/client’
// 上传文件测试
[root@kubernetes fdfs]# /usr/bin/fdfs_upload_file /etc/fdfs/client.conf /opt/fastdfs-5.11.tar.gz 
group1/M00/00/00/wKjAgV2wA9uAe_zaAAUkK6yqBFI.tar.gz
[root@kubernetes fdfs]# 
[root@kubernetes fdfs]# cd /opt/
// 安装依赖包
[root@kubernetes opt]# yum install -y libtool zlib zlib-devel openssl openssl-devel pcre pcre-devel 
[root@kubernetes opt]# cd nginx-1.16.1
[root@kubernetes nginx-1.16.1]# ll
total 748
drwxr-xr-x 6 1001 1001    326 Oct 23 15:01 auto
-rw-r--r-- 1 1001 1001 296463 Aug 13 20:51 CHANGES
-rw-r--r-- 1 1001 1001 452171 Aug 13 20:51 CHANGES.ru
drwxr-xr-x 2 1001 1001    168 Oct 23 15:01 conf
-rwxr-xr-x 1 1001 1001   2502 Aug 13 20:51 configure
drwxr-xr-x 4 1001 1001     72 Oct 23 15:01 contrib
drwxr-xr-x 2 1001 1001     40 Oct 23 15:01 html
-rw-r--r-- 1 1001 1001   1397 Aug 13 20:51 LICENSE
drwxr-xr-x 2 1001 1001     21 Oct 23 15:01 man
-rw-r--r-- 1 1001 1001     49 Aug 13 20:51 README
drwxr-xr-x 9 1001 1001     91 Oct 23 15:01 src
[root@kubernetes opt]# cd fastdfs-nginx-module-1.20
[root@kubernetes fastdfs-nginx-module-1.20]# ll
total 8
-rw-rw-r-- 1 root root 2804 Jul  5  2018 HISTORY
-rw-rw-r-- 1 root root 1748 Jul  5  2018 INSTALL
drwxrwxr-x 2 root root  109 Jul  5  2018 src
[root@kubernetes fastdfs-nginx-module-1.20]# cd src/
[root@kubernetes src]# ll
total 84
-rw-rw-r-- 1 root root 43501 Jul  5  2018 common.c
-rw-rw-r-- 1 root root  3995 Jul  5  2018 common.h
-rw-rw-r-- 1 root root   848 Jul  5  2018 config
-rw-rw-r-- 1 root root  3725 Jul  5  2018 mod_fastdfs.conf
-rw-rw-r-- 1 root root 28668 Jul  5  2018 ngx_http_fastdfs_module.c
// 修改fastdfs-nginx-module-1.20配置文件
[root@kubernetes src]# vi config 
ngx_module_incs="/usr/include/fastdfs /usr/include/fastcommon/" 
CORE_INCS="$CORE_INCS /usr/include/fastdfs /usr/include/fastcommon/
[root@kubernetes src]# cd /opt/nginx-1.16.1
// 配置nginx
[root@kubernetes nginx-1.16.1]# ./configure --prefix=/opt/nginx --add-module=/opt/fastdfs-nginx-module-1.20/src/
// 编译安装nginx
[root@kubernetes nginx-1.16.1]# make && make install
[root@kubernetes nginx-1.16.1]# ll /opt/nginx
total 0
drwxr-xr-x 2 root root 333 Oct 23 15:57 conf
drwxr-xr-x 2 root root  40 Oct 23 15:57 html
drwxr-xr-x 2 root root   6 Oct 23 15:57 logs
drwxr-xr-x 2 root root  19 Oct 23 15:57 sbin
// 查看nginx模块信息
[root@kubernetes nginx-1.16.1]# /opt/nginx/sbin/nginx -V
nginx version: nginx/1.16.1
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) 
configure arguments: --prefix=/opt/nginx --add-module=/opt/fastdfs-nginx-module-1.20/src/
[root@kubernetes nginx-1.16.1]# cd /opt/fastdfs-nginx-module-1.20/src/
[root@kubernetes src]# ll
total 84
-rw-rw-r-- 1 root root 43501 Jul  5  2018 common.c
-rw-rw-r-- 1 root root  3995 Jul  5  2018 common.h
-rw-rw-r-- 1 root root   991 Oct 23 15:56 config
-rw-rw-r-- 1 root root  3725 Jul  5  2018 mod_fastdfs.conf
-rw-rw-r-- 1 root root 28668 Jul  5  2018 ngx_http_fastdfs_module.c
// 复制mod_fastdfs配置文件
[root@kubernetes src]# cp mod_fastdfs.conf /etc/fdfs/
// 编辑mod_fastdfs配置文件
[root@kubernetes src]# vi /etc/fdfs/mod_fastdfs.conf 
base_path=/home/fastdfs
tracker_server=192.168.192.129:22122
store_path0=/home/fastdfs/storage
url_have_group_name = true
// 编辑nginx配置文件
[root@kubernetes src]# vi /opt/nginx/conf/nginx.conf
location ~/M00 {
    root /home/fastdfs/storage/data;
    ngx_fastdfs_module;
}  
// 复制http配置文件
[root@kubernetes src]# cp /opt/fastdfs-5.11/conf/http.conf /etc/fdfs/
// 复制mime配置文件
[root@kubernetes src]# cp /opt/fastdfs-5.11/conf/mime.types /etc/fdfs/
// 启动nginx
[root@kubernetes src]# /opt/nginx/sbin/nginx 
ngx_http_fastdfs_set pid=23695
// 开机自启动nginx服务
[root@kubernetes init.d]# vi /etc/rc.d/rc.local
/opt/nginx/sbin/nginx 
[root@kubernetes init.d]# chmod u+x /etc/rc.d/rc.local
[root@kubernetes src]# ss -lnp | grep nginx
tcp    LISTEN     0      128       *:80                    *:*                   users:(("nginx",pid=23697,fd=6),("nginx",pid=23696,fd=6))
[root@kubernetes src]# 
[root@kubernetes src]# ss -lnp | grep fdfs
tcp    LISTEN     0      128       *:23000                 *:*                   users:(("fdfs_storaged",pid=14133,fd=5))
tcp    LISTEN     0      128       *:22122                 *:*                   users:(("fdfs_trackerd",pid=3442,fd=5))
[root@kubernetes src]# ll /home/fastdfs/
total 0
drwxr-xr-x 2 root root  6 Oct 23 15:40 client
drwxr-xr-x 4 root root 30 Oct 23 15:35 storage
drwxr-xr-x 4 root root 30 Oct 23 15:24 tracker
// 浏览器访问下载
http://192.168.192.129/group1/M00/00/00/wKjAgV2wA9uAe_zaAAUkK6yqBFI.tar.gz
~~~