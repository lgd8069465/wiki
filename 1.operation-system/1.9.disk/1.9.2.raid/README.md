[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0

| 模式        | 最低磁盘个数 | 最大使用率          | 概要               | 读快 | 写快         |
| ----------- | ------------ | ------------------- | ------------------ | ---- | ------------ |
| raid0       | 2            | 1                   | 便宜快速危险       | yes  | yes          |
| raid1       | 2            | 0.5                 | 高速读简单安全     | yes  | yes          |
| raid5       | 3            | (n-1)/n(一块热备盘) | 安全(速度)成本折中 | yes  | 依赖最慢的盘 |
| raid6       | 4            | (n-2)/n(两块热备盘) | 安全(速度)成本折中 | yes  | 依赖最慢的盘 |
| raid10(1+0) | 4            | 0.5                 | 昂贵高速安全       | yes  | yes          |

[Windows 上的 RAID 配置](https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/WindowsGuide/raid-config.html) 

[create volume raid](https://docs.microsoft.com/zh-cn/windows-server/administration/windows-commands/create-volume-raid) 

| 模式  | windows关键字 | windows名称    |
| ----- | ------------- | -------------- |
| raid0 | stripe        | 带区卷(条带卷) |
| raid1 | mirror        | 镜像卷         |
| raid5 | raid          | RAID-5卷       |

```
[root@kibana ~]# lvcreate --type raid0 -i 2 -L 20G -n lvr0 lvm_01 /dev/sdb /dev/sdc
[root@kibana ~]# lvcreate --type raid1 -m 1 -L 20G -n lvr1 lvm_01 /dev/sdb /dev/sdc
[root@kibana ~]# lvcreate --type raid5 -i 2 -L 20G -n lvr5 lvm_01 /dev/sdb /dev/sdc /dev/sdd
[root@kibana ~]# lvcreate --type raid6 -i 3 -L 20G -n lvr6 lvm_01 /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf
[root@kibana ~]# lvcreate --type raid10 --mirrors 1 --stripes 2 -L 20G --name lvr10 lvm_01 /dev/sdb /dev/sdc /dev/sdd /dev/sde
```

