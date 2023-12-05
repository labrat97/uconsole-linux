Linux kernel
============

There are several guides for kernel developers and users. These guides can
be rendered in a number of formats, like HTML and PDF. Please read
Documentation/admin-guide/README.rst first.

In order to build the documentation, use ``make htmldocs`` or
``make pdfdocs``.  The formatted documentation can also be read online at:

    https://www.kernel.org/doc/html/latest/

There are various text files in the Documentation/ subdirectory,
several of them using the Restructured Text markup notation.

Please read the Documentation/process/changes.rst file, as it contains the
requirements for building and running the kernel, and information about
the problems which may result by upgrading your kernel.

Build status for rpi-6.1.y:
[![Pi kernel build tests](https://github.com/raspberrypi/linux/actions/workflows/kernel-build.yml/badge.svg?branch=rpi-6.1.y)](https://github.com/raspberrypi/linux/actions/workflows/kernel-build.yml)
[![dtoverlaycheck](https://github.com/raspberrypi/linux/actions/workflows/dtoverlaycheck.yml/badge.svg?branch=rpi-6.1.y)](https://github.com/raspberrypi/linux/actions/workflows/dtoverlaycheck.yml)

## Building

First, build the kernel and copy to the boot partition.

```
sudo apt install git bc bison flex libssl-dev make
git clone https://github.com/labrat97/uconsole-linux.git
make bcm2711_uc_defconfig
make -j4 Image.gz modules dtbs
sudo make modules_install
sudo cp arch/arm64/boot/dts/broadcom/*.dtb /boot/
sudo cp arch/arm64/boot/dts/overlays/*.dtb* /boot/overlays/
sudo cp arch/arm64/boot/Image.gz /boot/kernel8.img
```

Next, find the kernel tag used for your build using `ls /lib/modules`. Using that tag as `[TAG]`:

```
sudo cp .config /boot/config-[TAG]
sudo update-initramfs -c -k [TAG]
```

After everything is done, `sudo reboot`.
