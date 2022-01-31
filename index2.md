# Arch Installation Quick Guide (GNOME Edition)

Get SSH Up and Running on the liveCD
- Connect your phone via USB and turn on USB tethering
- `pacman -S networkmanager && systemctl start NetworkManager && nmtui`
- `nmcli && ssh-keygen`
- ssh (`ip provided by nmcli`)

Assuming you've already partitioned by this point according to your configuration
- `mount /dev/sdaX /mnt && mkdir /mnt/boot && mount /dev/sdaX /mnt/boot/`

- Install packages 
`
pacstrap /mnt base base-devel linux-zen linux-zen-headers linux-firmware grub os-prober efibootmgr ntfs-3g sshd git fish sudo doas neovim nano networkmanager ttf-indic-otf noto-fonts noto-fonts-cjk noto-fonts-emoji man-db texinfo btop htop neofetch xsettingsd libva-intel-driver pipewire pipewire-pulse pavucontrol xorg-server xorg-xrandr kitty xdg-user-data-dirs-gtk gnome-shell gnome-control-center gnome-tweaks nautilus gdm firefox code eog picard lollypop mpv flameshot`

Generate Fstab
- `fstabgen -U /mnt >> /mnt/etc/fstab`

Chroot into Arch Linux
- `arch-chroot /mnt`

Switch shell to Fish because it's simpler
- `fish`

Set your time
- `timedatectl set-ntp true`
- `ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime`
- `hwclock --systohc`

Generate Localisation
- `nano /etc/locale.gen && locale-gen `(uncomment `en_US.UTF-8`)

Export Localisation
- `nano /etc/locale.conf` (Add the following lines)
```
export LANG="en_US.UTF-8"
export LC_COLLATE="C"
```

Install and Configure bootloader/GRUB
- `grub-install --target=x86_64-efi --efi-directory=/boot --removable && grub-mkconfig -o /boot/grub/grub.cfg`

Set Hostname
- `nano /etc/hostname`

Set Hosts
- `nano /etc/hosts` 
```
127.0.0.1        localhost
::1              localhost
127.0.1.1        myhostname.localdomain  myhostname
```
 
 Enable Services
- `systemctl enable NetworkManager`
- `systemctl enable sshd`
- `systemctl enable bluetooth`

 Restart
 - `exit`
 - `exit`
 - `umount -Rl /mnt`
 - `reboot`
