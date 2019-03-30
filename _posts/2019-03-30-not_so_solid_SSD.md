---
title: "not so solid SSD"
---

I observed first signals of problem two or three weeks ago. Desktop booting took
longer than before. Not each time but it had happened. First glance into
system logs had shown nothing. I decided to look deeper into it on the next weekend.
In the meantime, new symptoms of problem occurred. 
The OS system was working, suddenly stopped for several seconds and started working
as usual. I couldn't wait for the next weekend, I started digging and I found part
of [_dmesg_][link_dmesg] logs full of red

    [   81.166494] ata2.00: exception Emask 0x0 SAct 0x14000000 SErr 0x0 action 0x6 frozen
    [   81.166588] ata2.00: failed command: READ FPDMA QUEUED
    [   81.166671] ata2.00: cmd 60/08:d0:80:ff:73/00:00:07:00:00/40 tag 26 ncq dma 4096 in
                            res 40/00:00:00:00:00/00:00:00:00:00/00 Emask 0x4 (timeout)
    [   81.166789] ata2.00: status: { DRDY }
    [   81.166856] ata2.00: failed command: READ FPDMA QUEUED
    [   81.166920] ata2.00: cmd 60/01:e0:00:00:00/00:00:00:00:00/40 tag 28 ncq dma 512 in
                            res 40/00:00:00:00:00/00:00:00:00:00/00 Emask 0x4 (timeout)
    [   81.167003] ata2.00: status: { DRDY }
    [   81.167063] ata2: hard resetting link

According to mister _google_ and [its first answers][link_first_answer] problem was related
to _SATA connector_. I quickly checked which disk might has a problem with a connector.

    dirdival@discworld:~$ ls -l /sys/block/sd* | grep ata2
    lrwxrwxrwx 1 root root 0 mar 21 18:37 /sys/block/sdb -> ../devices/pci0000:00/0000:00:01.3/0000:03:00.1/ata2/host1/target1:0:0/1:0:0:0/block/sdb
    dirdival@discworld:~$ sudo lsblk
    [sudo] hasło użytkownika dirdival: 
    NAME                  MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
    sda                     8:0    0  59,6G  0 disk  
    └─sda1                  8:1    0  59,6G  0 part  
      └─md0                 9:0    0  59,6G  0 raid1 
        └─md0_crypt       253:0    0  59,6G  0 crypt 
          └─system-system 253:1    0  59,6G  0 lvm   /
    sdb                     8:16   0  59,6G  0 disk  
    └─sdb1                  8:17   0  59,6G  0 part  
      └─md0                 9:0    0  59,6G  0 raid1 
        └─md0_crypt       253:0    0  59,6G  0 crypt 
          └─system-system 253:1    0  59,6G  0 lvm   /
    sdc                     8:32   0 465,8G  0 disk  
    └─sdc1                  8:33   0 465,8G  0 part  
      └─md1                 9:1    0 465,7G  0 raid1 
        └─md1_crypt       253:2    0 465,6G  0 crypt 
          └─data-data     253:3    0 465,6G  0 lvm   /home
    sdd                     8:48   1   7,5G  0 disk  
    └─sdd1                  8:49   1   7,5G  0 part  /boot
    sde                     8:64   0 465,8G  0 disk  
    └─sde1                  8:65   0 465,8G  0 part  
      └─md1                 9:1    0 465,7G  0 raid1 
        └─md1_crypt       253:2    0 465,6G  0 crypt 
          └─data-data     253:3    0 465,6G  0 lvm   /home

It was _Solid State Drive_. I replanted cables between _sda_ and _sdb_, I tried the new one, but
after each change problem was still present.

I checked disk with [smartmontools][link_smartmontools], but according to them storage
was fine

    dirdival@discworld:~$ sudo smartctl -i -a /dev/sdb
    smartctl 6.6 2016-05-31 r4324 [x86_64-linux-4.9.0-8-amd64] (local build)
    Copyright (C) 2002-16, Bruce Allen, Christian Franke, www.smartmontools.org
    
    === START OF INFORMATION SECTION ===
    Model Family:     JMicron based SSDs
    ...
    Firmware Version: 5.9
    User Capacity:    64,022,175,232 bytes [64,0 GB]
    Sector Size:      512 bytes logical/physical
    Rotation Rate:    Solid State Device
    ...
    SMART support is: Available - device has SMART capability.
    SMART support is: Enabled
    
    === START OF READ SMART DATA SECTION ===
    SMART overall-health self-assessment test result: PASSED
    ...

---

Because I was in a pickle, I tried to reduce number of possible problems.
I ordered a _power supply tester_. On the weekend I checked with it all _SATA connectors_
and voltage was valid on each cable.

