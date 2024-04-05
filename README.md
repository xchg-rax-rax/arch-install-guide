# arch-install-guide
I find myself doing this quite often so here's how I do it

lsblk, find what the name of the drive(s) you wish to install to
ls /sys/firmware/efi/efivars, to check if the machine uses uefi or legacy bios, if you see stuff it's the former
ping 4.2.2.2, check if internet is up, if not use iwctl to set up connection
fdisk /dev/sdx, create 3 partitions, ~1G boot, >30G root and what remains for home partition, type n for new partition, w to write
mkfs.ext4 /dev/sdx2, make root fs
mkfs.ext4 /dev/sdx3, make home fs
if bios == uefi:
	mkfs.fat -F32 /dev/sdx1
else:
	mkfs.ext4 /dev/sdx1
mount /dev/sdx2 /mnt
mkdir /mnt/boot
mkdir /mnt/home
mount /dev/sdx1 /mnt/boot
mount /mnt/sdx3 /mnt/home
lsblk, verify
vim /etc/pacman.d/mirrorlist, find a uk mirror and move it to the top of the list
pacstrap /mnt alacritty alsa-utils arandr asio base base-devel bind bspwm btop calibre clang cmake curl dmenu dmidecode dnsmasq docker docker-compose efibootmgr feh geoclue git glow go graphviz grub iptables-nft iputils iwd jq keepassxc libvirt linux linux-firmware lvm2 mage man-db mkinitcpio mpv neofetch neovim net-tools networkmanager newsboat nlohmann-json nmap noto-fonts npm ntfs-3g obs-studio obsidian openldap openssh openvpn p7zip picom polybar proxychains-ng pv python-jedi python-pywal qemu-full rust-analyzer scrot steam stow sxhkd syncthing syncthing-discosrv tldr tmux tor tree usbutils vim virt-manager wget whois wine wireshark-qt xorg-server xorg-xbacklight xorg-xinit yarn zathura zsh 
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Europe/London /etc/localtime
hwclock --systohc
vim /etc/locale.gen, uncomment the correct locale
locale-gen
vim /etc/locale.conf, LANG=en_GB.UTF-8
systemctl enabel NetworkManager
vim /etc/hostname, type name of computer you want
vim /etc/hosts, type defaults
if bios == uefi:
	grub-install --target=x86-64 --efi-directory=/boot --bootloader-id=GRUB
else:
	grub-install --target=i386-pc /dev/sdx
grub-mkconfig -o /boot/grub/grub.cfg
passwd
--Ya done son-- No for the set up...
useradd -m -g wheel <usr>
passwd
echo "exec bspwm" >> ~/xinitrc
--Really for here just install your normal dot files and fix bugs/install
drivers and packages to taste---
