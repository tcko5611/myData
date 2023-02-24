```bash
$> lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda           8:0    1  7.6G  0 disk 
└─sda1        8:1    1  7.6G  0 part /media/pi/ESD-USB
sdc           8:33   1  7.4G  0 disk 
└─sdc1        8:33   1  7.4G  0 part /media/pi/disk
mmcblk0     179:0    0 14.9G  0 disk 
├─mmcblk0p1 179:1    0   63M  0 part /boot
└─mmcblk0p2 179:2    0 14.8G  0 part /
$> umount /media/pi/disk
$> lsblk
$> sudo dd if=ubuntu-16.10-desktop-amd64.iso of=/dev/sdc bs=1M
/dev/sdc bs=1M
1520+0 records in
1520+0 records out
1593835520 bytes (1.6 GB) copied, 493.732 s, 3.2 MB/s
```
DELL E7740 use F12 key to select init device(usb or sd card)