Finally, I decided to make the last thing. _sdb1_ is part of _/dev/md0_ (_RAID1_). I marked
it as fail, removed from array, and added again to synchronise matrix.

    dirdival@discworld:~$ sudo mdadm  /dev/md0 -f /dev/sdb1
    dirdival@discworld:~$ sudo mdadm /dev/md0 -r /dev/sdb1
    mdadm: hot removed /dev/sdb1 from /dev/md0
	dirdival@discworld:~$ cat /proc/mdstat 
	Personalities : [raid1] [linear] [multipath] [raid0] [raid6] [raid5] [raid4] [raid10] 
	md1 : active raid1 sde1[1] sdc1[0]
		  488253440 blocks super 1.2 [2/2] [UU]
		  bitmap: 2/4 pages [8KB], 65536KB chunk

	md0 : active raid1 sda1[0]
		  62487552 blocks super 1.2 [2/1] [U_]
	dirdival@discworld:~$ sudo mdadm /dev/md0 -a /dev/sdb1
	mdadm: added /dev/sdb1
	dirdival@discworld:~$ cat /proc/mdstat 
	Personalities : [raid1] [linear] [multipath] [raid0] [raid6] [raid5] [raid4] [raid10] 
	md1 : active raid1 sde1[1] sdc1[0]
		  488253440 blocks super 1.2 [2/2] [UU]
		  bitmap: 2/4 pages [8KB], 65536KB chunk

	md0 : active raid1 sdb1[2] sda1[0]
		  62487552 blocks super 1.2 [2/1] [U_]
		  [>....................]  recovery =  0.2% (135744/62487552) finish=38.2min speed=27148K/sec

During recovery, I started observing well known logs

	[21085.000832] ata2.00: status: { DRDY }
	[21085.000837] ata2: hard resetting link
	[21085.476822] ata2: SATA link up 1.5 Gbps (SStatus 113 SControl 310)
	[21085.486973] ata2.00: configured for UDMA/33
	[21085.486990] ata2.00: device reported invalid CHS sector 0
	[21085.487004] ata2: EH complete
	[21177.168343] ata2.00: exception Emask 0x0 SAct 0x0 SErr 0x0 action 0x6 frozen
	[21177.168352] ata2.00: failed command: WRITE DMA EXT
	[21177.168362] ata2.00: cmd 35/00:00:00:8f:5d/00:05:05:00:00/e0 tag 1 dma 655360 out
							res 40/00:00:00:00:00/00:00:00:00:00/00 Emask 0x4 (timeout)
	[21177.168366] ata2.00: status: { DRDY }

and the speed of operation started falling down dramatically

	[>....................]  recovery =  0.2% (135744/62487552) finish=38.2min speed=27148K/sec
	[>....................]  recovery =  0.2% (135744/62487552) finish=76.5min speed=13574K/sec
	[>....................]  recovery =  0.3% (219776/62487552) finish=99.1min speed=10465K/sec
	[>....................]  recovery =  0.9% (590784/62487552) finish=162.3min speed=6352K/sec
	[>....................]  recovery =  1.1% (721152/62487552) finish=178.3min speed=5769K/sec
	[>....................]  recovery =  1.8% (1175808/62487552) finish=174.2min speed=5862K/sec
	[>....................]  recovery =  4.6% (2931904/62487552) finish=230.6min speed=4303K/sec
	[=>...................]  recovery =  6.5% (4102528/62487552) finish=228.2min speed=4263K/sec
	[==>..................]  recovery = 11.4% (7155328/62487552) finish=222.0min speed=4152K/sec
	[===>.................]  recovery = 15.9% (9951040/62487552) finish=228.0min speed=3839K/sec
	[====>................]  recovery = 20.0% (12553024/62487552) finish=307.2min speed=2708K/sec
	[====>................]  recovery = 21.2% (13268608/62487552) finish=362.3min speed=2263K/sec
	[====>................]  recovery = 24.6% (15412736/62487552) finish=129.2min speed=6065K/sec
	[=====>...............]  recovery = 25.2% (15800896/62487552) finish=351.5min speed=2212K/sec
	[=====>...............]  recovery = 25.3% (15865600/62487552) finish=470.1min speed=1652K/sec
	[=====>...............]  recovery = 27.0% (16891712/62487552) finish=128.9min speed=5891K/sec
	[=====>...............]  recovery = 27.0% (16908992/62487552) finish=128.3min speed=5915K/sec
	[=====>...............]  recovery = 27.6% (17258944/62487552) finish=92.1min speed=8180K/sec
	[=====>...............]  recovery = 28.4% (17774272/62487552) finish=15.6min speed=47700K/sec
	[=====>...............]  recovery = 28.6% (17888256/62487552) finish=18.2min speed=40786K/sec
	[=====>...............]  recovery = 29.0% (18148672/62487552) finish=52.6min speed=14029K/sec
	[===========>.........]  recovery = 56.2% (35142528/62487552) finish=232.2min speed=1961K/sec

