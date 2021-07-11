# Unmatched-Yocto-Image
Making and loading Yocto images to the SiFive Unmatched using Ubuntu LTS 20.04

# Starting by creating a clone of your SD card 
To clone the SD card that comes with the board, use the dd function. To do this, start by unmounting any partitions on the SD card. In Ubuntu you can open the "Disks" application and see the name of the SD card as well as any mounted partitions. Once you are familiar with these, use the command ```sudo dd if=/dev/mmcblk0 of=~/sd-card-copy.img bs=1M status=progress```(And note the ```mmcblk0``` as this will need to be changed by you based on the name of your device.) Once this process is completed, you will have a file appear called ```sd-card-copy.img``` This is a clone of your SD card.

# Prefabricated Version

To bypass the detailed steps, follow this. First, follow the section titled "Starting by creating a clone of your SD card."

# 

# Booting a Yocto Image to your newly formatted SD card
Below are the numbered steps to create a Yocto image(this is taken from https://github.com/sifive/freedom-u-sdk, the SiFive FreedomUSDK repo)

1. Begin by creating a new directory and changing to it with the command ```mkdir riscv-sifive && cd riscv-sifive``` 

2. Next, download the needed repositories with ```repo init -u git://github.com/sifive/meta-sifive -b 2021.06 -m tools/manifests/sifive.xml``` and to sync the repo use the command ```repo sync``` 

3. Now to start a working branch use ```repo start work --all```

4. To download the build tools that are provided use ```Python3 ./openembedded-core/scripts/install-buildtools -r yocto-3.2_M2 -t 20200729```And also .```Python3 ./openembedded-core/buildtools/environment-setup-x86_64-pokysdk-linux``` 

5. To setup the build environment use ```. ./meta-sifive/setup.sh```

6. Now to build an image for the Unmatched board use the command ```MACHINE=unmatched bitbake core-image-minimal``` 

# Copying the image to your SD card
1. To find the image you just made go to $BUILDDIR/tmp-glibc/deploy/images/$MACHINE and the image should be found in the Unmatched folder. 
2. To copy the image to the SD card, first find the file that will be formatted as ```<image>-<machine>.<output_format>``` an example is ```core-image-minimal-unmatched.wic.xz``` Look for the ```wic.xz``` file. And open a terminal in that directory. Make sure to unmount your SD card and then put in the command ```xzcat core-image-minimal-unmatched.wic.xz | sudo dd of=/dev/sdX bs=512K iflag=fullblock oflag=direct conv=fsync status=progress``` (Note that you should write to the entire SD card, not a specific partition)

