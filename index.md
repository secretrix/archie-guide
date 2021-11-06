# Arch Installation Quick Guide

Get SSH Up and Running on the liveCD
- Connect your phone via USB and turn on USB tethering
- `pacman -S networkmanager && systemctl start NetworkManager && nmtui`
- `nmcli && ssh-keygen`
- ssh (`ip provided by nmcli`)

Assuming you've already partitioned by this point according to your configuration
- `mount /dev/sdaX /mnt && mkdir /mnt/boot && mount /dev/sdaX /mnt/boot/`

- Install packages 
`
pacstrap /mnt base base-devel linux-zen linux-zen-headers linux-firmware grub os-prober efibootmgr ntfs-3g irqbalance sshd bash-completion fish git zsh bat sudo doas neovim nano networkmanager ttf-indic-otf noto-fonts noto-fonts-cjk noto-fonts-emoji man-db texinfo bpytop htop neofetch xsettingsd youtube-dl intel-media-driver libvdpau-va-gl libva-utils intel-gpu-tools libva-intel-driver pipewire pipewire-pulse bluez-utils pipewire-zeroconf bluez flatpak tlp pavucontrol i3-gaps xorg-xinit xorg-server xorg-xrandr kitty i3status-rs feh python-pywal dmenu light playerctl xdg-user-data-dirs-gtk xdg-user-data-dirs lxqt-policykit-agent dex firefox lollypop mpv blueman flameshot gvfs pcmanfm
`

Lockscreen and AUR
- TODO

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
- `systemctl enable tlp`
- `systemctl enable irqbalance`
- `systemctl enable bluetooth`

 Restart
 - `exit`
 - `exit`
 - `umount -Rl /mnt`
 - `reboot`
