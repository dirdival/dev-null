---
title: "not well known applications"
---

During last week I made a basic set up of [new desktop.]({{ site.baseurl }}{% post_url 2018-02-18-discworld %})
I had a lot of data to review, gathered since many years. Dozens of unneeded, forgotten GBs and a lot of directories
with _interesting things_ to touch in the future. I can't say that I got rid of them all but I was under a delusion
that I'm able to do many things at the same time. The _old school_ of clean up always works -- if you didn't use/take care of
something more than one year you can remove it. As always are some exceptions from the rule -- photographs (memories) and 
_interesting things_ (ambition).

---

With a clean up I also made an inspection of all programs installed on the old computer. Very useful is `history` command which
directly shows you list of the most common typed commands. Obviously it's helpful when you call applications from command line. Also,
useful is list of all installed programs:

    dpkg -l

Many programs work in daemon mode, they are started by scripts and you may just forget about them but you still use them indirectly.
One of the best solutions to find out which programs might are important for you is check the list of modified files.
Thankfully in `debian` repository you can find command `debsums`. The most powerful set of arguments looks like this:

    debsums -ca

I'm not going to describe how it works, please follow `man` pages for more details.

---

After all, I generated a long list of used programs. I'm not going to share with you whole list, there is a lot of common programs, but
I think that it's quite good idea to mention here some not well known applications:

* [`slim`][link_slim] -- displayer manager. It's the smallest and the fastest one which I found long time ago. Moreover, you can run it
without any problems even on `VESA`. Right now modern displayer managers like `GDM` don't support it.
* [`i3`][link_i3] -- if you love `Vim` you will be delighted. The most configurable window manager ever. Nothing more to say -- just start using it.
* [`w3m`][link_w3m] -- there is only one reasonable text-based web browser. It has saved my bacon many times when `X11` blown up. BTW, it supports cookies (_read_ -- sessions).
* [`feh`][link_feh] and [`eog`][link_eog] -- the first one, _feh_, is a image viewer used by me mostly from command line. It works well with `i3` and has a lot
of options. `Eog` is minimalistic image viewer with a good looking interface -- mostly used to show something other people and don't scare them `feh` command line arguments.
* [`scrot`][link_scrot] -- very easy and useful command line screen capturing application. You can take picture of whole screen or select a part. Works perfectly with `i3`.
* [`Evince`][link_evince] -- light `PDF` viewer. Small memory usage, fast, nice look, it has everything what I need.
* [`calibre`][link_calibre] -- an e-book manager. To be honest, I don't like it look, but it has build-it conversion from `epub` into `mobi`. Moreover, `ebook-viewer`, part of `calibre` is very useful.
* [`Liferea`][link_liferea] -- `RSS` reader. Old school, easy to use.
* [`Anki`][link_anki] -- I use `Anki` since a few months day by day. It's much more than flashcards on computer. I'm mostly use it to learn English but I also have some _odd_ decks.
* [`Redshift`][link_redshift] -- Yet another application which has to be installed on my desktop. It protects my sight against screen bright light. Just try it. If you think that it doesn't work simply switch off it at night. Then you see the difference.

---

I also learn that it's good idea to create an user account even for one application. On old desktop I called `steam` from my user account. `Steam` creates a lot of directories in different places on your home directory. After a few years you have mess. Moreover, when I made a backup from `pld` I had to exclude all this directories. Right now, on `discworld` I made an user account `steam` and it resolved my problems. Furthermore, when I enter into that account `steam` starts automatically. Finally, I don't have to be worried that this application contains backdoor. Even if it has _something special_, it can't reach my other data.

[link_slim]:https://en.wikipedia.org/wiki/SLiM "SLiM wikipedia"
[link_i3]:https://i3wm.org/ "i3 main page"
[link_w3m]:w3m.sourceforge.net "w3m main page"
[link_feh]:https://feh.finalrewind.org/ "feh main page"
[link_eog]:https://wiki.gnome.org/Apps/EyeOfGnome "eog main page"
[link_scrot]:https://en.wikipedia.org/wiki/Scrot "scrot wikipedia"
[link_evince]:https://wiki.gnome.org/Apps/Evince "Evince main page"
[link_calibre]:https://calibre-ebook.com/ "calibre main page"
[link_liferea]:https://lzone.de/liferea/ "Liferea main page"
[link_anki]:https://apps.ankiweb.net/ "Anki main page"
[link_redshift]:http://jonls.dk/redshift/ "Redshift main page"
