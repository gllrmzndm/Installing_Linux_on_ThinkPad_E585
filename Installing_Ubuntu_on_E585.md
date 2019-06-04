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


1. Add GRUB_CMDLINE_LINUX="**ivrs_ioapic[32]=00:14.0**"
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

Source(s): https://forum.level1techs.com/t/lenovo-thinkpad-e585-ryzen-2500u-vega-8-review-impressions-linux-etc/130307