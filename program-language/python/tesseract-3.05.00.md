[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

在此感谢 [@tesseract-ocr](https://github.com/tesseract-ocr) [@DanBloomberg](https://github.com/DanBloomberg)

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0

[leptonica-1.74.4.tar.gz](https://github.com/DanBloomberg/leptonica/releases/download/1.74.4/leptonica-1.74.4.tar.gz)

[tesseract-3.05.00.tar.gz](https://github.com/tesseract-ocr/tesseract/archive/3.05.00.tar.gz)
~~~
// 操作系统版本
[root@kubernetes opt]# cat /etc/centos-release
CentOS Linux release 7.7.1908 (Core)
// 关闭selinux
[root@kubernetes opt]# vi /etc/selinux/config
SELINUX=disabled
// 重启
[root@kubernetes opt]# reboot
// 解压软件
[root@kubernetes opt]# tar zxf leptonica-1.74.4.tar.gz
[root@kubernetes opt]# tar zxf tesseract-3.05.00.tar.gz
// 安装依赖包
[root@kubernetes opt]# yum install -y gcc gcc-c++ autoconf automake libtool libpng-devel libjpeg-devel libtiff-devel zlib-devel libicu-devel
[root@kubernetes opt]# cd leptonica-1.74.4
// 配置leptonica-1.74.4
[root@kubernetes leptonica-1.74.4]# ./configure
// 编译安装
[root@kubernetes leptonica-1.74.4]# make && make install
[root@kubernetes leptonica-1.74.4]# cd /opt/tesseract-3.05.00
// 配置tesseract-3.05.00
[root@kubernetes tesseract-3.05.00]# ./autogen.sh
[root@kubernetes tesseract-3.05.00]# ./configure
// 编译安装
[root@kubernetes tesseract-3.05.00]# make && make install
// 即时生效动态链接库和缓存
[root@kubernetes tesseract-3.05.00]# ldconfig
// 复制语言包
~~~
[root@kubernetes opt]# cp [chi_sim.traineddata](https://github.com/tesseract-ocr/tessdata/) /usr/local/share/tessdata/

[root@kubernetes opt]# cp [chi_tra.traineddata](https://github.com/tesseract-ocr/tessdata/) /usr/local/share/tessdata/

[root@kubernetes opt]# cp [eng.traineddata](https://github.com/tesseract-ocr/tessdata/) /usr/local/share/tessdata/

~~~
// 进入测试目录
[root@kubernetes opt]# cd /opt/tesseract-3.05.00/testing/
[root@kubernetes testing]# ll
total 1920
-rwxrwxr-x 1 root root   1785 Feb 17  2017 counttestset.sh
-rw-rw-r-- 1 root root 708120 Feb 17  2017 DuTillet1004Pg2LG.jpg
-rw-rw-r-- 1 root root 102598 Feb 17  2017 eurotext.tif
-rw-rw-r-- 1 root root   1219 Feb 17  2017 FILES
-rw-rw-r-- 1 root root 399613 Feb 17  2017 hebrew-nikud-genesis-1-2.png
-rw-rw-r-- 1 root root 193942 Feb 17  2017 hebrew.png
-rw-rw-r-- 1 root root 444141 Feb 17  2017 hebtypo.jpg
-rw-r--r-- 1 root root  12639 Oct 24 17:27 Makefile
-rw-rw-r-- 1 root root    219 Feb 17  2017 Makefile.am
-rw-r--r-- 1 root root  12641 Oct 24 17:26 Makefile.in
-rw-rw-r-- 1 root root  38668 Feb 17  2017 phototest.tif
-rw-rw-r-- 1 root root   1687 Feb 17  2017 README
-rwxrwxr-x 1 root root   1729 Feb 17  2017 reorgdata.sh
drwxrwxr-x 2 root root    140 Feb 17  2017 reports
-rwxrwxr-x 1 root root   4148 Feb 17  2017 runalltests.sh
-rwxrwxr-x 1 root root   2060 Feb 17  2017 runtestset.sh
// 测试图片
[root@kubernetes testing]# tesseract phototest.tif result -l eng
[root@kubernetes testing]# ll
total 1924
-rwxrwxr-x 1 root root   1785 Feb 17  2017 counttestset.sh
-rw-rw-r-- 1 root root 708120 Feb 17  2017 DuTillet1004Pg2LG.jpg
-rw-rw-r-- 1 root root 102598 Feb 17  2017 eurotext.tif
-rw-rw-r-- 1 root root   1219 Feb 17  2017 FILES
-rw-rw-r-- 1 root root 399613 Feb 17  2017 hebrew-nikud-genesis-1-2.png
-rw-rw-r-- 1 root root 193942 Feb 17  2017 hebrew.png
-rw-rw-r-- 1 root root 444141 Feb 17  2017 hebtypo.jpg
-rw-r--r-- 1 root root  12639 Oct 24 17:27 Makefile
-rw-rw-r-- 1 root root    219 Feb 17  2017 Makefile.am
-rw-r--r-- 1 root root  12641 Oct 24 17:26 Makefile.in
-rw-rw-r-- 1 root root  38668 Feb 17  2017 phototest.tif
-rw-rw-r-- 1 root root   1687 Feb 17  2017 README
-rwxrwxr-x 1 root root   1729 Feb 17  2017 reorgdata.sh
drwxrwxr-x 2 root root    140 Feb 17  2017 reports
-rw-r--r-- 1 root root    287 Oct 24 17:35 result.txt
-rwxrwxr-x 1 root root   4148 Feb 17  2017 runalltests.sh
-rwxrwxr-x 1 root root   2060 Feb 17  2017 runtestset.sh
// 查看结果
[root@kubernetes testing]# cat result.txt 
This is a lot of 12 point text to test the
ocr code and see if it works on all types
of file format.

The quick brown dog jumped over the
lazy fox. The quick brown dog jumped
over the lazy fox. The quick brown dog
jumped over the lazy fox. The quick
brown dog jumped over the lazy fox.

// 在线安装pytesseract(请参考python3.6.2)
[root@kubernetes testing]# pip3 install pytesseract

// 离线source方式安装pytesseract
// 此网站下载压缩包文件 https://pypi.org/project/pytesseract/#files
[root@kubernetes opt]# pip install pytesseract-0.3.0.tar.gz
// 或者解压执行脚本安装
[root@kubernetes opt]# tar zxf pytesseract-0.3.0.tar.gz
[root@kubernetes opt]# cd pytesseract-0.3.0
[root@kubernetes opt]# python3 setup.py install               // 或 ./configure --> make && make install
~~~