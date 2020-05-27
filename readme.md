# Raspberry Pi TV HAT documentation

Getting [Started](https://www.raspberrypi.org/app/uploads/2018/10/Getting-started-with-the-Raspberry-Pi-TV-HAT.pdf)

***Steps to add new undefined frequency channel***
1. Enter the frequency of the channel (listed below for Singapore's MediaCorp FTA channels), 
2. Enter 8MHz as Bandwidth and click on Create. 
3. Repeat the steps to Add the frequencies for all the FTA channels.

## Singapore's MediaCorp Frequency Channel
* 538250 Hz 
  + Ch 5 (HD)
  + Suria (HD)
* 554000 Hz 
  + Ch 8 (HD)
  + Vasantham (HD)
* 570000 Hz 
  + Channel News Asia (HD)
  + Ch U (SD)
  + okto* (HD)
* 586000 Hz 
  + Ch U (HD)
  
## Alternative Frenquency
* 585000 Hz
  + Ch 5 (HD)
  + Suria (HD)
  + Ch 8 (HD)
  + Vasantham (HD)
* 610000 Hz
  + Ch 8 (HD)
  + Vasantham (HD)
* 698000 Hz  
  + Ch 8 (HD)
  + Vasantham (HD)
* 862000 Hz  
  + Ch 8 (HD)
  + Vasantham (HD)

## References
[raspberry pi pinout](https://pinout.xyz/#)

***TV HAT pinout (using SPI - Serial Peripheral Interface)***
* 5V - pin#2, pin#4;
* 3V3 - pin#1, pin#17 (not connected);
* GND - pin#6, pin#9, pin#14, pin#20, pin#25, pin#30, pin#34 pin,#39;
* SPI MOSI - pin#19;
* SPI MISO - pin#21;
* SPI SCLK - pin#23;
* SPI SC0 - pin#24;
* ID_SD - pin#27;
* ID_SC - pin#28.

[Sony CXD2880](https://elinux.org/images/2/2f/ELCE2018-poster-Sony-rpi-cxd2880.pdf)
Connecting Multiple Devices [SPI](http://www.learningaboutelectronics.com/Articles/Multiple-SPI-devices-to-an-arduino-microcontroller.php)

Connecting Multiple Devices [SPI] (http://www.learningaboutelectronics.com/Articles/Multiple-SPI-devices-to-an-arduino-microcontroller.php)

Raspberry Pi [Transcoding](https://www.raspberrypi.org/forums/viewtopic.php?t=227359)
TvHeadend [Transcoding](https://tvheadend.org/boards/5/topics/13892)

[pimylifeup](https://pimylifeup.com/raspberry-pi-exfat/)

## Attaching USB exfat drive
0. [Reference](https://www.shellhacks.com/raspberry-pi-mount-usb-drive-automatically/)
1. Format drive to exFAT on windows
2. Plug to pi
3. Install the module

``` 
$ sudo apt-get install exfat-fuse
$ sudo apt-get install exfat-utils
```

4. check the UUID

``` 
$ sudo blkid

or 

$ sudo lsblk -o UUID,NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL,MODEL
```

5. Edit fstab to auto mount on boot

```
$ sudo nano /etc/fstab
```

```
UUID=DE37-6D10 /media/exfat exfat defaults,auto,umask=000,users,rw 0 0
UUID=A697-5A33 /media/hdd_exfat exfat defaults,auto,umask=000,users,rw 0 0

```

7. create /media/exfat directory for mount

```
$ sudo mkdir /media/exfat
```

8. Reboot the pi

```
$ sudo shutdown -r now
```

9. Create a directory on USB drive 

```
$ sudo mkdir /media/exfat/recordings
```

6. Goto tvhendend [homepage](http://192.168.1.111:9981/extjs.html) to change the recording path to '/media/exfat/recordings'


## To unmount usb
```
$ sudo umount /mnt/usb0
```

## Prepare USB drive format
1. list the drive

```
$ sudo fdisk -l
```

2. edit the disk partition

```console
$ sudo fdisk /dev/sda*

# n,p,1,<enter>,<enter>,w
```

3. Format the partition

```
$ sudo mkfs -t ext4 /dev/sda1*
```

4. Create a directory to mount the filesystem

```
$ sudo mkdir /media/ext4
```

5. Mount the partition

```
$ sudo mount /dev/sda1* /media/ext4
```

## Find files

* Find large files (10MB+)

```
$ sudo find / -type f -size +10000k -exec ls -lh {} \; | awk '{ print $NF ": " $5 }' 
```

* Find largest folder (current working directory)

```
$ sudo du -hsx * | sort -rh | head -10 
```

## methods to copy files

* cp : normal local file copying
  + syntax : `cp <source> <dest>` 

* curl : copy to / from ftp / http / file etc with progress speed
  + syntax : `curl -o <dest> <source>`

```
$ curl -O ../hdd_extfat/recordings/file.mp4 file:///media/exfat/recordings/file1.mp4

# sftp protocol
$ curl -o ./a.ts -k "ftp://192.168.1.111/media/hdd_exfat/recordings/test.ts" --user "username:password"

```

* rsync : copying & sychronizing files and directories remotely and locally
  + syntax : `rsync <option> <source> <dest>`
  + `-v` : verbose
  + `-r` : recursive
  + `-a` : archive mode
  + `-z` : compress file data
  + `-h` : human-readable
  + `-e` : specify protocol (eg : ssh)
  + `--progress` : show progress 
  
```
$ rsync -avz ./recordings/file1.mp4 ../hdd_exfat/recordings/file.mp4

# from remote path
$ rsync -avz recordings/ root@192.168.1.111:/media/hdd_exfat/recordings

# via SSH from remote to local
$ rsync -avzhe ssh pi@192.168.1.111:/media/exfat/recordings ./recordings/
```

* tar : compress files into single file
  + syntax : `tar `
  + `-v` : verbose output
  + `-c` : create new archive
  + `-x` : extract files from archive
  + `-z` : filter archive throug gzip
  + `-j` : filter archive through bzip2
  + `-f file.tar.gz` : use archive file
  
```

``` 