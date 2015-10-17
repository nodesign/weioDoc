# How to extend WeIO flash space

Even if the flash space on WeIO is sufficient for most applications, when new python packages must be installed, or when the applications assets are important, the flash space available can fast become a problem.

To solve this problem, WeIO version 1.2 (and above) can be extended to a SD Card. This allow to increase the flash space from few megabytes to several gigabytes, depending of the SD card capacity.

Obviously, once the system is extended, the SD card must always stay plugged. Without it, the system will not boot anymore. Moreover, the future system updates will squash it and you will need to extend your system again.

## Pre-requisities

Before trying to extend your system, be sure to be able to connect to the board via the debug console. 

For Linux and OSX, this can be done with ```screen``` :

````screen /dev/ttyUSBx 115200 8N1```` under Linux systems, or ````screen /dev/tty.usbxxxxxx 115200 8N1```` under OSX.

On Windows system, [PuTTY](http://www.putty.org/) is known to work well. 

You also need an empty SD Card. If the SD Card is not empty, it will be erased during the processus. The filesystem in which the SD Card is formatted is not important.

## Prepare the system

To extend the root filesystem, it's strongly recommended to insert only the SD Card. All other external storage devices (USB HDD or USB Flash) must be disconnected of WeIO.

Via the debug console, connect to the board (and if needed, press enter) : the banner should be displayed :

```
##      ## ######## ####  #######  
##  ##  ## ##        ##  ##     ## 
##  ##  ## ##        ##  ##     ## 
##  ##  ## ######    ##  ##     ## 
##  ##  ## ##        ##  ##     ## 
##  ##  ## ##        ##  ##     ## 
 ###  ###  ######## ####  #######

  WEIO Web Of Things Platform
Copyright (C) 2013-2015 Nodesign.net,
Uros PETREVSKI, Drasko DRASKOVIC
     All rights reserved


root@WEIO:/# 
```

By default, removable devices are automatically mounted under /weioUser/. To control that your SD Card is correctly detected and mounted, you can type the command :

```
root@WEIO:/# df
Filesystem           1K-blocks      Used Available Use% Mounted on
rootfs                    3904       312      3592   8% /
/dev/root                11264     11264         0 100% /rom
tmpfs                    30812       276     30536   1% /tmp
/dev/mtdblock5            3904       312      3592   8% /overlay
overlayfs:/overlay        3904       312      3592   8% /
tmpfs                      512         0       512   0% /dev
/dev/sda1              3863720      8036   3639700   0% /weioUser/sd
```

The SD Card (```/dev/sda1```) is currently mounted on ```weioUser/sd```. 

The process that automatically check for removable device and mount them must be stopped :

```
root@WEIO:/# /etc/init.d/usd stop
```

And the SD Card can be unmounted :

```
root@WEIO:/# umount /weioUser/sd/
```

## Prepare the SD Card
At this point, the SD Card can be prepared. The first step is to format it :

```
root@WEIO:/# mkfs.ext4 -F /dev/sda1 
```

Once formatted, the SD Card can be mounted under ```/mnt```

```
root@WEIO:/# mount /dev/sda1 /mnt/
```

Now, the content of ```/overlay``` must be copied on the SD Card :

```
root@WEIO:/# tar -C /overlay -cvf - . | tar -C /mnt -xf -
```

And finally, the SD Card can be unmounted :

```
root@WEIO:/# umount /mnt/
```

## Set the system to use the SD Card
The last step is to setup the system to use the SD Card :

```
block detect > /etc/config/fstab
```

You'll now have to manually edit ```/etc/config/fstab```, with ```nano``` or ```vi``` :

```
root@WEIO:/# vi /etc/config/fstab
```

The file will looks like :

```
config 'global'
        option  anon_swap       '0'
        option  anon_mount      '0'
        option  auto_swap       '1'
        option  auto_mount      '1'
        option  delay_root      '5'
        option  check_fs        '0'

config 'mount'
        option  target  '/mnt/sda1'
        option  uuid    '0824a446-69a1-405a-b856-b276102fa6db'
        option  enabled '0'
```

The SD Card must be mounted on ```/overlay``` during the boot process. To do that, the file must be edited that way :

```
config 'global'
        option  anon_swap       '0'
        option  anon_mount      '0'
        option  auto_swap       '1'
        option  auto_mount      '1'
        option  delay_root      '5'
        option  check_fs        '0'

config 'mount'
        option  target  '/overlay'
        option  uuid    '0824a446-69a1-405a-b856-b276102fa6db'
        option  enabled '1'
```

Then, save the file. The system is now ready. You can reboot the system :

```
root@WEIO:/# reboot 
```

When the system has finished to boot, you can check that the system has been extended :

```
root@WEIO:/# df -h
Filesystem                Size      Used Available Use% Mounted on
rootfs                    3.7G      7.8M      3.5G   0% /
/dev/root                11.0M     11.0M         0 100% /rom
tmpfs                    30.1M     32.0K     30.1M   0% /tmp
/dev/sda1                 3.7G      7.8M      3.5G   0% /overlay
overlayfs:/overlay        3.7G      7.8M      3.5G   0% /
tmpfs                   512.0K         0    512.0K   0% /dev
```

In my case, I have now 3.5GB of free space !