# Install Debian on iMag G5

I struggled 2 weeks until i got it running.

Take the latest Debian ppc distro, not ppc64el. Ppc64el is for intel ppc with little endian.
Install it. If you have a freezing screen @ nouveau, having the NVidia FX 5200, do the following:
* install the kernel with page size 4K instead 64K. Link below
* @ boot: Linux nouveau.noaccel=1 video=TV-1:d
This made my iMac show the X.

Do not bother to try to install nvidia drivers. They are only for amd64/i386.

## put the settings in yaboot
With root rights:
nano /etc/yaboot.conf

Add:
append="nouveau.noaccel=1 video=TV-1:d"

Run:
ybin -v //to recreate conf for boot




### Links:
- https://wiki.debian.org/iMacG5
- https://www.debian.org/ports/powerpc/inst/yaboot-howto/index.en.html#contents
- https://www.debian.org/ports/powerpc/inst/yaboot-howto/ch9.en.html
- http://hermes.ppckernel.org/cgi-bin/man/man2html?8+yaboot
- http://powerpcliberation.blogspot.ro/2015/07/g5-nouveau-2d-acceleration.html
- https://lists.debian.org/debian-powerpc/2015/06/msg00009.html
- https://lists.debian.org/debian-powerpc/2015/07/msg00012.html
- http://ppcluddite.blogspot.ie/2012/03/installing-debian-linux-on-ppc-part-iii.html
- https://wiki.ubuntu.com/PowerPCFAQ#Configure_graphics
- http://nouveau.freedesktop.org/wiki/
- http://powerpcliberation.blogspot.ca/2015/05/jessie-meets-bigmac.html
- https://lists.debian.org/debian-powerpc/2015/06/msg00064.html !!! the kernel to install
- https://drive.google.com/folderview?id=0B8pqd5Ots1vffmY5ZnpGUDg4UGNnVFk2M05tQUtEUUkwUmhmQWdLMWpfZGVraDIxSFltb1k&usp=drive_web > 2 debs to install
-http://www.phoronix.com/forums/forum/linux-graphics-x-org-drivers/open-source-nvidia-linux-nouveau/44881-nv-and-xorg-conf-under-debian-ppc
- https://wiki.ubuntu.com/PowerPCFAQ#How_do_I_configure_yaboot.conf.3F > lots of info that clarified my questions
- http://hermes.ppckernel.org/cgi-bin/man/man2html?5+yaboot.conf


### Sound links:
- http://alsa.opensrc.org/Aoa
- http://johannes.sipsolutions.net/Projects/snd-aoa
