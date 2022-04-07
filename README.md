# USB-boot-Raspberry-pi-2b-V1.1
## Step 1

in Windows:

Download latest Raspbian Buster from images server
Download Win32 Disk Imager from this link
Flash Buster on a 16/324GB SD with Win32 Disk Imager
Add an empty file named 'ssh' to the boot (VFAT) partition of your SD card to enable SSH

## Step 2

in RPi:

Insert the SD into the Raspberry and power on
Complete the base installation in the normal way
Power off RPi
## Step 3

in Windows:

Create an image of SD with Win32 Disk Imager and name it Buster_installed.img
Connect your SSD to USB
If new, quick format it as FAT32
Flash Buster_installed image on the SSD with Win32 Disk Imager
Disconnect the SSD
## Step 4

in RPi

Insert the SD and the SSD into the Raspberry and power on

From now on all the PARTUUID shown are only for example, you’ve to use yours…

Now type

sudo ls /dev/disk/by-partuuid

the output:

pi@raspberrypi:~ $ Sudo ls /dev/disk/by-partuuid

16f266b1-01 16f266b1-02 2f1ce4b1-01 2f1ce4b1-02

If the PARTUUID are the same (and it should be):

Type:

sudo fdisk /dev/sda

Welcome to fdisk (util-linux 2.34).

Changes will remain in memory only, until you decide to write them.

Be careful before using the write command.

Command (m for help): x # enter x to go to expert menu

Expert command (m for help): i # enter i to change identifier

Here below put random 8 numbers/letters

Enter the new disk identifier: 0x60123f76 # at your choice

Disk identifier changed from 0x60123f75 to 0x60123f76.

Expert command (m for help): r # enter r to return to main menu

Command (m for help): w # enter w to write change to MBR

The partition table has been altered.

Calling ioctl() to re-read partition table.

Syncing disks.

Now type

sudo ls /dev/disk/by-partuuid

For sure now the PARTUUID are different.

## Step 5

Open a terminal and type

blkid

the output’:

pi@raspberrypi:~ $ blkid

/dev/mmcblk0p1: LABEL_FATBOOT="boot" LABEL="boot" UUID="B05C-D0C4" TYPE="vfat" PARTUUID="2f1ce4b1-01"

/dev/mmcblk0p2: LABEL="rootfs" UUID="075b0d0c-7b1e-4855-9547-125591698723" TYPE="ext4" PARTUUID="2f1ce4b1-02"

Here you find the SD PARTUUID (ex. 2f1ce4b1-02)

## Step 6

Now type

sudo ls /dev/disk/by-partuuid

the output:

pi@raspberrypi:~ $ Sudo ls /dev/disk/by-partuuid

60123f76-01 60123f76-02 2f1ce4b1-01 2f1ce4b1-02

Here you find the SSD PARTUUID (ex. 60123f76-02), usually /dev/sda2

Now type:

sudo nano /boot/cmdline.txt

Find “root=PARTUUID=2f1ce4b1-02”, replace with “root=PARTUUID=60123f76-02” and save

Type:

sudo reboot

If all is OK Raspberry should now boot from SSD.

Note that all above doesn’t compromise the original SD.

You can at anytime, simply running STEP 5 with the original SD’s PARTUUID, revert to SD boot.

Done!