It was late, I was tired so I went to sleep.

---

In the morning desktop was still working and I hoped that recovery procedure was completed.
The first thing which I observed was an email

	From root@discworld Sun Mar 24 00:36:49 2019
	Return-path: <root@discworld>
	Envelope-to: root@discworld
	Delivery-date: Sun, 24 Mar 2019 00:36:49 +0100
	Received: from root by discworld with local (Exim 4.89)
			(envelope-from <root@discworld>)
			id 1h7qC0-0005Gl-Ul
			for root@discworld; Sun, 24 Mar 2019 00:36:49 +0100
	From: mdadm monitoring <root@discworld>
	To: root@discworld
	Subject: FailSpare event on /dev/md/0:discworld
	Message-Id: <E1h7qC0-0005Gl-Ul@discworld>
	Date: Sun, 24 Mar 2019 00:36:48 +0100
	X-UID: 59
	Status: RO
	Content-Length: 702
	Lines: 21

	This is an automatically generated mail message from mdadm
	running on discworld

	A FailSpare event had been detected on md device /dev/md/0.

	It could be related to component device /dev/sdb1.

	Faithfully yours, etc.

	P.S. The /proc/mdstat file currently contains the following:

	Personalities : [raid1] [linear] [multipath] [raid0] [raid6] [raid5] [raid4] [raid10] 
	md1 : active raid1 sde1[1] sdc1[0]
		  488253440 blocks super 1.2 [2/2] [UU]
		  bitmap: 0/4 pages [0KB], 65536KB chunk

	md0 : active raid1 sdb1[2](F) sda1[0]
		  62487552 blocks super 1.2 [2/1] [U_]
		  [==============>......]  recovery = 72.0% (45039936/62487552) finish=83.6min speed=3471K/sec
		  
	unused devices: <none>

I analysed _dmesg_ logs and I found

	[21322.588249] ata2.00: status: { DRDY }
	[21322.588254] ata2: hard resetting link
	[21332.605002] ata2: softreset failed (device not ready)
	[21332.605010] ata2: hard resetting link
	[21342.621815] ata2: softreset failed (device not ready)
	[21342.621823] ata2: hard resetting link
	[21353.178677] ata2: link is slow to respond, please be patient (ready=0)
	[21377.668678] ata2: softreset failed (device not ready)
	[21377.668687] ata2: hard resetting link
	[21382.881262] ata2: softreset failed (device not ready)
	[21382.881271] ata2: reset failed, giving up
	[21382.881275] ata2.00: disabled
	[21382.881304] ata2: EH complete
	[21382.881337] sd 1:0:0:0: [sdb] tag#6 FAILED Result: hostbyte=DID_BAD_TARGET driverbyte=DRIVER_OK
	[21382.881342] sd 1:0:0:0: [sdb] tag#6 CDB: Synchronize Cache(10) 35 00 00 00 00 00 00 00 00 00
	[21382.881346] blk_update_request: I/O error, dev sdb, sector 2056
	[21382.881350] md: super_written gets error=-5
	[21382.881354] md/raid1:md0: Disk failure on sdb1, disabling device.
				   md/raid1:md0: Operation continuing on 1 devices.

The funniest thing of this story is that _smartctl_ still claims that _sdb_ doesn't have any problems and
prints the same output as before. To be honest, the device is still working but a small comparison with the corresponding
device shows that something is wrong

	root@discworld:/home/dirdival# hdparm -t --direct /dev/sda

	/dev/sda:
	 Timing O_DIRECT disk reads: 858 MB in  3.00 seconds = 285.69 MB/sec

	root@discworld:/home/dirdival# hdparm -t --direct /dev/sdb

	/dev/sdb:
	 Timing O_DIRECT disk reads:   2 MB in 19.21 seconds = 106.63 kB/sec

2MB in almost 20 seconds. Really solid drive.

---

Lessons I learned from this accident:

* _SSD_ is not so solid. I had a lot of hard drives and only this one failed very quickly.
* _smartctl_ sucks. This message:

    SMART support is: Available - device has SMART capability.
    SMART support is: Enabled
    ...
    SMART overall-health self-assessment test result: PASSED

means nothing in this particular example.
* I have to find out more about examining _SSD_ health.
* To sleep well at night _RAID1_ is mandatory.

[link_dmesg]:https://en.wikipedia.org/wiki/Dmesg "wikipedia: dmesg"
[link_first_answer]:https://ubuntuforums.org/showthread.php?t=2272486 "failed command: READ FPDMA QUEUED" for my WD10EZEX-08M2NA0 drive."
[link_smartmontools]:https://www.smartmontools.org "Smartmontools - main page"

