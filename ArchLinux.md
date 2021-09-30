# Vanilla Arch Installation Guide

1. Check the internet connection
	1. Check IP info
	```bash
	ip addr
	```
	2. Check connection to the internet
	```bash
	ping gnu.org
	```
	3. Connect WiFi adapter to the network using ```iwctl```
		1. Open the tool
		```bash
		iwctl	
		```	
		2. Show list of wifi adapters 
		```bash
		device list
		```
		3. Scan with selected wifi adapter
		```bash
		station ADAPTER_NAME scan 	
		```
		4. See the list of networks
		```bash
		station ADAPTER_NAME get-networks
		```
		5. Connect to networkd
		```bash
		station ADAPTER_NAME connect NETWORK_NAME
		```
		6. Enter password for the selected network
		7. Exit ```iwctl``` by ````exit```` command
2. Sync network/time protocol
```bash
timedatectl set-ntp true
```
3. Find fastest mirror to download packages
```bash
reflector -c Istanbul -a 12 --sort rate --save /etc/pacman.d/mirrorlist
```
4. Sync servers
```bash
pacman -Syy
```
5. Show disks info
```bash
lsblk
```
6. Partition disk using ```cfdisk``` for UEFI system
	1. Open desired disk
	```bash
	cfdisk /dev/DISK_LABEL
	```
	2. Chose ```gpt``` as partition table
	3. Allocate ```250M``` to ````efi system```` for efi partition
	4. Allocate the rest of the disk as ```Linux file system``` for root partition
7. Partitions
	1. Format ```efi``` partition
	```bash
	mkfs.fat -F32 /dev/sd*
	```
	2. Format ```root``` partition
	```bash
	mkfs.ext4 /dev/sd*
	```
8. Mount partitions
	1. Mount ```root``` partition
	```bash
	mount /dev/sd* /mnt
	```
	2. Mount ```efi``` partition
	```bsah
	mkdir -p /mnt/boot/efi
	mount /dev/sd* /mnt/boot/efi
	```
	3. Check the mounted partitions using ```lsblk```
9. Install base packages
```bash
pacstrap /mnt base linux linux-firmware vim git intel-ucode
```
10. Generate filesystem table
```bash
genfstab -U /mnt >> /mnt/etc/fstab
# for check it
cat /mnt/etc/fstab
```
11. Switch to installer from ISO
```bash
arch-chroot /mnt
```
12. Create ```swap``` file in ```efi``` system
```bash
# create 2GB swapfile
dd if=/dev/zero of=/swapfile bs=1M count=2048 status=progress
# change swapfile permission
chmod 600 /swapfile
# make swap
mkswap /swapfile
# active swap
swapon /swapfile 
# add swapfile to fstab
vim /etc/fstab
/swapfile	none	swap	defaults	0	0
```
13. Set timezone
```bash
ln -s /usr/share/zoneinfo/Europe/Istanbul /etc/localtime
```
14. Sync system clock and hardware clock
```bash
hwclock --systohc
```
15. Generate locale and locale.conf
```bash
# edit locale file
# remve the # sign from line 'en_US.UTF-8'
vim /etc/locale.gen
# create locale.conf and add following lines to it
vim /etc/locale.conf
# LAN=en_US.UTF-8
```
16. Generate ```hostname``` file
```bash
# add host name only
vim /etc/hostname
```
17. Generate ```hosts``` file
```bash
# add following lines
# 127.0.0.1	localhost
# ::1		localhost
# 127.0.1.1	vm.localdomain	vm
vim /etc hosts
``` 
18. Set root user password using ```passwd```
19. Install other packages
```bash
pacman -S grub efibootmgr networkmanager network-manager-applet dialog wpa_supplicant mtools dosfstools reflector base-devel linux-headers bluez bluez-utils cups hplip bash-completion openssh rsync ntfs-3g 
```
20. Install ```grub```
```bash
grab-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-if=GRUB
```
21. Generate ```grub``` configuration file
```bash
grub-mkconfig -o /boot/grub/grub.cfg
```
22. Enable services for next reboot internet connection
```bash
systemctl enable NetworkManager
systemctl enable bluetooth
systemctl enable org.cups.cupsd
```
23. Create user
```bash
useradd -mG wheel USERNAME 
passwd USERNAME
# delete # from first wheel group using 'visudo'
```
24. Exit the installation and reboot
```bsah
exit
uname -a
reboot
```
