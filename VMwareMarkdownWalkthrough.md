VMware Markdown

# Install VMware in terminal for Linux(Deb):

```bash
wget https://download3.vmware.com/software/wkst/file/VMware-Workstation-Full-16.1.0-17198959.x86_64.bundle

chmod a+x VMware-Workstation-Full-16.1.0-17198959.x86_64.bundle

sudo ./VMware-Workstation-Full-16.1.0-17198959.x86_64.bundle 
```

# Download ISO Key : 
- Link to **DOWNLOADS PAGE**
- Scroll to **United States**:
- Pick **Mit.iso**:
- Pick **archlinux-2022.10.01-x86_64.iso**

# Enter Requested License key for VMware version:
Software key:

     4N216-LG14L-68ZJ2-03CA0-0HF0N
# Create an Arch Linux VM:
 - "Create New VM"
 - CHOOSE: Custom(Advanced)
 - Guest Operating System : Linux
    - Version : Other Linux 5.x and later kernal 64-bit
 - CHANGE NAME --> "Arch Linux"
 - Processors:
    - '#' of processors: **1**
    - '#' of core processors **2**
    - **Total: 2**
    ## Memory:
    - Specify 2GB (2048MB)
    ## Network Connection:
    - Keep Default
        - **Network address Translation**(NAT)
    ## I/O Controller types:
    ### SCSI Controller:
    - Default 
        - LSI LOGIC **(Recommended)**    
    ## Virtual Disk Type:
    - Default
        - SCSI **(Recommended)**
    CHOOSE: Create New VM Disk
    ### Disk Size:
    - increase to 20 GB
    - store on single file
    ## Finish
    
# Installation:

## Verify Signature:
    $ gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
## Boot the VM & Verify Boot method
     ls /sys/firmware/efi/efivars
## Connect to the internet
Set up a network connection in the live environment, go through the following steps.
    Ensure the network interface is enabled with: 
    
    ip link
## Test Connection
     ping archlinux.org
    ^C
## Update the system clock
In the live environment systemd-timesyncd is enabled by default and time will be synced automatically once a connection to the internet is established.
Ensure the system clock is accurate:

    timedatectl status

## Partition Disks:
    fdisk -l
    fdisk /dev/sda
    p
### First SDA:
    n (for creating new)
    p (primary)
    # Partion Number : 'enter'(default)
    # First Sector : 'enter' (default)
    # Second Sector: +500M
### Second SDA:
    n (for creating new)
    p (primary)
    # Partion Number : 'enter'(default)
    # First Sector : 'enter' (default)
    # Second Sector: 'enter' (default, will use the rest of the space)
### Exit
    Use 'w' to exit and save
## Mount the file systems
Mount the root volume to UEFI:

    mkfs.ext4 /dev/sda2 (format sda2)
    mount /dev/sda2 /mnt (mount sda2)
    mkfs.ext4 /dev/sda1 (format sda1)
    mount --mkdir /sda1 /mnt/boot (mount sda1)
   
## Install Pacstrap
    pacstrap -K /mnt base linux linux-firmware
## Configure System
    # genfstab -U /mnt >> /mnt/etc/fstab
## Change root
chroot into new system:

    arch-chroot/mnt 
### VIM
    pacman -S vim
### DO NOT CHANGE KEYBOARD ENV UNLESS YOU DO NOT HAVE A US LAYOUT
## Change/Set timezone
    ln -sf /usr/share/zoneinfo/Unitedstates/Chicago /etc/localtime
    hwclock --systohc
## Localization
Edit /etc/locale.gen and uncomment en_US.UTF-8 UTF-8 and other needed locales. Generate the locales:

    locale-gen
Create the locale.conf(5) file, and set the LANG variable accordingly:

    /etc/locale.conf
Set LANG equal to:

    en_US.UTF-8
## Configure Network
Create hostname file

    /etc/hostname ; Linux(Deb)64
## Set root password
    passwd
## Bootloader
Have had issues installing bootloader with grub-install:

    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

## REBOOT
Exit Chroot

    exit
### Optional
Unmount all partitions

```bash
umount -R /mnt
```

Restart the machine by typing
-Any partitions still mounted will beunmounted by systemd

    reboot

# INSTALL DESKTOP ENVIORNMENT
## Connection Check
    ping google.com
## Create users
```bash
useradd helaina -m -g wheel
useradd codi -m -g wheel
passwd codi
passwd helaina
```
## Sudo

Configure sudo: open file add under root
    
    helaina ALL=(ALL:ALL) ALL
    codi ALL=(ALL:ALL) ALL
## Network DCHP
Necessary step!!! Didn't know at first complicated my entire life!!

    pacman -S sudo dhcp dhcpcd
    systemctl enable dhcpcd
## Window Manager
Had a lot of trouble installing, nothing was working correctly. Picked fish and finally things started to work.

    pacman -S xorg herbstluftwm lightdm-gtk-greeter

(hit enter auto downloads all variations)
## Reboot
    reboot
## User Shell Change
    sudo pacman -S fish
    chsh -s /bin/fish

## SSH
### Install SSH
    sudo pacman -S openssh
### Utilization
```bash
sudo systemctl status sshd
sudo systemctl start sshd
sudo systemctl enable sshd
systemctl enable systemd-networkd.service
turn on vpn (linux user problems)
ssh sysadmin@10.10.1.116
enter password
result in picture
```
# Aliases

## Go to root user 
 ```bash   
nano ~/.bash_aliases
cdog ~/.bashrc 
gabby ~/.bashrc
```
### update box
```bash
alias update='sudo -- sh -c "apt update && apt upgrade"'
# colorful output 
alias grep='grep --color=auto'
alias vnstat='vnstat -i eth0' 
alias flush_redis='redis-cli -h 127.0.0.1 FLUSHDB'

#Save and close the file.

source ~/.bash_aliases
    alias sudo='sudo '
```
Arch Linux 10/10 would not recommend if you like being headache free!

