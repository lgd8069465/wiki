[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

在此感谢 [@mongodb](https://www.mongodb.com/)

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0

[mongodb-linux-x86_64-rhel70-4.0.12.tgz](https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-4.0.12.tgz)

~~~
mysql         -> databases -> tables      -> rows       -> columns   ->index   -> primary key
mongodb       -> databases -> collections -> documents  -> fields    ->index   -> primary key

// 创建目录
[root@docker opt]# mkdir -p /home/mongodb-4.0.12/db
[root@docker opt]# mkdir -p /home/mongodb-4.0.12/logs
// 授权
[root@docker opt]# chmod -R 777 /home/mongodb-4.0.12/
// 添加用户
[root@docker opt]# adduser mongodb -p mongodb
// 更改用户和组
[root@docker opt]# chown -R mongodb:mongodb /home/mongodb-4.0.12
// 解压
[root@docker opt]# tar zxf mongodb-linux-x86_64-rhel70-4.0.12.tgz
[root@docker opt]# cd /opt/mongodb-linux-x86_64-rhel70-4.0.12/bin
// 配置文件
[root@docker bin]# vi mongodb.conf
dbpath = /home/mongodb-4.0.12/db
logpath = /home/mongodb-4.0.12/logs/mongodb.log
port = 27017
fork = true                                                            # 后台运行
logappend = true                                                       # 开启系统日志
auth = false                                                           # 关闭认证
bind_ip= 0.0.0.0                                                       # 监听ip
// 启动
[root@docker bin]# ./mongod -f mongodb.conf
// 进入
[root@docker bin]# ./mongo
[root@docker bin]#
[root@docker bin]# ps -ef | grep mongo
[root@docker bin]# kill -15 17758
// 以service的形式管理mongodb
[root@docker system]# cd /lib/systemd/system
[root@docker system]# vi mongodb.service
[Unit]
Description=mongodb
After=network.target remote-fs.target nss-lookup.target
[Service]
Type=forking
ExecStart=/opt/mongodb-linux-x86_64-rhel70-4.0.12/bin/mongod --config /opt/mongodb-linux-x86_64-rhel70-4.0.12/bin/mongodb.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/opt/mongodb-linux-x86_64-rhel70-4.0.12/bin/mongod --shutdown --config /opt/mongodb-linux-x86_64-rhel70-4.0.12/bin/mongodb.conf
PrivateTmp=true
[Install]
WantedBy=multi-user.target

// 或者
[Unit]
Description=mongodb
After=network.target remote-fs.target nss-lookup.target
[Service]
Environment="MONGODB=/opt/mongodb-linux-x86_64-rhel70-4.0.12/bin"
Type=forking
ExecStart=/bin/bash -c "${MONGODB}/mongod --config ${MONGODB}/mongodb.conf"
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/bash -c "${MONGODB}/mongod --shutdown --config ${MONGODB}/mongodb.conf"
PrivateTmp=true
[Install]
WantedBy=multi-user.target
[root@docker system]#
// 重载
[root@docker system]# systemctl daemon-reload
[root@docker system]# systemctl status mongodb
[root@docker system]# systemctl start mongodb
[root@docker system]# systemctl enable mongodb
[root@docker bin]# ./mongo 192.168.192.128:27017
> db.getSiblingDB('admin').runCommand( { setParameter: 1, failIndexKeyTooLong: false } );      # 开启索引支持
> use admin;                                       
> db.createUser({user:"admin", pwd:"qazwsx",roles:[{role:"userAdminAnyDatabase",db:"admin"},{role:"readWriteAnyDatabase",db:"admin"}]});
> db.system.users.find();
> use sampledb;
> db.createUser({user: "wiki", pwd: "qazwsx", roles: [{ role: "dbOwner", db: "sampledb" }]});
[root@docker opt]#
[root@docker opt]# systemctl stop mongodb
[root@docker opt]# vi mongodb.conf 
auth = true                                                              # 开启认证
[root@docker opt]# systemctl start mongodb
[root@docker bin]# ./mongo 192.168.192.128:27017 
> use sampledb;
> db.auth('wiki','qazwsx');
> 1
[root@docker opt]#
// 逻辑备份还原
[root@docker opt]# ./mongorestore -h 192.168.192.128:27017 -d sampledb --dir=/opt/sampledb -u wiki -p qazwsx;
[root@docker opt]# ./mongodump -h 192.168.192.128:27017 -d sampledb --dir=/opt/sampledb -u wiki -p qazwsx;
// 压缩逻辑备份还原(推荐)
[root@docker opt]# ./mongorestore -h 192.168.192.128:27017 -d sampledb --gzip --archive=/opt/sampledb.gz -u wiki -p qazwsx;
[root@docker opt]# ./mongodump -h 192.168.192.128:27017 -d sampledb --gzip --archive=/opt/sampledb.gz -u wiki -p qazwsx;
~~~