### 3.
```buildoutcfg
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk
sdc                    8:32   0  2.5G  0 disk
```

### 4.
```buildoutcfg
vagrant@vagrant:~$ sudo fdisk /dev/sdc

Welcome to fdisk (util-linux 2.34).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): e
Partition number (1-4, default 1): 1
First sector (2048-5242879, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879):

Created a new partition 1 of type 'Extended' and of size 2.5 GiB.

Command (m for help): n
All space for primary partitions is in use.
Adding logical partition 5
First sector (4096-5242879, default 4096):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4096-5242879, default 5242879): +2G

Created a new partition 5 of type 'Linux' and of size 2 GiB.

Command (m for help): n
All space for primary partitions is in use.
Adding logical partition 6
First sector (4200448-5242879, default 4200448):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4200448-5242879, default 5242879):

Created a new partition 6 of type 'Linux' and of size 509 MiB.

Command (m for help): p
Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xf05761ca

Device     Boot   Start     End Sectors  Size Id Type
/dev/sdc1          2048 5242879 5240832  2.5G  5 Extended
/dev/sdc5          4096 4198399 4194304    2G 83 Linux
/dev/sdc6       4200448 5242879 1042432  509M 83 Linux

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    1K  0 part
├─sdc5                 8:37   0    2G  0 part
└─sdc6                 8:38   0  509M  0 part
vagrant@vagrant:~$
```
