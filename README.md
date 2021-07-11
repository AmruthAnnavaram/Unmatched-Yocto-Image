# Unmatched-Yocto-Image
Loading Yocto images to the SiFive Unmatched using Ubuntu LTS 20.04

# Starting by creating a clone of your SD card 
To clone the SD card that comes with the board, use the dd function. To do this, start by unmounting any partitions on the SD card. In Ubuntu you can open the "Disks" application and see the name of the SD card as well as any mounted partitions. Once you are familiar with these, use the command ```sudo dd if=/dev/mmcblk0 of=~/sd-card-copy.img bs=1M status=progress```(And note the ```mmcblk0``` as this will need to be changed by you based on the name of your device.) Once this process is completed, you will have a file appear called ```sd-card-copy.img``` This is a clone of your SD card.

# Easy installation

To bypass the steps below, follow this. First 

# Booting a Yocto Image to your newly formatted SD card
Begin by creating a new directory and changing to it with the command ```mkdir riscv-sifive && cd riscv-sifive``` Next, download the needed repositories with ```repo init -u git://github.com/sifive/meta-sifive -b 2021.06 -m tools/manifests/sifive.xml``` and to sync the repo use the command ```repo sync``` Now to start a working branch use ```repo start work --all```To download the build tools that are provided use ```Python3 ./openembedded-core/scripts/install-buildtools -r yocto-3.2_M2 -t 20200729```And also .```Python3 ./openembedded-core/buildtools/environment-setup-x86_64-pokysdk-linux``` To setup the build environment use ```. ./meta-sifive/setup.sh```
Now to build an image for the Unmatched board use the command ```MACHINE=unmatched bitbake core-image-minimal``` Finally, to find the image you just made go to $BUILDDIR/tmp-glibc/deploy/images/$MACHINE and the image should be found in the Unmatched folder. Lastly, to copy the image to the SD card, first find the file that will be formatted as ```<image>-<machine>.<output_format>``` an example is ```core-image-minimal-unmatched.wic.xz``` Look for the ```wic.xz``` file. And open a terminal in that directory. Make sure to unmount your SD card and then put in the command ```xzcat core-image-minimal-unmatched.wic.xz | sudo dd of=/dev/sdX bs=512K iflag=fullblock oflag=direct conv=fsync status=progress``` (Note that you should write to the entire SD card, not a specific partition)

