# Unmatched-Yocto-Image
Making and loading Yocto images to the SiFive Unmatched using Ubuntu LTS 20.04

# Starting by creating a clone of your SD card
1. Go to this link https://drive.google.com/file/d/1cKBcpb9g0_FtCSoG6hXRJecwlgSh4Tqw/view?usp=sharing which contains a 32GB image of the default OS that comes with the board in a zipped file

2. Upon opening and unzipping, you will get an image of the default OS that comes with the Unmatched board

OR:

Below are numbered steps to creating an SD card clone:
1. Start by unmounting any partitions on the SD card. In Ubuntu you can open the "Disks" application and see the name of the SD card as well as any mounted partitions. 

2. Once you are familiar with your device name and have unmounted all the partitions, use the command ```sudo dd if=/dev/mmcblk0 of=~/sd-card-copy.img bs=1M status=progress```(And note the ```mmcblk0``` as this will need to be changed by you based on the name of your device.) 

3. Once this process is completed, you will have a file appear called ```sd-card-copy.img``` This is a clone of your SD card.

# Copying the cloned image to a new SD card
Below are numbered steps to copying a cloned SD card .img to a new SD card:
1. Make sure your new SD card has all its partitions unmounted.

2. Then use the command ```sudo dd if=~/sd-card-copy.img of=/dev/mmcblk0 bs=1M status=progress```

3. You will find that the SD card now has the same formatting as the default SD card that came with the Unmatched


# Easy Version

To bypass the detailed steps below, follow this. First, follow the section titled "Starting by creating a clone of your SD card." AND the section titled "Copying the cloned image to a new SD card."

Upon having a cloned SD card, follow the steps below to use the core-image-minmal Yocto image and run a basic hello world program on it:
1. Download the file from this link https://drive.google.com/file/d/1oGXfI1gWtRhK6f9vyqzeM9ht16cbyMT6/view?usp=sharing which contains a core-image-minimal Yocto image for the Unmatched with a basic hello world program

2. Unzip the file 

3. Plug in your new SD card and unmount all volumes

4. Use the command ```sudo dd if=~/yocto-unmatched-minimal-image.img of=/dev/mmcblk0 bs=1M status=progress```

5. Once completed, plug the SD card into the board and it should boot 

6. Enter root as the username

7. To run the hello world program, just input ```./a.out```


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

