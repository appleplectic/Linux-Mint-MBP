# Linux Mint for T2 Macs

This folder is for people who would like to install Linux Mint 20.2 Cinnamon on their T2 Macs.

Find the ISO here: https://drive.google.com/drive/folders/1503hbWvWguvKIay_8dgrfbcWsqJ5UkLk?usp=sharing

Be sure to verify the SHA sum!


# Installation

Instead of the normal Linux Mint installation: 
a) Open the terminal in the LiveUSB and type `ubiquity -b` - this will open the installer but won't install the bootloader (don't choose an EFI partition during install).
b) Grab those wifi firmware files (use usb or ethernet or something else) and move them over to /lib/firmware/brcm in the LiveUSB, and then `sudo modprobe -r brcmfmac && sudo modprobe brcmfmac` in the terminal to activate WiFi. See the T2 Linux wiki for more info.
c) After install is complete you'll want to mount your root partition to /mnt and mount your EFI partition to /mnt/boot/efi (create that directory if it doesn't exist).
d) Type in the terminal: 
```
sudo apt update
sudo apt install arch-install-scripts
sudo arch-chroot /mnt
```
You'll now be in a chroot.
e) Install a T2 Ubuntu Kernel, and install dkms modules **for that kernel** (use the -k option when installing that kernel).
f) Install grub without nvram: `grub-install --removable --no-nvram --efi-directory=/boot/efi --bootloader-id=GRUB --target=x86_64-efi`
g) Modify /etc/default/grub - the one that says `GRUB_CMDLINE_LINUX` - put in the quotes `efi=noruntime acpi_osi=linux iommu=pt intel_iommu=on`
h) Run `update-grub`
g) Run the command `exit` to exit the chroot, `sudo umount /mnt/boot/efi && sudo umount /mnt` to unmount everything, and `sudo reboot`!
h) You're done!


## Credits & Help

https://wiki.t2linux.org should help you get DKMS, WiFi, and the Kernel up and running.

https://discord.gg/fsaU8nbaRT if you need people to help you, and to meet the people behind this project!
