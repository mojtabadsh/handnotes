### Extend LVM (XFS) /root - 

This Scenario add a new hard disk Drive and extend lvm with new hard disk 

- List and check the partition we have created using fdisk.

```bash
$ fdisk -l 
Disk /dev/sda: 42.9 GB, 42949672960 bytes, 83886080 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x0001e878

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    83886079    40893440   8e  Linux LVM

Disk /dev/mapper/centos-root: 37.6 GB, 37576769536 bytes, 73392128 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/centos-swap: 4294 MB, 4294967296 bytes, 8388608 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/sdb: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

ext, create new **PV** (Physical Volume) using following command.

```bash
$ pvcreate /dev/sdb 
  Physical volume "/dev/sdb" successfully created.
```

Verify the pv using below command.

```bash
$ pvs
  PV         VG     Fmt  Attr PSize   PFree  
  /dev/sda2  centos lvm2 a--  <39.00g      0 
  /dev/sdb          lvm2 ---  100.00g 100.00g
```

Add this pv to **centos** vg to extend the size of a volume group to get more space for expanding **lv**.

```bash
$ vgextend centos /dev/sdb
  Volume group "centos" successfully extended
```

Let us check the size of a Volume Group now using.

```bash
$ vgs
  VG     #PV #LV #SN Attr   VSize   VFree
  centos   2   2   0 wz--n- 138.99g    0 
```

We can even see which **PV** are used to create particular Volume group using.

```bash
$ pvscan
  PV /dev/sda2   VG centos          lvm2 [<39.00 GiB / 0    free]
  PV /dev/sdb    VG centos          lvm2 [<100.00 GiB / <100.00 GiB free]
  Total: 2 [138.99 GiB] / in use: 2 [138.99 GiB] / in no VG: 0 [0   ]
```

For getting the available Physical Extend size run.

```bash
$ vgdisplay
  --- Volume group ---
  VG Name               centos
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               138.99 GiB
  PE Size               4.00 MiB
  Total PE              35582
  Alloc PE / Size       9983 / <39.00 GiB
  Free  PE / Size       25599 / <100.00 GiB
  VG UUID               sNRP5q-77QM-S6ww-QRxr-LS0q-opm9-bxzXzB
```

There are **25599** free PE available = **100GB** Free space available. So we can expand our logical volume up-to **100GB** more. Let us use the PE size to extend.

Use **+** to add the more space. After Extending, we need to re-size the file-system using.

```bash
$ lvextend -l +25599 /dev/centos/root
  Size of logical volume centos/root changed from <35.00 GiB (8959 extents) to 134.99 GiB (34558 extents).
  Logical volume centos/root successfully resized.
```

- **Applying changes without erasing the data or turning off the server**

```bash
$ xfs_growfs /dev/centos/root
```

- **Display disk changes**

```bash
$ vgdisplay
  --- Volume group ---
  VG Name               centos
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  5
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               138.99 GiB
  PE Size               4.00 MiB
  Total PE              35582
  Alloc PE / Size       35582 / 138.99 GiB
  Free  PE / Size       0 / 0   
  VG UUID               sNRP5q-77QM-S6ww-QRxr-LS0q-opm9-bxzXzB
```

