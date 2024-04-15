# multi-volume-backup

中文介绍 | [English](./README.md)

一个基于 `tar` 的智能多卷备份脚本。

## 动机

我的 NAS （OpenMediaVault）中存放了大量的文件，为了低成本地保证数据安全，我买了数个小容量的二手机械硬盘做冷备份（以后还会购入磁带机做一样的事情），因为硬盘笼位置有限，我一次只能通过 USB 硬盘底座接入一块硬盘，于是如何把大量的文件备份到数个小容量的硬盘（类似移动硬盘）中便成为了问题，我的要求是：

- 支持自定义卷大小的多卷备份
- 支持自动检测磁盘容量，如果可用空间大于卷大小，则自动在当前磁盘存放下一卷
- 当磁盘可用空间不足时，提示并等待我更换磁盘
- 支持增量备份
- 写入备份卷的同时**在空中流式**计算校验值，每一卷完成时写入对应的文件，而不是需要我在备份完成后再次读取硬盘数据计算校验值（**操作非常慢**）

在经过一翻搜寻后我没有发现网上有现成的方案，于是自己基于 `tar` 编写了一个脚本完成上述功能。

## 用法

```bash
Usage:
  multi-volume-backup backup <source path> <dest path with volume name prefix> <volume size> <snar file path>
  multi-volume-backup restore <first volume path> <dest path> <strip path level>

Note:
  - snar file is used to save filelist to achieve incremental backup
  - strip path level indicates how many levels of paths need to be deleted when extract

Example:
  multi-volume-backup backup /data /mnt/bak/full_backup- 100G /data.snar
  multi-volume-backup restore /mnt/bak/full_backup-1.tar /data 1
```

