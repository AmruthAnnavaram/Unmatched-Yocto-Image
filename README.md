# Unmatched-Yocto-Image
Loading Yocto images to the SiFive Unmatched using Ubuntu LTS 20.04

# Starting by creating a clone of your SD card 
To clone the SD card that comes with the board, use the dd function. To do this, start by unmounting any partitions on the SD card. In Ubuntu you can open the "Disks" application and see the name of the SD card as well as any mounted partitions. Once you are familiar with these, use the command ```sudo dd if=/dev/mmcblk0 of=~/sd-card-copy.img bs=1M status=progress```(And note the ```mmcblk0``` as this will change based on the name of your device.)
