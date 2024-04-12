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

## Usage
