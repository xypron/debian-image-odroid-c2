Build Debian SD card image for RISC-V 64bit QEMU
================================================

This project provides files to generate a Debian SD card image
for RISC-V 64bit QEMU.

The following command installs the dependencies:

    sudo apt-get install debian-ports-archive-keyring debootstrap dosfstools \
    fakeroot xz-utils

When building on an architecture other than riscv64 QEMU is needed:

    sudo apt-get install qemu-user-static

To create the SD card image execute

    make

A new image is created with three partions:

- efi partition
- boot partition
- root partition

Debootstrap is used to install a base system.
The U-Boot and Linux kernel images are added from
http://debian.xypron.de/.

A sudo user *qemu* with password *qemu* is provided.

The created image file is called *image*.

To copy the image to an SD card use

    sudo dd iflag=dsync oflag=dsync if=image of=/dev/sdX bs=16M

Replace /dev/sdX by the actual device.
**Beware of overwriting your harddisk by specifying the wrong device.**

After first boot
----------------

Change the password of user qemu.

    passwd

You can enlarge the root partition: Use fdisk to delete partition 3 and recreate
it with the same start sector. **Do not delete the signature.**
Use resize2fs to resize the file system to match the partition size.
