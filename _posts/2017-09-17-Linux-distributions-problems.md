---
title: "Linux distributions problems"
---

I use Linux since many years. Like many other developers I was keen on it
on studies. Learning new operating system quickly involved me. I started
my journey with [PLD distribution](https://www.pld-linux.org),
commonly used in  my academy. I remember
like today ... I said to myself _Let's start_ and during weekend I was reading
all posts from PLD forum -- yes, all of them, one by one. Second step, which
probably makes each _serious_ Linux user is switch from currently used system
into Linux. In some day you just realise that you work more often on Linux than for example
Windows. Most of people stays on that level, I made a step forward and decided
to become a PLD developer. After many reported patches, few new Spec files
developers _addmited me to their body_. As you might expect, I was overjoyed,
still I committed patches, I tracked when new version of software where released
and bumped it in my distribution. Such ordinary work as serial developer.

But, when I cooled of a little, reflection came _what am I doing?_ I searched
many Linux forums in exploration clues how to fix some issue. And what is obvious
_I found out_ that many other developers make exactly the same work like me, but
in favour of other distribution. When I realized that in few days I decided
to stop working for PLD.

Sad true about Linux is that so many people make the same work, in other words -- _wasteing time_
instead of creating new things. Please don't get me wrong, I don't mean Linux distributions
diversity. It's great when people have different approach to the same problem,
but they should work together and learn something from each other. This mainly applies
to different software package management systems: `rpm`, `deb`, `tar`. You can't
_just like that_ install package from one system into another. Even worse.
Many distributions use the same software package manager system but what is ridiculous you can't install packages
between distributions, because they have:
* different name of packages (requirements),
* different paths where files are installed,
* even different rules for setting package version!

Of course, there are half measures,
which allows you _transplant_ package, but they don't work on each package. For today I don't
see any simple solution of that problem. Obviously _big_ distributions have processing power
and they decide who will be on the market. If they start working together then gains
of they work will be shared between all of them. But, are they interested to cooperate?
I don't think so. And to be clear, I'm not talking about monopoly on package management system.
I think about consistent way to describe building package which might be translated
into each system, like _Yaml_ might be translated into _Json_ and vice verse.

Longer usage of Linux distribution might lead you to second problem. At some
point you should (sometimes you have to) upgrade your current version into new release.
To be honest, only on PLD (on one machine) I made upgrades from _Ra_ (1.0) to _Ac_ (2.0) and
finally to _Th_ (3.0) without any issues. But on most common used distributions like _Ubuntu_
this _procedure_ might be painful. Always something _blows up_ -- old configuration of _X.org server_ stop working,
graphic card driver problem occurs, sound layer on Linux -- specially Pulse Audio
made me sick many times. As a programmer you know how to deal with it, but issue makes regular
user suffer. I've talked about upgrades with few people and they left Linux only because
they didn't know how to resolve some problems. Here, without serious approach to the subject,
specially creators of distribution, will be hard to react against it. _Core packages_, not only
kernel with modules, but also _systemd_ (yet another _monster_ which replace _sysVinit_) should
have upgrade tests. Like a simple thing: we have predefined configurations of version _A_ and we just
check that all of them where upgraded into version _B_ without any problems. A little work with _docker_
and _jenkins_ and we have tests.

And on the end one small idea. Linux might be used more often if installation process would be simple. Yes,
I remember how it was in old days -- command line, sometimes  _chroot_. _Modern_ Linux distributions right
now have good wizard, only few steps, but that's not what I mean. Still you have to _know more_ about
your computer, like graphic and network card and chose proper drivers. So, I would like to have CD, USB (whatever)
which detects all devices and suggest _the best drivers_ free and non free. With that report on internet
page I chose meta packages, see predefined configuration -- paragraph above, like _Gnome_, _Xfce_.
On the end I receive link to small net installation image which have info about all my needs. During
installation of final image in most cases I chose _next_, _next_, to confirm licences to non free drivers, etc.
Finally, I have fully working system _out of the box_ and perfectly fitted rescue image. 

And really the last but not least. Today Bryan Lunduke inspired me to write this post.
I have a lot of fun during watching this:
[!["Linux Sucks... For the Last Time" - 2017](https://img.youtube.com/vi/SMKeWTVYBUo/0.jpg)](https://www.youtube.com/watch?v=SMKeWTVYBUo)

I also have seen few of his previous videos and I think that you will enjoy them too.


