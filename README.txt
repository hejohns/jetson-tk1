# jetson-tk1
notes for installing L4T on nvidia jetson tk1 (after many hours of testing/searching)

notes for versions:
--21.5

easiest method is to follow nvidia's "getting started" txt file instructions w/ these additional notes:
--ext2/ext3 seem to work w/o additional flags passed to mkfs.ext* but this is system dependent. YMMV
--for ext4, see https://forums.developer.nvidia.com/t/uboot-and-kernel-fails-to-boot-due-to-ext4/52057
--(aka do 'mkfs.ext4 -O ^64bit,^metadata_csum'. Apparently these are likely default flags and uboot no like)
--double check that in /boot/extlinux on rootfs, that extlinux.conf is the one you want
--(if booting from sd, use jetson-tk1_extlinux.conf.sd, etc)

when using sata as rootfs:
--as per https://forums.developer.nvidia.com/t/solved-l4t21-1-problem-booting-from-sata/35886/12
--the goal is to boot off sd card (or emmc), then bootloader mounts /dev/sda1 as /dev/root
--flash bootloader as descibed by "getting started" txt file to mmcblk1p1 aka sd card
--('sudo ./flash.sh -S 14GiB jetson-tk1 mmcblk1p1' seems fine)
--(or emmc. I just like to avoid degrading the emmc as much as possible)
--partition whatever /dev/sda is such that the first partition is 14GiB. gpt partition was ok for me.
--pay attention to filesystem caveats above when partitioning sd card and wherever /dev/sda1 is going to be
--load sample rootfs from nvidia onto sd card and 'sudo ./apply_binaries'. Do the same for /dev/sda1
--(of course /dev/sda on the jetson may not be called such on the host you're flashing from)
--on the sd card fs and /dev/sda1 fs, 'cp -a jetson-tk1_extlinux.conf.usb extlinux.conf'
--it's linkely the sd card does not need the full sample filesystem (probably only needs /boot), but this method at least works
