# HP Microserver Gen8 with TrueNAS

The hardware configuration is the following:
- 4 GB Ram
- 2 x 2TB Hdd in 2 bays
- 1 x 120GB SSD in ODD
- 1 x 4GB USB Stick in the internal USB port

As previously I made this server to have FreeNAS with NextCloud on it for storing files, I used a 32 GB stick to keep and boot from it the Freenas installation. After a few years, this setup crashed. Here is how I made it work.

## Debian helped booting from the SSD on SATA port 5.

Here are the steps:
The easiest way to do this is to just install a latest version of Debian with GRUB2 and let it be the default boot loader. You can easily make changes in the future and it's so simple and quick to set up.

### Install TrueNAS on SDD
___UPDATE___: if there is an error while booting, after some years :), maybe the BIOS battery died. You just have to change it, and set in BIOS the AHCI mode.
* Set your BIOS to AHCI
    * The first thing to do is install TrueNAS (used Truenas-12.0-U8.iso) on the 5th SATA port. The key to this is to have it be the only drive plugged into the system and it'll boot fine. Remove the front drives, all USB, etc
    * I just installed TrueNAS this way, using an USB stick on the external port and it ran perfectly in AHCI mode.
* Once TrueNAS is setup and running, power the server down and unplug that SATA port.
    * Double check - Unplug all drives, SSD, front HDD etc

### Install Debian
* Install the target boot USB stick in the internal slot (I used a 4GB one) this is where to install Debian
* Download latest Debian ISO and flash to another USB stick.
    * https://www.debian.org/download
* Use RaspberryPi Imager tool to write the ISO to USB
    * https://www.raspberrypi.com/news/raspberry-pi-imager-imaging-utility/
* Mount this Debian stick in the front port with a keyboard
* Get Debian installed as GUI but a minimal install to the internal USB. It's a full desktop so it can be handy, but I've only installed the basics. If you installed only the text mode version, you can do an ```sudo apt install xfce4 xorg```
* Install one new tool on the Ubuntu server called - *Grub Customizer*
* Open a terminal and type ```sudo apt install grub-customizer```
* Shutdown the server
    * Unplug the Ubuntu installer USB stick from the front.
    * Plug back in your SDD on the internal SATA port
    * Plug back in your front drives
    * Leave your newly installed Debian in the internal USB port as we want to boot from this al the time.
* Boot into BIOS and set the default boot to be USB. This is critical

### Configure grub
* The Debian will still boot from the internal USB
* Run the tool - Grub Customizer from within Debian
* It will scan and detect all of your drives looking for operating systems
* It'll find your TrueNAS install so move that to the very top of the list as the default entry
* Rename it to something meaningful if you want
* Make the Debian entry second
* Save
* Reboot and the server will automatically boot from USB, then load your OS from the 5th SATA port automatically.

I faced some issues when running Grub Customizer. As i didn't see any other disks than Debian USB stick, i did the following:
* created a new entry named TrueNAS. Left it empty. Moved it the first one.
* Saved and closed aplication
* run ```sudo os-prober```
* run ```sudo update-grub```
* go to /etc/grub.d/ and in the folders find the file **custom**. It should contain an empty *menuentry*. Fill it like this:
```
menuentry "TrueNAS" --class freebsd --class bsd --class os {
    insmod bsd
    insmod zfs
    insmod chain
    insmod part_gpt
    echo   Chaining hd5 ...
    set root=(hd5)
    chainloader +1
}

```
* save the file and run ```sudo update-grub```
* reboot

In this moment you should have the server boot from the desired disk.

**Warning: Grub isn't counting the drive bays that have no drive in!!!**

I got this error: **HD5 cannot get C/H/S values**. So I set it to chainload *HD3* (the ODD) and it now boots.

As a reminder, each time you add/extract HDDs from the bay, maybe you get this error and have to count which drive to put.


### Links
- https://homeservershow.com/forums/topic/18229-booting-from-5th-sata-port-help-installing-grub-on-usb/
- https://askubuntu.com/questions/197868/grub-does-not-detect-windows
- https://community.hpe.com/t5/ProLiant-Servers-Netservers/Booting-Freenas-from-SSD-in-ODD-Bay-gives-error-HD5-cannot-get-C/td-p/7044134
- https://www.truenas.com/community/threads/unable-to-boot-freenas-on-ssd.74327/
