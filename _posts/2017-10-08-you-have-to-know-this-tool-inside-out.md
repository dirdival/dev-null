---
title: "You have to know this tool inside out"
---

Each developer spends more than a hundred hours per week in front of computer.
I should be more specific -- most of the time we are using one program and
I'm not talking about a web browser.

Non programmers usually are surprised when first time see _our editor_ -- _What it
is? Why each word has different color? Do you need list of all this things?_

For us this _things_ are so obvious. Current _modern_ editors or I should write
_IDEs_ have a lot of new features  -- built-in static analysis, _precompilation_,
_intelligent_ rename, etc. Sometimes I'm really amazed how many new interesting
things IDE can do. But, do you really need all of them?

---

Many years ago I was looking for _the best editor_. I checked many of them, list is not
important and I found it. Each of us has different needs. I prefer quick moving around
the code, quick response -- not waiting for some action when IDE _is thinking_. You
may expect from yours editor some tips, static analysis or have _inteligent_ autocomplete.

After many weeks of search I realized that is not important what I chose. There is no
the best editor and it never appears. There are only yours expectations. Some of them
are crucial like highlighting, other are _nice to have_ and on the end are _gadgets_.
Do you really need instant static analysis of your code during development? How often
do you rename your methods in class or other members? Does quick search of file
is important for you? Does quick switch between source code and header is crucial?
Do you allow your editor take all memory? How many files I usually open?

There are hundreds of questions, but crucial things are quite easy to define. If you know your
expectations than you can chose your _the best editor_.

If you are not fussy then list of yours editors contains at least few items. In case
of any confusion which chose, pick _any_ of them. Yes, any which satisfy your needs.
Now the time has come to the most important step -- learn each option of it, each shortcut,
each available feature. Now it's yours the most important tool at work. Many developers
_foget_ about it. I have seen so many times like someone fights against IDE. He
didn't configure it, search anything takes ages because this so _intelligent_ editor
scanned result of compilation and other data which shouldn't touch. Many developers
open dozens of files and after that have problem to navigate between them. They use
_ordinary_ search when have more advanced tools which allow gain more. You can explore
only methods, variables, members of class, etc. It's much faster and output is much
shorter.

---

For me interesting is: how many new _things_ might enter into crucial list?
Editors evolve. The oldest one which I used was _Norton Commander_. When I look
on available options related to editing files their are really _poor_. But to be honest,
it defined many of my _crucial_ items. Some of them are so evident like syntax highlighting,
open file in specified line. I love brackets matching and shows tabulators. You can work
only on one file, there is no tabs or I didn't found them. I was learning _Pascal_ in that
days and it was enough for me. Computers have small amount of memory, weak CPU so other
fancy things came later. _Specialized_ things like debugger you used directly.
After some time when `DOS` just gone and epoch of `Windows` come, new features appeared:
mentioned tabs, list of functions, list of classes and their members. Files are grouped together
in projects, you have thousands of color schemes and dozens of keymaps to chose. In that days
IDE were powerful but in most cases they were _closed_. New _era_ came when internet blossomed out.
Plugins which you can download from network changed _everything_.
Do you need support for new specific format? No problem. Somebody released interesting tool
for static analysis -- after few weeks you may have it _build-in_. Appears so many useful
things which you didn't think about it. Now I can't imagine developing without:
* enable/disable information about mercurial/git changeset for each line,
* highlighting new not committed code,
* language spell checker mostly used in documentation (nothing new in editors but not all IDEs got it),
* build-in merge tool,
* autoindent of whole block,

but, for example I can live without instant _precompilation_ and  _static analysis_. I use them on
the end of work and mostly as separated tools. Still I assume that program designed only for one purpose
is better than tool which makes _everything_.

---

So, what's next? It's hard to talk about editor without programming languages. Some of them, like _Python_,
_Javascript_ or languages used to make web _things_  might gain many benefits from new IDEs. Right now each change
in code you can see immediately. I can imagine that things like _pip_, might evolve from package manager into
something which has thousands of templates and frameworks where you fill few things and gains fully working product.

What about _old_ languages like _C/C++_, _assembler_? What we can obtain from new IDEs? To be honest I don't see
so much interesting plugins. Obviously there are interesting tools which shows for example how your code changes
when you apply optimization option. Programs related to coding standard (nobody read it and nobody care it) -- like `clang-format`.
At work we tried `include-what-you-use`, it failed on our source code but idea to check, reduce number of includes is good.
I'm looking for a good refactoring tool. Something which help refurbish code from old coding standard into new one in `C++`.
Also I wish to have tool for searching copy/paste code -- but not only in trivial way -- it should find functions working in the
same way, with the same logic.

Many people thing that _everything_ go to internet and cloud. Web browsers editors exist since few years
and some of them are quite popular. Is it _good_ way? For someone might be, but if you think about privacy it's not
the best choice. Also, big companies can't afford to lose control, they have to store their code directly.

---

On the end I have to mention that the best editor ever made is -- of course -- `Vim`. When cooperates with `cscope` they make
powerful IDE.
