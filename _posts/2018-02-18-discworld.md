---
title: "Discworld"
---


Almost exactly 10 years ago I made this desktop set:

    Intel(R) Core(TM)2 Duo CPU     E7400  @ 2.80GHz
    NVIDIA Corporation G94 [GeForce 9600 GT] (rev a1)
    4GB of RAM

In that days it was something. I didn't chose the newest CPU and graphic card but
quite near _the top_. And surprisely it works quite well, even now, until you are not
interested AAA games.

---

Before the Christmas the [Steam released information][link_steam_survey] about hardware used by gamers.
Then I found that I'm in the minority of minority -- as a Linux user and as a graphic
card user. In January 2018 _GeForce 9600 GT_ used only 0.16% players. I'd like to
play from time to time, mostly on games released ages ago but last year I had to admit
that I bought a few games on Steam and I couldn't _tweak_ them to start.

---

There are always some excuses to not touch hardware and software. I [had bad experience]({{ site.baseurl }}{% post_url 2017-09-17-Linux-distributions-problems %})
with upgrading software on many Linux distributions. From the same reason I avoid changing
hardware. _Fun_ with black screen it's OK when you are a student and have a lot of time,
but when your computer becomes your _working place_ then you look on it in different way
and wish to have a stable environment. Even year ago I was thinking and talking about
new desktop but I didn't do anything. Near the Christmas I _received a sign_ from my
graphic card. After a few hours of working I saw glitches on monitor and then I knew that
_the time has come_.

---

Fortunately also my colleague Paweł resolved to change a hardware. We had several chit-chats
over coffee and I decided to take the same set like his. I was ready for shopping and suddenly that happened:

![Price chart of GeForce GTX 1050 Ti ]({{ "/assets/images/2018-02-18_geforce_gtx_1050_ti.png" | absolute_url }})

Prices of graphic cards [increased dramatically during a few weeks][link_pcpartpicker_geforce]. Moreover they were not available.
Cause of that was Bitcoin -- it broke _10.000$_ and this _magic value_ bring that people started buy
graphic cards to mine money. I waited a little to February until enthusiasm falls and on Thursday I gathered
all items to mount from scratch new desktop.

---

The _technology_ has been changed during 10 years so I watched several movies on youtube about desktop installation.
The most surprising thing for me was the new place for a power supply -- in my old desktop it's on the top.
In the new one it's on the bottom, it's built-up. Other things unfortunately didn't
change. Connecting panel header -- power, reset switch, etc. -- nightmare. After all, I connected all things together,
I started the power button and ... nothing. To be more specific -- CPU fan was working, other fans too, but I didn't
see anything on the screen and beeper didn't give me any clue. I switched off all drives, started again and ... still _I see
darkness_. I get rid of graphic card -- the beeper started the song -- and it was about _no graphic card_. OK, I know
at least two things -- beeper is connected properly and the graphic card was connected properly too. But why I don't see
anything on the screen? I started googling to find out more about _black screen_ on my new motherboard. And I found!
Cause of my problem was the UEFI, yet another _new thing_ which I have to know. Long story short, after restart of CMOS
I saw the BIOS. After that things went faster. I set software _RAID 1_ twice -- once for system and second for data:

    dirdival@discworld:~$ cat /proc/mdstat 
    Personalities : [raid1] [linear] [multipath] [raid0] [raid6] [raid5] [raid4] [raid10] 
    md1 : active raid1 sdd1[1] sdc1[0]
          488253440 blocks super 1.2 [2/2] [UU]
          bitmap: 1/4 pages [4KB], 65536KB chunk
    
    md0 : active raid1 sda1[0] sdb1[1]
          62487552 blocks super 1.2 [2/2] [UU]
          
    unused devices: <none>

Each raid device is encrypted (cryptosetup) and on that I set the LVM with ext4 for _system_ and _data_. Moreover, I set _/boot_
from _USB_ -- we might call it _dongle_. Without this _key_ you can't boot desktop and read data, because system and data are encrypted.
I know that it sounds crazy, but it's easy to obtain even with default Debian wizard. If you are interested this setup please follow this two films
by Rex Kneisley (Crypto dad): [_Installing Debian Linux with full-disk encryption and a key-boot disk (Part 2)_][link_crypto_dad_part_2] and [_Part 3_][link_crypto_dad_part_3].

---

After boot the new machine is ready for work damn fast. Did I mention that I didn't have SSD disk yet?

And finally the most important info about _discworld_:

    AMD Ryzen 7 1700 Eight-Core Processor
    NVIDIA Corporation GP107 [GeForce GTX 1050 Ti] (rev a1)
    8GB of RAM

![result of htop]({{ "/assets/images/2018-02-18_htop.png" | absolute_url }})

For today it's enough, but it's only part of the story about new host -- _discworld_. Right now I have to install drivers, programs
 and migrate all important data retained on _pld_ host.


[link_steam_survey]:http://store.steampowered.com/hwsurvey/videocard/ "Steam Hardware & Software Survey: January 2018"
[link_pcpartpicker_geforce]:https://pcpartpicker.com/trends/price/video-card/#gpu.chipset.geforce-gtx-1050-ti "Price chart of GeForce GTX 1050 Ti"
[link_crypto_dad_part_2]:https://www.youtube.com/watch?v=yJdBIigQcVw "Installing Debian Linux with full-disk encryption and a key-boot disk (Part 2)"
[link_crypto_dad_part_3]:https://www.youtube.com/watch?v=JyA4owRVUls "Installing Debian Linux with full-disk encryption and a key-boot disk (Part 3)"
