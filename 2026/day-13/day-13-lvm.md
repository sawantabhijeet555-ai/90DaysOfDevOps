## Task 1: Check Current Storage

ubuntu@ip-172-31-14-118:~$ lsblk
NAME     MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0      7:0    0 28.2M  1 loop /snap/amazon-ssm-agent/13009
loop1      7:1    0   74M  1 loop /snap/core22/2411
loop2      7:2    0 48.4M  1 loop /snap/snapd/26382
loop3      7:3    0 49.3M  1 loop /snap/snapd/26865
xvda     202:0    0    8G  0 disk 
├─xvda1  202:1    0  6.9G  0 part /
├─xvda13 202:13   0 1023M  0 part /boot
├─xvda14 202:14   0    4M  0 part 
└─xvda15 202:15   0  106M  0 part /boot/efi

ubuntu@ip-172-31-14-118:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       6.7G  2.4G  4.3G  37% /
tmpfs           476M     0  476M   0% /dev/shm
tmpfs           191M  928K  190M   1% /run
tmpfs           476M     0  476M   0% /tmp
none            1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
none            1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service
/dev/xvda13     989M   95M  827M  11% /boot
/dev/xvda15     105M  6.3M   99M   7% /boot/efi
none            1.0M     0  1.0M   0% /run/credentials/systemd-networkd.service
none            1.0M     0  1.0M   0% /run/credentials/getty@tty1.service
none            1.0M     0  1.0M   0% /run/credentials/serial-getty@ttyS0.service
tmpfs            96M  8.0K   96M   1% /run/user/1000


ubuntu@ip-172-31-14-118:~$ lvm
  WARNING: Running as a non-root user. Functionality may be unavailable.
lvm> pvcreate /dev/xvdf /dev/xvdg
  /run/lock/lvm/P_global:aux: open failed: Permission denied
lvm> exit
  Exiting.
ubuntu@ip-172-31-14-118:~$ sudo su
root@ip-172-31-14-118:/home/ubuntu# lvm


## Task 2: Create Physical Volume

lvm> pvcreate /dev/xvdf /dev/xvdg

  Physical volume "/dev/xvdf" successfully created.
  Physical volume "/dev/xvdg" successfully created.

lvm> pvcreate /dev/xvdh
  Physical volume "/dev/xvdh" successfully created.

lvm> pvs
  PV         VG Fmt  Attr PSize  PFree 
  /dev/xvdf     lvm2 ---  10.00g 10.00g
  /dev/xvdg     lvm2 ---  12.00g 12.00g
  /dev/xvdh     lvm2 ---  14.00g 14.00g

## Task 3: Create Volume Group

lvm> vgcreate tws_vg /dev/xvdf /dev/xvdg
  Volume group "tws_vg" successfully created

lvm> vgs
  VG     #PV #LV #SN Attr   VSize  VFree 
  tws_vg   2   0   0 wz--n- 21.99g 21.99g

## Task 4: Create Logical Volume

lvm> lvcreate -L 10G -n tws_lv tws_vg
  Logical volume "tws_lv" created.

lvm> lvs
  LV     VG     Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  tws_lv tws_vg -wi-a----- 10.00g   


root@ip-172-31-14-118:/home/ubuntu# lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0             7:0    0 28.2M  1 loop /snap/amazon-ssm-agent/13009
loop1             7:1    0   74M  1 loop /snap/core22/2411
loop2             7:2    0 48.4M  1 loop /snap/snapd/26382
loop3             7:3    0 49.3M  1 loop /snap/snapd/26865
xvda            202:0    0    8G  0 disk 
├─xvda1         202:1    0  6.9G  0 part /
├─xvda13        202:13   0 1023M  0 part /boot
├─xvda14        202:14   0    4M  0 part 
└─xvda15        202:15   0  106M  0 part /boot/efi
xvdf            202:80   0   10G  0 disk 
xvdg            202:96   0   12G  0 disk 
└─tws_vg-tws_lv 252:0    0   10G  0 lvm  
xvdh            202:112  0   14G  0 disk 

## Task 5: Format and Mount

root@ip-172-31-14-118:/home/ubuntu# mkdir /mnt/tws_lv_mount
root@ip-172-31-14-118:/home/ubuntu# mkfs.ext4 /dev/tws_vg/tws_lv
mke2fs 1.47.2 (1-Jan-2025)
Creating filesystem with 2621440 4k blocks and 655360 inodes
Filesystem UUID: e1564806-fa0f-4f8f-9af0-ef7d2c578e9c
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done 

root@ip-172-31-14-118:/home/ubuntu# mount /dev/tws_vg/tws_lv /mnt/tws_lv_mount
root@ip-172-31-14-118:/home/ubuntu# df -h
Filesystem                 Size  Used Avail Use% Mounted on
/dev/root                  6.7G  2.4G  4.3G  37% /
tmpfs                      476M     0  476M   0% /dev/shm
tmpfs                      191M  936K  190M   1% /run
tmpfs                      476M     0  476M   0% /tmp
none                       1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
none                       1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service
/dev/xvda13                989M   95M  827M  11% /boot
/dev/xvda15                105M  6.3M   99M   7% /boot/efi
none                       1.0M     0  1.0M   0% /run/credentials/systemd-networkd.service
none                       1.0M     0  1.0M   0% /run/credentials/getty@tty1.service
none                       1.0M     0  1.0M   0% /run/credentials/serial-getty@ttyS0.service
tmpfs                       96M  8.0K   96M   1% /run/user/1000
/dev/mapper/tws_vg-tws_lv  9.8G  2.1M  9.3G   1% /mnt/tws_lv_mount


## Task 6: Extend the Volume

root@ip-172-31-14-118:/home/ubuntu# lvm
lvm> lvextend -L +5G -n tws_lv /dev/tws_vg/tws_lv
  Volume group "tws_lv" not found
  Cannot process volume group tws_lv
lvm> lvs
  LV     VG     Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  tws_lv tws_vg -wi-ao---- 10.00g                                                    
lvm> lvextend -L +5G -n  /dev/tws_vg/tws_lv
  Size of logical volume tws_vg/tws_lv changed from 10.00 GiB (2560 extents) to 15.00 GiB (3840 extents).
  Logical volume tws_vg/tws_lv successfully resized.
lvm> exit
