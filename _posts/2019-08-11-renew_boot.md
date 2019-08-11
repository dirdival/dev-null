---
title: "renew /boot"
---

A year ago, I had fun with the set-up of
[`discworld`]({{ site.baseurl }}{% post_url 2018-02-18-discworld %}). After the accident with
[`not so solid SDD`]({{ site.baseurl }}{% post_url 2019-03-30-not_so_solid_SSD %}), I started looking closer at my desktop.
I made some small configuration mistakes, and I'm going to fix all of them, one by one.

---

Let's start with `/boot`. What's wrong with it? I use whole disk space from `USB` drive to store boot loader with required files.

    dirdival@discworld:~$ lsblk /dev/sdd
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdd      8:48   1  7,5G  0 disk
    └─sdd1   8:49   1  7,5G  0 part /boot
    dirdival@discworld:~$ du -sh /boot/
    65M	/boot/

The idea is to reduce the size of the partition and have more space for storing other data. I'm not going to work with
my `key-boot disk`, even if I have a backup of it. Let's start with a new one pendrive.

---

The first thing is simple. Create the a new partition and set the same file system.

    dirdival@discworld:~$ cat /etc/fstab  | grep boot
	# /boot was on /dev/sdf1 during installation
	UUID=35fac341-4759-491b-8a07-e24f1768417d /boot           ext2    defaults        0       2

I have the new fresh device

    dirdival@discworld:~$ lsblk  /dev/sdf
    NAME MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdf    8:80   1 29,3G  0 disk

which is not empty:

	dirdival@discworld:~$ sudo parted /dev/sdf
	GNU Parted 3.2
	Using /dev/sdf
	Welcome to GNU Parted! Type 'help' to view a list of commands.
	(parted) print
	Model: General UDisk (scsi)
	Disk /dev/sdf: 31.5GB
	Sector size (logical/physical): 512B/512B
	Partition Table: loop
	Disk Flags:
    
	Number  Start  End     Size    File system  Flags
	 1      0.00B  31.5GB  31.5GB  ntfs
    
	(parted)

Get rid of the default configuration:

    (parted) rm 1
    (parted) mktable msdos
    Warning: The existing disk label on /dev/sdf will be destroyed and all data on this disk
	will be lost. Do you want to continue?
    Yes/No? Yes
    (parted) print
    Model: General UDisk (scsi)
    Disk /dev/sdf: 31.5GB
    Sector size (logical/physical): 512B/512B
    Partition Table: msdos
    Disk Flags:
    
    Number  Start  End  Size  Type  File system  Flags
    
    (parted)

After that, we simply create the first small partition and set the `boot` flag on it.

    (parted) mkpart primary
    File system type?  [ext2]? ext2
    Start? 2048s
    End? 512MB
    (parted) print
    Model: General UDisk (scsi)
    Disk /dev/sdf: 31457MB
    Sector size (logical/physical): 512B/512B
    Partition Table: msdos
    Disk Flags:
    
    Number  Start   End    Size   Type     File system  Flags
     1      1.05MB  512MB  511MB  primary  ext2         lba
    
    (parted) toggle 1 lba
    (parted) toggle 1 boot
    (parted) print
    Model: General UDisk (scsi)
    Disk /dev/sdf: 31457MB
    Sector size (logical/physical): 512B/512B
    Partition Table: msdos
    Disk Flags:
    
    Number  Start   End    Size   Type     File system  Flags
     1      1.05MB  512MB  511MB  primary  ext2         boot
    
    (parted) quit

One thing may look odd here, start value `2048s`. If you are interested, follow
[Ian Chard post][link_parted] for more details.

---

