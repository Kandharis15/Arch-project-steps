#ARCH LINUX PROJECT INSTALLATION STEPS

1. Creating the actual VM for ArchLinux:
    a. Before being able to actually create VM, needed to get ISO image. Used this one in the "installation guide" area of the wiki:
    archlinux-2023.10.14-x86_64

    b. Went through the "custom" version of creating the VM. Ultimately, I gave the VM 2 GB of ram, 2 processors, and 20 GB of space in the hard disk (SCSI).

    c. At this point, the VM is created. Can boot into the VM without any issues.

2. Edit vmx file
    a. Open .vmx file in notepad. Using uief mode.
    b. After first line (encoding), hit ENTER and type the following: firmware = "efi"

3. Open the VM and install arch linux. Verify some stuff.
    a. Verify internet connect. ping archlinux.org. CTRL + C to exit out of endless loop.
    b. Verify time: timedatectl

4. Partition the disks
    a. fdisk/dev/sda. m for help.
    b. p, n, p (primary), 1
    c. Use default first sector. Last sector: 2048+300 mb; + 500M.
    d. Partition created at this point. Try p.
    e. Second partition: n, p, 2.

5. Format the partitions sda1 and sda 2
    a. Filesystem: mkfs.ext4/dev/sda2
    mkfs.ext4/dev/sd1
    b. mount /dev/sda2 /mnt
    c. mkdir /mnt/boot
    d. mount /dev/sda1 /mnt/boot


6. Download some packages/commands (sudo, grep, etc)
    a. pacstrap /mnt base base-devel linux linux-firmware vim
    b. genfstab -U /mnt >> /mnt/etc/fstab


7. Chroot in and set up network manager
    a. arch-chroot /mnt /bin/bash #Chroot
    b pacman -S networkmanager grub #Network manager
    c.  systemctl enable Network Manager 
    d. sudo grub-install --target=x86_64-efi --efi-directory=/mnt --bootloader-id=GRUB #Install grub
    e. grub-mkconfig -o /boot/grub/grub.cfg #Configure grub

8. Set password/locale
    a. passwd #set password
    b. vim/etc/locale.gen #Languagevim
    c. Hit the i letter on keyboard and uncomment en_US UTF and its iso. Esc to leave insert method. :wq to save and quit.
    d. locale-gen #Generate locales
    e. vim /etc/locale.conf
    f. LANG=en-US.UTF-8 and :wq
    g. vim /etc/hostname
    h. Arch Linux is name

9. Set time
    a. ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime #Set time zone

10. Exit chroot, unmount, and reboot
    a. umount -R /mnt
    b. reboot
