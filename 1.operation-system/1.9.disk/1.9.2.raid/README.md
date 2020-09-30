[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0

| 模式        | 最低磁盘个数 | 最大使用率      |
| ----------- | ------------ | --------------- |
| raid0       | 1            | 1               |
| raid1       | 2            | 0.5             |
| raid5       | 3            | 2/3(一块热备盘) |
| raid6       | 4            | 2/4(两块热备盘) |
| raid10(1+0) | 4            | 0.5             |

[Windows 上的 RAID 配置](https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/WindowsGuide/raid-config.html) 

[create volume raid](https://docs.microsoft.com/zh-cn/windows-server/administration/windows-commands/create-volume-raid) 

| 模式  | windows关键字 | windows名称    |
| ----- | ------------- | -------------- |
| raid0 | stripe        | 带区卷(条带卷) |
| raid1 | mirror        | 镜像卷         |
| raid5 | raid          | RAID-5卷       |

