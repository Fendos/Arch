

timedatectl set-ntp true

timedatectl status

fdisk -l


### Create Filesystem

```
mkfs.ext4 /dev/sdX1
mkswap /dev/sdX2
swapon /dev/sdX2






mount /dev/sdX1 /mnt



```



