# Setup for Zybo Z7-10

## 1. Setting up the SD card
***
In order to run PetaLinux on the Zybo, we must first prepare an SD card.

Creating a root filesystem that is persistent allows us to copy files to the root partition using our computer, then plug the card into the ZYBO board and boot from the card, rather than RAM (non-persistent).

On macOS, creating the partitions can be done in terminal using a single command.

**NOTE: We should first run `diskutil list` to determine the SD card's location (e.g. `disk4`), otherwise we could unintentionally format the wrong drive.**

Once we know our SD card location, run:

```
diskutil partitionDisk disk4 FAT32 BOOT 1G exfat root R
```
This calls the macOS Disk Utility function to partition the disk.

It tells Disk Utility which format the boot partition should be (`FAT32`), it's label (`BOOT`), and the size of the partition. One GB should be plenty for out PetaLinux installation.

The final part tells Disk Utility to format as `exFAT` the remaining SD card as `root`.

Next, we need to set the boot partition to active.

```
fdisk /dev/disk4
a
w
```
## 2. Copying files over
***
Next we need to copy the boot files onto the card. The `BOOT.BIN` and `image.ub` can be copied in terminal easily enough.

Navigate a terminal window to the place where our downloaded boot files are stored then:

```
cp BOOT.BIN /Volumes/BOOT
cp image.ub /Volumes/BOOT
```
The ext4 files are a little more difficult. We must first unmount (not eject) our `root` volume using `diskutil unmount /dev/disk4s3`. Then, while still in a terminal set at our download location, run:

```sudo dd if=rootfs.ext4 of=/dev/disk4s3```

This runs the data duplicator command copies bit for bit our PetaLinux instantiation.