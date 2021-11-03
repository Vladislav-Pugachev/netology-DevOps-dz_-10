### 2.
>Нельзя, так как права хранятся в метаинформации. Метаинфа в inode. А так как он один на обе ссылки, то и права одни

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
### 5.
```buildoutcfg
vagrant@vagrant:~$ sudo sfdisk -d /dev/sdc > sdc-tables.txt
vagrant@vagrant:~$ sudo sfdisk /dev/sdb < sdc-tables.txt
Checking that no-one is using this disk right now ... OK

Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Created a new DOS disklabel with disk identifier 0xf05761ca.
/dev/sdb1: Created a new partition 1 of type 'Extended' and of size 2.5 GiB.
/dev/sdb2: Created a new partition 5 of type 'Linux' and of size 2 GiB.
/dev/sdb6: Created a new partition 6 of type 'Linux' and of size 509 MiB.
/dev/sdb7: Done.

New situation:
Disklabel type: dos
Disk identifier: 0xf05761ca

Device     Boot   Start     End Sectors  Size Id Type
/dev/sdb1          2048 5242879 5240832  2.5G  5 Extended
/dev/sdb5          4096 4198399 4194304    2G 83 Linux
/dev/sdb6       4200448 5242879 1042432  509M 83 Linux

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
├─sdb1                 8:17   0    1K  0 part
├─sdb5                 8:21   0    2G  0 part
└─sdb6                 8:22   0  509M  0 part
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    1K  0 part
├─sdc5                 8:37   0    2G  0 part
└─sdc6                 8:38   0  509M  0 part
```

### 6.

```buildoutcfg
root@vagrant:~# mdadm --create --verbose /dev/md1 --level=1 --raid-devices=2 /dev/sdc5 /dev/sdb5
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
mdadm: size set to 2094080K
Continue creating array?
Continue creating array? (y/n) y
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md1 started.
root@vagrant:~# cat /proc/mdstat
Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]
md0 : active raid0 sdb6[1] sdc6[0]
      1038336 blocks super 1.2 512k chunks

md1 : active raid1 sdb5[1] sdc5[0]
      2094080 blocks super 1.2 [2/2] [UU]

unused devices: <none>
root@vagrant:~#
```

### 7.

```buildoutcfg
root@vagrant:~# mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdc6 /dev/sdb6
mdadm: chunk size defaults to 512K
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
```

### 8.

```buildoutcfg
root@vagrant:~# pvcreate /dev/md0
  Physical volume "/dev/md0" successfully created.
root@vagrant:~# pvcreate /dev/md1
  Physical volume "/dev/md1" successfully created.
root@vagrant:~#
```

### 9.

```buildoutcfg
root@vagrant:~# vgcreate netology /dev/md0 /dev/md1
  Volume group "netology" successfully created
root@vagrant:~# pvs
  PV         VG        Fmt  Attr PSize    PFree
  /dev/md0   netology  lvm2 a--  1012.00m 1012.00m
  /dev/md1   netology  lvm2 a--    <2.00g   <2.00g
  /dev/sda5  vgvagrant lvm2 a--   <63.50g       0
root@vagrant:~#
```

### 10.

```buildoutcfg
root@vagrant:~# lvcreate -n lv1 -L 100M netology /dev/md0
  Logical volume "lv1" created.
```

### 11.

```buildoutcfg
root@vagrant:~# mkfs.ext4 /dev/netology/lv1
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 25600 4k blocks and 25600 inodes

Allocating group tables: done
Writing inode tables: done
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done
```

### 12.

```buildoutcfg
root@vagrant:~# mkdir /tmp/new
root@vagrant:~# mount /dev/netology/lv1 /tmp/new
```

### 14.
```buildoutcfg
root@vagrant:~# lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    1K  0 part
├─sdb5                 8:21   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
└─sdb6                 8:22   0  509M  0 part
  └─md0                9:0    0 1014M  0 raid0
    └─netology-lv1   253:2    0  100M  0 lvm   /tmp/new
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    1K  0 part
├─sdc5                 8:37   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
└─sdc6                 8:38   0  509M  0 part
  └─md0                9:0    0 1014M  0 raid0
    └─netology-lv1   253:2    0  100M  0 lvm   /tmp/new
```

### 16.
```buildoutcfg
root@vagrant:~# pvmove /dev/md0 /dev/md1
  /dev/md0: Moved: 16.00%
  /dev/md0: Moved: 100.00%
root@vagrant:~# lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    1K  0 part
├─sdb5                 8:21   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
│   └─netology-lv1   253:2    0  100M  0 lvm   /tmp/new
└─sdb6                 8:22   0  509M  0 part
  └─md0                9:0    0 1014M  0 raid0
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    1K  0 part
├─sdc5                 8:37   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
│   └─netology-lv1   253:2    0  100M  0 lvm   /tmp/new
└─sdc6                 8:38   0  509M  0 part
  └─md0                9:0    0 1014M  0 raid0
root@vagrant:~#
```

### 18.
```
[31424.378919] md/raid1:md1: Disk failure on sdc5, disabling device.
               md/raid1:md1: Operation continuing on 1 devices.
root@vagrant:~#
```

### 19.
```buildoutcfg
root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0
```