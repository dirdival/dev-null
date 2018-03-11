---
title: "5% more for free"
---

This week I learned something new about [`Extended File System`][ext_link].
On Thursday I received an email alert from administrator that one's machine belongs
to my team exceeded 90% of disk usage. It happened us first time and
I was a little confused. Since a few months we are working on a new machine,
moreover after migration we have much more disk space.
On Friday went _on production_ a new version of our software which extracts core dumps,
checks backtracks and creates or update reports in issue tracking system.
I decided that _an investigation_ should be performed by the developer responsible
for last upgrade. It wasn't hard to check what is wrong, just login into that machine
and review sizes of all directories. One hour later I had a report that it was an _ordinary_
issue -- we don't clean up cache and it's full of old data, but there was something more.
The developer mentioned that we lose _somewhere_ 5% of whole free space. _It sounds strange_ --
I said. I mentioned that `Extended File System`, `ext4` in this case, like each file system
takes part of a disk to store metadata like i-nodes. But, according to developer it was something
different. He showed me calculations -- sizes of directories, free space -- and I had to admit
that he was right. 

After the stand-up meeting he said that he found out what it going on. When you create a new
partition by default 5% of it's reserved for system -- more precisely -- 5% of blocks. Detailed
description comes from `man` for `tune2fs`:

    -m reserved-blocks-percentage
           Set the percentage of the filesystem which may only be allocated by privileged processes.
           Reserving some number of filesystem blocks for  use  by  privileged  processes  is  done to
           avoid filesystem fragmentation, and to allow system daemons, such as syslogd(8), to continue
           to function correctly after non-privileged processes are prevented from writing to the filesystem.
           Normally, the default percentage of reserved blocks is 5%.
    
You may say that 5% it's small part of disk and we shouldn't care about it. As always everything depends on usage.
If it's a one big partition for everything, system and data, like at work, then I agree -- we decided to not change it.
But, when you have many partitions, dedicated only for data or casual usb stick then you wish use whole available space.
Small example from my usb backup disk, it has 1TB:

    dirdival@discworld:~$ sudo tune2fs -l /dev/sdd1
    ...
    Filesystem OS type:       Linux
    Inode count:              61054976
    Block count:              244190208
    Reserved block count:     12209510
    Free blocks:              63679274
    Free inodes:              61054956
    First block:              0
    Block size:               4096
    ...

Quick calculation shows that I'm wasting `47693MB`, more or less `46GB`.

    >>> (12209510 * 4096) / 1024**2
    47693
    >>> 47693 / 1024
    46

In this case scale has a meaning, almost `47GB` more for free makes a different for a backup disk.
From that purpose I decided to change that value into `0`, it's easy to obtain:

    dirdival@discworld:~$ sudo tune2fs -m 0  /dev/sdd1

After that I have:

    dirdival@discworld:~$ sudo tune2fs -l /dev/sdd1
    ...
    Filesystem OS type:       Linux
    Inode count:              61054976
    Block count:              244190208
    Reserved block count:     0
    Free blocks:              63679274
    Free inodes:              61054956
    First block:              0
    ...

And small proof that it was good idea. Before change:

	dirdival@discworld:~$ df -h
	...
	/dev/sdd1                  917G  674G  197G  78% /media/cdrom

and after:

    dirdival@discworld:~$ df -h
    ...
    /dev/sdd1                  917G  674G  243G  74% /media/cdrom

Of course I made change on unmounted partition.

---
Thank you Marcin for your deeper investigation, I really appreciate it.

[ext_link]:https://en.wikipedia.org/wiki/Extended_file_system "wikipedia:Exteneded File System"
