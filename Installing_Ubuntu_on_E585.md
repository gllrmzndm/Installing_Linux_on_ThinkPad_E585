# Installing Ubuntu 18.04.x LTS (Bionic Beaver) on the Lenovo ThinkPad E585

## Make sure to update your FIRMWARE of your laptop.

[Lenovo E585 support website](https://pcsupport.lenovo.com/nl/en/products/laptops-and-netbooks/thinkpad-edge-laptops/thinkpad-e585-type-20kv/downloads)

## Get Ubuntu 18.04.x LTS.

[Ubuntu 18.04.2 LTS (Bionic Beaver)](http://releases.ubuntu.com/18.04/)

## Use Etcher or Rufus to make a bootable iso.

[BalenaEtcher](https://www.balena.io/etcher/) For Linux, MacOS and Windows.

[Rufus](https://rufus.ie/) For Windows only.

## Installing Ubuntu

1. After inserting USB flash drive and turning on your laptop make sure to **press F12** during boot and select your USB flash drive or Ubuntu installation. 
2. **make sure to hold or press SHIFT a couple of times to enter the GRUB menu.**
3. Highlight "**Installing Ubuntu**"
4. Press the "**e**" key on your keyboard to edit.
5. add without quotes "**ivrs_ioapic (32)=00:14.0**" to the list.
6. 

    setparams 'Try Ubuntu without Installing'

            set gfxpayload-keep
            linux         /casper/vmlinuz file/cdrom/preseed/ubuntu.seed boot=casper quiet splash -- ivrs_ioapic (32)=00:14.0
            initrd        /casper/initrd


7. **Press F10** to start the installation of Ubuntu.

## Configuring GRUB

Open your terminal with the keys **CTRL + ALT + T** and do the following:

    sudo nano /etc/default/grub


1. **Add** GRUB_CMDLINE_LINUX="**ivrs_ioapic[32]=00:14.0**"
2. It should look like this
3. 

    $ sudo cat /usr/share/grub/default/grub
    # If you change this file, run    'update-grub' afterwards to update
    # /boot/grub/grub.cfg.
    # For full documentation of the     options in this file, see:
    #   info -f grub -n 'Simple     configuration'

    GRUB_DEFAULT=0
    GRUB_HIDDEN_TIMEOUT=0
    GRUB_HIDDEN_TIMEOUT_QUIET=true
    GRUB_TIMEOUT=10
    GRUB_DISTRIBUTOR=`lsb_release -i -s 2>    /dev/null || echo Debian`
    GRUB_CMDLINE_LINUX_DEFAULT="quiet     splash"
    GRUB_CMDLINE_LINUX="ivrs_ioapic[32]=00:14.0"

    # Uncomment to enable BadRAM filtering,    modify to suit your needs
    # This works with Linux (no patch     required) and with any kernel that    obtains
    # the memory map information from GRUB    (GNU Mach, kernel of FreeBSD ...)
    #GRUB_BADRAM="0x01234567,0xfefefefe,    0x89abcdef,0xefefefef"

4. Press **CTRL + O** to save and **CTRL + X** to exit.
5. **Update** GRUB with:

        sudo update-grub

## Done


## What does ivrs_ioapic[32]=00:14.0 do?

**Quote from eazrael on https://evilazrael.de/comment/914**

*Ok, this is really a firmware bug, the ACPI IVRS table lacks at least one entry. Adding ivrs_ioapic[32]=00:14.0 instead of intremap=off is sufficient to make the system boot until Lenovo releases an UEFI update with a working IVRS table. At least UEFI 1.27 (2018-07-24) needs this override. And spec_store_bypass_disable=prctl is still needed for Ubuntu & co.*

*The clue is the line "[Firmware Bug]: AMD-Vi: IOAPIC[32] not in IVRS table". I decompiled the ACPI tables, started to read the AMD documentation, but in the end I just guessed the 32 from the error message and 00:14.0 from the lspci output and the Stack Overflow/Ubuntu forum entries.  Interesting stuff, but too much to read in my little time.* 

*What was helpful is the Linux boot parameter amd_iommu_dump=1 which will dump information from the IVRS table:*

    [    0.851042] AMD-Vi: Using IVHD type 0x11
    [    0.851401] AMD-Vi: device: 00:00.2 cap: 0040 seg: 0 flags: b0 info 0000
    [    0.851401] AMD-Vi:        mmio-addr: 00000000feb80000
    [    0.851430] AMD-Vi:   DEV_SELECT_RANGE_START  devid: 00:01.0 flags: 00
    [    0.851431] AMD-Vi:   DEV_RANGE_END           devid: ff:1f.6
    [    0.851870] AMD-Vi:   DEV_ALIAS_RANGE                 devid: ff:00.0 flags: 00   devid_to: 00:14.4
    [    0.851871] AMD-Vi:   DEV_RANGE_END           devid: ff:1f.7
    [    0.851875] AMD-Vi:   DEV_SPECIAL(HPET[0])           devid: 00:14.0
    [    0.851876] AMD-Vi:   DEV_SPECIAL(IOAPIC[33])                devid: 00:14.0
    [    0.851877] AMD-Vi:   DEV_SPECIAL(IOAPIC[34])                devid: 00:00.1
    [    1.171028] AMD-Vi: IOMMU performance counters supported

*Resolving devid 00:14.0 was easy via lspci:*

*00:14.0 SMBus: Advanced Micro Devices, Inc. [AMD] FCH SMBus Controller (rev 61)
but for 00:00.1 I have not found the device. If anybody knows how to list all device ids and their associate devices/drivers/etc, please mail me.*


*Sources/References:*

    https://ubuntuforums.org/showthread.php?t=2254677
    https://superuser.com/questions/1052023/ioapic0-not-in-ivrs-table
    https://lwn.net/Articles/664999/
    https://01.org/linux-acpi/utilities
    https://support.amd.com/TechDocs/48882_IOMMU.pdf



Source(s): https://forum.level1techs.com/t/lenovo-thinkpad-e585-ryzen-2500u-vega-8-review-impressions-linux-etc/130307

https://evilazrael.de/comment/914

All the credits goes the person(s) for making this happen cited in the source. Thanks you.