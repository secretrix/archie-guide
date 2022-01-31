# Arch Installation Quick Guide (GNOME Edition)

Get SSH Up and Running on the liveCD
- Connect your phone via USB and turn on USB tethering
- `pacman -S networkmanager && systemctl start NetworkManager && nmtui`
- `nmcli && ssh-keygen`
- ssh (`ip provided by nmcli`)

Assuming you've already partitioned by this point according to your configuration
- `mount /dev/sdaX /mnt && mkdir /mnt/boot && mount /dev/sdaX /mnt/boot/`

- Install packages 
` for a GNOME-based desktop 
pacstrap /mnt base base-devel linux-zen linux-firmware grub openssh git fish sudo doas neovim nano networkmanager ttf-indic-otf noto-fonts noto-fonts-cjk noto-fonts-emoji ttf-hannom man-db texinfo btop pipewire pipewire-pulse pavucontrol xorg-server xorg-xrandr kitty xdg-user-data-dirs-gtk gnome-shell gnome-control-center gnome-tweaks nautilus gdm flameshot`
`
` for a i3-based desktop
pacstrap /mnt base base-devel linux-zen linux-firmware grub openssh fish git sudo doas neovim nano networkmanager ttf-indic-otf noto-fonts wqy-microhei ttf-baekmuk otf-ipafont ttf-hannom noto-fonts-emoji man-db texinfo pipewire pipewire-pulse pavucontrol i3-gaps xorg-xinit xorg-server xorg-xrandr kitty i3status-rs feh dmenu light playerctl xdg-user-data-dirs-gtk xdg-user-data-dirs lxsession-gtk3 flameshot gvfs pcmanfm xarchiver unzip
`


``` optionally 
aur : paru
browser : librewolf
gpu decode : intel-media-driver libvdpau-va-gl libva-utils intel-gpu-tools libva-intel-driver 
nvidia : linux-zen-headers nvidia-dkms
smaller asian fonts : wqy-microhei ttf-baekmuk otf-ipafont ttf-hannom 
bluetooth : blueman
power saving : tlp 
eye-candy : lxappearance-gtk3 wpgtk xsettingsd neofetch btop
login manager : ly
uefi wo/w windows : os-prober efibootmgr /ntfs-3g 
```

Generate Fstab
- `genfstab -U /mnt >> /mnt/etc/fstab`

Chroot into Arch Linux
- `arch-chroot /mnt`

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
- `grub-install --target=x86_64-efi --efi-directory=/boot --removable && nano /etc/default/grub && grub-mkconfig -o /boot/grub/grub.cfg`
- `grub-install --target=i386-pc /dev/sdX && nano /etc/default/grub && grub-mkconfig -o /boot/grub/grub.cfg`

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
 - `reboot`
