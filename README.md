# multi-volume-backup

[中文介绍](./README_zh_CN.md) | English

A smart multi-volume backup script based on `tar`.

## Motivation

I have a large number of files stored in my NAS (OpenMediaVault). To ensure data safety at a low cost, I bought several small-capacity second-hand mechanical hard drives for cold backup (I plan to buy a tape drive for the same purpose in the future). Due to limited hard drive bay space, I can only connect one hard drive at a time via a USB hard drive dock. Therefore, how to backup a large number of files to several small-capacity hard drives (equivalent to portable hard drives) became a problem. My requirements are:

- Support for multi-volume backup with customizable volume size
- Automatic detection of disk capacity. If the available space is larger than the volume size, the next volume is automatically stored on the current disk
- When the disk's available space is insufficient, prompt and wait for me to change the disk
- Supports incremental backup.
- While writing the backup volume, **streamingly calculate** the checksum in the air. Write the corresponding file when each volume is completed, instead of requiring me to read the hard drive data again to calculate the checksum after the backup is completed (**operation is very slow**)

After some searching, I didn't find any ready-made solutions, so I wrote a script based on `tar` to accomplish the above functions.

## Pre Requisite

In addition to the common commands, the following commands are required:

- `mkfifo`
- `tee`

## Usage

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

