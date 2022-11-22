### 禁用 reflector
`systemctl stop reflector.service`
### 验证是否为 UEFI 模式
`ls /sys/firmware/efi/efivars`
### 连接网络
```bash
# 无线连接
iwctl                           #执行iwctl命令，进入交互式命令行
device list                     #列出设备名，比如无线网卡看到叫 wlan0
station wlan0 scan              #扫描网络
station wlan0 get-networks      #列出网络 比如想连接YOUR-WIRELESS-NAME这个无线
station wlan0 connect YOUR-WIRELESS-NAME #进行连接 输入密码即可
exit                            #成功后exit退出

# 有线连接
ip link  #列出网络接口信息，如不能联网的设备叫wlan0
ip link set wlan0 up #比如无线网卡看到叫 wlan0
```
### 更新系统时钟
```
timedatectl set-ntp true    #将系统时间与网络时间进行同步
timedatectl status          #检查服务状态
```
### 分区
```
lsblk                       #显示分区情况 找到你想安装的磁盘名称
parted /dev/sdx             #执行parted，进入交互式命令行，进行磁盘类型变更
(parted)mktable             #输入mktable
New disk label type? gpt    #输入gpt 将磁盘类型转换为gpt 如磁盘有数据会警告，输入yes即可
quit                        #最后quit退出parted命令行交互
```	

### 格式化
```
mkfs.ext4  /dev/sdax            #格式化根目录和home目录的两个分区
mkfs.vfat  /dev/sdax            #格式化efi分区
```
### 挂载
```
mount /dev/sdax  /mnt
mkdir /mnt/efi     #创建efi目录
mount /dev/sdax /mnt/efi
mkdir /mnt/home    #创建home目录
mount /dev/sdax /mnt/home
```
### edit /etc/pacman.d/mirrorlist
```
nano /etc/pacman.d/mirrorlist
# mirrorlist
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
Server = https://mirror.archlinux.tw/ArchLinux/$repo/os/$arch   #东亚地区:中华民国
Server = https://mirror.0xem.ma/arch/$repo/os/$arch    #北美洲地区:加拿大
Server = https://mirror.aktkn.sg/archlinux/$repo/os/$arch    #东南亚地区:新加坡
Server = https://archlinux.uk.mirror.allworldit.com/archlinux/$repo/os/$arch    #欧洲地区:英国
Server = https://mirrors.cat.net/archlinux/$repo/os/$arch    #东亚地区:日本


```
### 安装系统
```
pacstrap /mnt base base-devel linux linux-headers linux-firmware sof-firmware  
# 安装其他所需包
pacstrap /mnt bluez bluez-utils networkmanager xf86-input-libinput
```
### 生成 fstab 文件
`genfstab -U /mnt >> /mnt/etc/fstab`
`cat /mnt/etc/fstab`
### change root
`arch-chroot /mnt`
### 时区设置
```
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```
### 设置 Locale 进行本地化
```
nano /etc/locale.gen

# 去掉 en_US.UTF-8 所在行以及 zh_CN.UTF-8 所在行的注释符号（#）
locale-gen
echo 'LANG=en_US.UTF-8'  > /etc/locale.conf
```
### 设置主机名
```
nano /etc/hostname

arch
```
### edit hosts
```
nano /etc/hosts

####
127.0.0.1   localhost
::1         localhost
127.0.1.1   arch
```
### 为 root 用户设置密码
```
passwd root
```
### 安装微码
```
pacman -S intel-ucode   #Intel
pacman -S amd-ucode     #AMD
```
### 安装引导程序
```
pacman -S grub efibootmgr   #grub是启动引导器，efibootmgr被 grub 脚本用来将启动项写入 NVRAM。
grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB


````
### 生成 GRUB 所需的配置文件
```
grub-mkconfig -o /boot/grub/grub.cfg

grub-install --target=x86_64-efi --efi-directory=/efi --removable
grub-mkconfig -o /boot/grub/grub.cfg
```
### 设置需要开机启动的服务
```

```
### 完成安装
```
exit                # 退回安装环境#
umount -R  /mnt     # 卸载新分区
reboot              # 重启

```
