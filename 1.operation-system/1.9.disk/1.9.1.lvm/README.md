| 命令 | pv        | vg        | lv                |
| ---- | --------- | --------- | ----------------- |
| 扫描 | pvscan    | vgscan    | lvscan            |
| 创建 | pvcreate  | vgcreate  | lvcreate          |
| 显示 | pvdisplay | vgdisplay | lvdisplay         |
| 移除 | pvremove  | vgremove  | lvremove          |
| 扩展 | pvresize  | vgextend  | lvextend/lvresize |
| 缩小 |           | vgreduce  | lvreduce/lvresize |

```
[root@kibana ~]# lvcreate --type raid0 -i 2 -L 20G -n lvr0 lvm_01 /dev/sdb /dev/sdc
[root@kibana ~]# lvcreate --type raid1 -m 1 -L 20G -n lvr1 lvm_01 /dev/sdb /dev/sdc
[root@kibana ~]# lvcreate --type raid5 -i 2 -L 20G -n lvr5 lvm_01 /dev/sdb /dev/sdc /dev/sdd
[root@kibana ~]# lvcreate --type raid6 -i 3 -L 20G -n lvr6 lvm_01 /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf
[root@kibana ~]# lvcreate --type raid10 --mirrors 1 --stripes 2 -L 20G --name lvr10 lvm_01 /dev/sdb /dev/sdc /dev/sdd /dev/sde
```

