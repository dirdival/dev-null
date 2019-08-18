---
title: "private USB with LUKS"
---

I still don't get why so many people don't use encrypted storages. I had a talk with friends about it, and
the most common answer is: _I would like to, but I don't know how it works_.

But come on, it's only an excuse. You use a car, computer, TV, and in many cases, you don't know how it works under
the hood. I would even say you don't have to know and it's fine. Complex things are hidden from us under _simple interfaces_.

This post is for people who afraid encryption. I would like to show you how to deal with it _without knowledge_.

---

# The idea

I would like to have private `USB`, used for storing secret things like passwords, `SSH` keys, and other valuable data.
Moreover, it has to be easy to use -- has a _simple interface_. When I start my computer with connected pendrive it should work like
any other ordinary `USB` storage. Encryption has to be _transparent_. Sounds good to me, let's start.

# Create LUKS partition

Linux Unified Key Setup, shorter [`LUKS`][link_luks_faq], is the simplest way to obtain encryption on `Linux`. To manage _everything_,
we need to know only `cryptsetup` tool. It has a great [`man` page][link_man_cryptsetup]. _The whole magic_ hides under a _simple interface_.
Let's see. I use my [`boot pendrive`]({{ site.baseurl }}{% post_url 2019-08-11-renew_boot %})
to be more specific the second partition from it. Due to my paranoia, before the main part, I
filled the partition with random data. It's not mandatory, but I strongly recommend it.

    dirdival@discworld:~$ sudo dd if=/dev/urandom of=/dev/sdd2 bs=4M
    dirdival@discworld:~$ sudo cryptsetup luksFormat /dev/sdd2
    
    WARNING!
    ========
    This will overwrite data on /dev/sdd2 irrevocably.
    
    Are you sure? (Type uppercase yes): YES
    Enter passphrase: 
    Verify passphrase: 

During creating of encrypted partition, `cryptsetup` asked about a password. Because it protects your data, please chose a good one.
Congratulation, that's all, we have it. Let's see how it looks:

    dirdival@discworld:~$ sudo cryptsetup open --type luks /dev/sdd2 private_usb
    Enter passphrase for /dev/sdd2: 
    dirdival@discworld:~$ ls -lhat /dev/mapper/private_usb 
    lrwxrwxrwx 1 root root 7 sie 15 12:03 /dev/mapper/private_usb -> ../dm-4
    dirdival@discworld:~$ sudo cryptsetup status private_usb 
    /dev/mapper/private_usb is active.
      type:    LUKS1
      cipher:  aes-xts-plain64
      keysize: 256 bits
      device:  /dev/sdd2
      offset:  4096 sectors
      size:    60436480 sectors
      mode:    read/write

I called it `privte_usb`, and after `open`, it's visible under path `/dev/mapper/private_usb`.
Rest of required things is very similar to an ordinary work with file systems.

# Create a private data file system

This part might be tough for some of you, but don't be afraid. I decided to have many private directories, mounted
in different places of my machine, instead of one secret catalog. To achieve this goal, I use [`LVM`][link_lvm] here,
but you can use any file system. I created `private_usb` volume group and `ssh` logical volume. After that, on `ssh`,
I created `ext4` partition.

    dirdival@discworld:~$ sudo vgcreate private_usb /dev/mapper/private_usb
      Physical volume "/dev/mapper/private_usb" successfully created.
      Volume group "private_usb" successfully created

    dirdival@discworld:~$ sudo lvcreate -n ssh -L 16M private_usb

    dirdival@discworld:~$ sudo mkfs.ext4 /dev/mapper/private_usb-ssh 
    mke2fs 1.43.4 (31-Jan-2017)
    Creating filesystem with 16384 1k blocks and 4096 inodes
    Filesystem UUID: 54faa466-e270-4b85-aa5f-614bc7c8eff5
    Superblock backups stored on blocks: 
        8193

    Allocating group tables: done                            
    Writing inode tables: done                            
    Creating journal (1024 blocks): done
    Writing superblocks and filesystem accounting information: done

I have more logical volumes, but to not scare readers, with unnecessary complexity, I omitted them.

# The first manual mount

Before the final configuration, I have to check that everything works and set proper user privileges for `ext4`.

    dirdival@discworld:~$ sudo mount /dev/mapper/private_usb-ssh /home/dirdival/tmp
    dirdival@discworld:~$ cd /home/dirdival/tmp
    dirdival@discworld:~/tmp$ ls -lhat
    razem 17K
    drwxr-xr-x 76 dirdival dirdival 4,0K sie 15 14:34 ..
    drwxr-xr-x  3 root     root     1,0K sie 15 12:54 .
    drwx------  2 root     root      12K sie 15 12:54 lost+found
    dirdival@discworld:~/tmp$ cd ..
    dirdival@discworld:~$ sudo chown -R dirdival:dirdival ./tmp/

Now in the `~/tmp` I have mounted my private partition. It's the proper time to fill it with valuable data. After that,
please remember to unmount it correctly.

    dirdival@discworld:~$ sudo umount /home/dirdival/tmp

# Ask about password during starting system

Let's go forward. Until now, we worked on the _opened_ (mapped to system) device. In a minute, we will configure the system.
Short term goal is to map the device under starting the Linux. We have to add a line to `/etc/crypttab`:

    private_usb UUID=bc7c0c08-45a4-41ef-a7b9-c99166ab1421 none luks

Quick explanation:

* `private_usb` -- refers to the name of the device used by system after proper mapping,
* `UUID=...` `UUID` of the device to map,
* `none` -- ask about password during mapping,
* `luks` -- the type of used encryption.

After save and restart of the computer, you should see prompt asking for the password to `private_usb`.

# Final transparent version

