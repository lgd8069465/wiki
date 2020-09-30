[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0

[NTFS vs FAT vs exFAT](https://www.ntfs.com/ntfs_vs_fat.htm) 

| 文件系统 | 最大单文件                 | 最大分区               | 操作系统          |
| -------- | -------------------------- | ---------------------- | ----------------- |
| APFS     | 2 63 bytes                 |                        | macOS 10.13       |
| Ext4     | 16TB                       | 1EB                    | rhel6             |
| XFS      | 16TB                       | 8EB                    | rhel7             |
| Brtfs    | 16EB(8EB due to Linux VFS) | 16EB                   | Fedora 33         |
| ZFS      | 16EB                       | 16EB                   | Solaris/Unix      |
| NTFS     | 16TB                       | 16TB                   | Windows7          |
| exFAT    | 16EB(理论值)               | 128PB(理论值)          | WinCE 6/Vista SP1 |
| FAT32    | 4GB                        | Win2000 32GB,WinXP 2TB | Win2000/WinXP     |
| FAT16    | 2GB                        | 2GB                    | Windows/DOS       |

| 多盘合一 | 命令                       |
| -------- | -------------------------- |
| LVM      | pvcreate,vgcreate,lvcreate |
| RAID     | mdadm                      |

| 分区格式 | 分区工具 | 容量  | 最大主区 | 引导启动 |
| -------- | -------- | ----- | -------- | -------- |
| GPT      | parted   | >2TB  |          | UEFI     |
| MBR      | fdisk    | <=2TB | 4        | BIOS     |

