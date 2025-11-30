# sunxi-livesuit to be used to install Valetudo

LiveSuit is a proprietary tool by Allwinner used to flash images to Allwinner-SoC based devices.
This repository is a fork of the [https://github.com/linux-sunxi/sunxi-livesuite](https://github.com/linux-sunxi/sunxi-livesuite) repo and is used to install [Valetudo](https://github.com/hypfer/Valetudo) on supported vacuum robots.

This readme assumes that you have just installed a fresh copy of [Debian Trixie](https://www.debian.org/releases/trixie/) with some kind of GUI (e.g. KDE).
Commands may vary based on your Linux-Distribution. I **strongly** recommend a fresh install for this to not taint your main one.

Ubuntu is known to cause problems. Just use Debian, really. It's the superior OS anyway.

**Hint:**<br/>
If you're using a recent-ish computer for this, make sure that `Secure Boot` is disabled in the BIOS or else the kernel
will refuse to load the unsigned freshly built `awusb` module.

With that out of the way, here are a bunch of commands to copy-paste into a **rootshell**:
```
apt install -y build-essential git bison flex linux-headers-amd64 dkms autogen automake libtool m4 zlib1g-dev fastboot

git clone https://github.com/hypfer/valetudo-sunxi-livesuit
cd valetudo-sunxi-livesuit/awusb
make -j2
cp awusb.ko /lib/modules/`uname -r`/kernel/
/sbin/depmod -a
/sbin/modprobe awusb
cd ..

tar xvf libpng_1.2.54.orig.tar.xz
cd libpng-1.2.54/
./autogen.sh
./configure
make -j2
make install
/sbin/ldconfig
cd ..

./LiveSuit.sh
```

The last command should leave you with an open LiveSuit GUI window looking similar to this:

![livesuit.png](livesuit.png)

If you see that, head back to the Valetudo installation instructions