Typing password each time during boot is annoying, especially when you have to do it many times,
for different encrypted storages. Let's make the second password allows map our device, but this time from a file.

    dirdival@discworld:~$ dd if=/dev/urandom of=/home/dirdival/.PRIVATE_USB.key bs=4K count=1
    1+0 przeczytanych rekordów
    1+0 zapisanych rekordów
    4096 bajtów (4,1 kB, 4,0 KiB), 0,000298769 s, 13,7 MB/s
    
    dirdival@discworld:~$ chmod 400 .PRIVATE_USB.key 
    
    dirdival@discworld:~$ sudo cryptsetup luksAddKey /dev/sdd2 /home/dirdival/.PRIVATE_USB.key
    Enter any existing passphrase: 

What happened here? I created the file `.PRIVATE_USB.key`, filled with `4KB` of random data.
After that, I added it as the second key. One thing is worth mentioning here. You can have more than
one password. `LUKS` has 8 slots to store keys. You can manage them, switch off, etc. Right now, I use two slots:

    dirdival@discworld:~$ sudo cryptsetup luksDump /dev/sdd2 | grep 'Key Slot'
    Key Slot 0: ENABLED
    Key Slot 1: ENABLED
    Key Slot 2: DISABLED
    Key Slot 3: DISABLED
    Key Slot 4: DISABLED
    Key Slot 5: DISABLED
    Key Slot 6: DISABLED
    Key Slot 7: DISABLED

When I have the second password, stored in the file, now I can say system to use it during boot. It only requires to change
`none` from the `/etc/crypttab` into the correct path to the file, like:

    private_usb UUID=bc7c0c08-45a4-41ef-a7b9-c99166ab1421 /home/dirdival/.PRIVATE_USB.key luks

You may ask -- _Is it secure to store a password in a file?_ In my solution the `/home` comes from other encrypted storage.
It means that the file is only accessible after a valid mount.

---

The time has come to the last final part. We have to set-up `/etc/fstab`. As you know, it describes mount points. In my particular case, it
looks like this:

    dirdival@discworld:~$ cat /etc/fstab  | grep private_usb
    /dev/mapper/private_usb-gnupg /home/dirdival/.gnupg           ext4    rw,auto,nouser,async,noexec        0       2
    /dev/mapper/private_usb-password_store /home/dirdival/.password-store           ext4    rw,auto,nouser,async,noexec        0       2
    /dev/mapper/private_usb-ssh /home/dirdival/.ssh           ext4    rw,auto,nouser,async,noexec        0       2

After a restart, if everything went great, you should see mounted partitions:

    dirdival@discworld:~$ mount | grep private_usb
    /dev/mapper/private_usb-gnupg on /home/dirdival/.gnupg type ext4 (rw,noexec,relatime,data=ordered)
    /dev/mapper/private_usb-password_store on /home/dirdival/.password-store type ext4 (rw,noexec,relatime,data=ordered)
    /dev/mapper/private_usb-ssh on /home/dirdival/.ssh type ext4 (rw,noexec,relatime,data=ordered)

And the `boot pendrive` has this structure:

    dirdival@discworld:~$ lsblk /dev/sdd
    NAME                             MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
    sdd                                8:48   1 29,3G  0 disk  
    ├─sdd1                             8:49   1  487M  0 part  /boot
    └─sdd2                             8:50   1 28,8G  0 part  
      └─private_usb                  253:4    0 28,8G  0 crypt 
        ├─private_usb-ssh            253:5    0   16M  0 lvm   /home/dirdival/.ssh
        ├─private_usb-password_store 253:6    0   16M  0 lvm   /home/dirdival/.password-store
        └─private_usb-gnupg          253:7    0   16M  0 lvm   /home/dirdival/.gnupg

# Extra tips and one LVM problem

Please remember to perform a backup of the whole `USB` frequently. It might fail, like each device, and access
to encrypted data will be limited or even impossible.

When I clone/copy the `USB`, I prefer to use `dd`. Before the operation, you have to unmount all related mount points and _close_
`LUKS` partition. If you decided to use `LVM` like me, then you may be stuck on one annoying problem. During manual _closing_ 
`cryptsetup` shows:

    dirdival@discworld:~$ sudo cryptsetup close /dev/mapper/private_usb
    device-mapper: remove ioctl on private_usb failed: Device or resource busy
    device-mapper: remove ioctl on private_usb failed: Device or resource busy
    ...
    Device /dev/mapper/private_usb is still in use.

It happens because `Linux` still refers to `LVM` volume. You can resolve this problem, just remove the problematic group:

    dirdival@discworld:~$ sudo vgchange -an private_usb
      0 logical volume(s) in volume group "private_usb" now active

After that command `close` should work:

    dirdival@discworld:~$ sudo cryptsetup status /dev/mapper/private_usb
    /dev/mapper/private_usb is active.
      type:    LUKS1
      cipher:  aes-xts-plain64
      keysize: 256 bits
      device:  /dev/sdd2
      offset:  4096 sectors
      size:    60436480 sectors
      mode:    read/write
    dirdival@discworld:~$ sudo cryptsetup close  /dev/mapper/private_usb
    dirdival@discworld:~$ sudo cryptsetup status /dev/mapper/private_usb
    /dev/mapper/private_usb is inactive.

---

If you are still here, I suppose that you enjoyed the post. I also hope that I hadn't scared you too much with `LUKS`.

[link_luks_faq]:https://gitlab.com/cryptsetup/cryptsetup/wikis/FrequentlyAskedQuestions
[link_man_cryptsetup]:http://man7.org/linux/man-pages/man8/cryptsetup.8.html "man page for cryptsetup"
[link_lvm]:https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux) "wikipedia: LVM"