The second part is also obvious, we create proper file system and copy data from `/boot`.

    dirdival@discworld:~$ sudo mkfs.ext2 /dev/sdf1
    mke2fs 1.43.4 (31-Jan-2017)
    Creating filesystem with 498688 1k blocks and 124928 inodes
    Filesystem UUID: 51afab14-a5fc-423f-bb67-f62f614ee096
    Superblock backups stored on blocks:
    	8193, 24577, 40961, 57345, 73729, 204801, 221185, 401409
    
    Allocating group tables: done
    Writing inode tables: done
    Writing superblocks and filesystem accounting information:
    done
    
    dirdival@discworld:~$ sudo mount /dev/sdf1 /media/usb
    dirdival@discworld:~$ cd /boot
    dirdival@discworld:/boot$ sudo cp -rp ./ /media/usb/
    dirdival@discworld:/boot$ sudo umount /media/usb

Is it all? Not yet. The time has come for non-clear things which are mandatory.
Even if we have all the data and we set `boot` flag, it doesn't mean that it works. We need a boot loader.

    dirdival@discworld:~$ sudo grub-install /dev/sdf
    Installing for i386-pc platform.
    Installation finished. No error reported.
    dirdival@discworld:~$

And the last thing. Let's see how current configuration for `/boot` looks like:

    dirdival@discworld:~$ cat /etc/fstab | grep boot
    # /boot was on /dev/sdf1 during installation
    UUID=35fac341-4759-491b-8a07-e24f1768417d /boot           ext2    defaults        0       2

It's required the specific `UUID`. Our new device has a new one:

    dirdival@discworld:~$ lsblk -f /dev/sdf1
    NAME FSTYPE LABEL UUID                                 MOUNTPOINT
    sdf1 ext2         51afab14-a5fc-423f-bb67-f62f614ee096

Here we can do two things. We can change `/etc/fstab` to use the new one `UUID`, or we can preserve the old one.
I prefer the second solution because in case of any problem we can use the old boot-key device.
But wait, there is one more thing. We can't have two partitions with the same `UUID`. This `magic` number is used to recognise
devices. When you have two partitions, with the same number, only one is visible. Right now we use this `UUID`:

    dirdival@discworld:~$ lsblk /dev/sdd -f
    NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
    sdd
    └─sdd1 ext2         35fac341-4759-491b-8a07-e24f1768417d /boot

How to deal with it? We have to unmount `/boot`, unplug it, and set `UUID` for the first partition on the new device.

    sudo umount  /boot
    dirdival@discworld:~$ lsblk /dev/sdd -f
    lsblk: /dev/sdd: not a block device

Before the change `/dev/sdf1` has this `UUID`

    dirdival@discworld:~$ lsblk -f /dev/sdf1
    NAME FSTYPE LABEL UUID                                 MOUNTPOINT
    sdf1 ext2         51afab14-a5fc-423f-bb67-f62f614ee096

Change, very simple:

    dirdival@discworld:~$ sudo tune2fs /dev/sdf1 -U 35fac341-4759-491b-8a07-e24f1768417d

and check:

    dirdival@discworld:~$ lsblk -f /dev/sdf1
    NAME FSTYPE LABEL UUID                                 MOUNTPOINT
    sdf1 ext2         35fac341-4759-491b-8a07-e24f1768417d /boot

How you can see `Debian` even assumes that we have the boot device plugged-in.
We can make one [extra change]({{ site.baseurl }}{% post_url 2018-03-10-5_percent_more_for_free %})
to gain more space.

    dirdival@discworld:~$ df -h /dev/sdf1
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/sdf1       472M   67M  381M  15% /boot
    dirdival@discworld:~$ sudo tune2fs -m 0 /dev/sdf1
    dirdival@discworld:~$ df -h /dev/sdf1
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/sdf1       472M   67M  406M  15% /boot

---

After the restart, we have the new boot-key device with plenty of free space for further use. Was it worth it? From my point of view, definitely yes.

[link_parted]:https://rainbow.chard.org/2013/01/30/how-to-align-partitions-for-best-performance-using-parted/ "How to align partitions for best performance using parted"
