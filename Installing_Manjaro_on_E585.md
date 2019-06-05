# Installing Manjaro on the Lenovo ThinkPad E585

## Make sure to update your FIRMWARE of your laptop.

[Lenovo E585 support website](https://pcsupport.lenovo.com/nl/en/products/laptops-and-netbooks/thinkpad-edge-laptops/thinkpad-e585-type-20kv/downloads)

Firmware updates can be done with an USB flash drive.

## Get the Manjaro ISO.

[ Manjaro](https://manjaro.org/download/)

## Use Etcher or Rufus to make a bootable iso.

[BalenaEtcher](https://www.balena.io/etcher/) For Linux, MacOS and Windows.

[Rufus](https://rufus.ie/) For Windows only.

## Installing Manjaro

1. After inserting USB flash drive and turning on your laptop make sure to **press F12** during boot and select your USB flash drive or Ubuntu installation. 
2. **make sure to hold or press SHIFT a couple of times to enter the GRUB menu.**
3. Highlight "**Installing Boot: Manjaro.x86_64**"
4. Press the "**E**" key on your keyboard to edit.
5. add without quotes "**iommu=soft**" to the list.
6. 

    # Set arguments above with editor

            linux /boot/vmlinuz-$2
            initrd /boot/amd_ucode.img /boot/intel_ucode.img /boot/initramsfs-x86_64.img iommu=soft

7. **Press F10** to boot Manjaro.

## Configuring GRUB

Open your terminal with the keys **Super key (Windows key) + 4** and do the following:

    sudo nano /etc/default/grub


1. **Add** GRUB_CMDLINE_LINUX="**iommu=soft**"
2. It should look like this
3. 

    1 GRUB_DEFAULT=saved
    2 GRUB_TIMEOUT=5
    3 GRUB_TIMEOUT_STYLE=menu
    4 GRUB_DISTRIBUTOR='Manjaro'
    5 GRUB_CMDLINE_LINUX_DEFAULT="quiet   cryptdevice=UUID=826fc543-9373-4b18-bea5-ff6dfbb0135e:luks-826fa543-7873-4b189
    6 GRUB_CMDLINE_LINUX="iommu=soft"
    7 
    8 # If you want to enable the save default function, uncomment the following
    9 # line, and set GRUB_DEFAULT to saved.
    10 GRUB_SAVEDEFAULT=true


4. Press **CTRL + O** to save and **CTRL + X** to exit.
5. **Update** GRUB with:

        sudo update-grub

## Done


## Explanation about 'iommu=soft'

**Quoted** from **Tim Kennedy** at https://unix.stackexchange.com/questions/469153/what-are-the-implication-of-iommu-soft

*'**iommu=soft**' tells the kernel to use a software implementation to remap memory for applications that can't read above the 4GB limit.*

*The kernel documentation for these options is here: https://github.com/spotify/linux/blob/master/Documentation/x86/x86_64/boot-options.txt#L207*

*What's preferable is a solution that satisfies your expectations for performance, system temperature, battery life, etc, etc. If iommu=soft give you satisfactory performance, temperature, and battery life, then I would say go with that.*


