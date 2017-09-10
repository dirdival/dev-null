---
title: "TODO comments"
---

When you write bigger feature it takes days sometimes weeks. Plenty of new
 files, hundreds or thousands new line of code. From obvious reason you are
focused on current module, class, method. Very often you have to also make
_mockup code_ -- _fully working_, but without error checking, without check all
necessary conditions, etc. Most of developers make it "automatically"
-- _without thinking_ -- just yet another piece of code used _temporary_
to check current work. _I'll fix that later_ -- you can think and you
backs to main task. 

Now, if you acted foolishly, you might regret that. What I am talking about?
Well, code _is just a code_ and you are not able distinguish between mockup and
ordinary code. Is even harder to catch this difference when you are near the
end of a feature. Is easy to omit missed part if method takes almost whole
screen and _looks good_. So, _what should I do?_ -- you might ask. The answer
is simple, so obvious, even typical -- leave a message for yourself
--  and just use a comment.
```cpp
//TODO dirdival: make error checking
//FIXME dirdival: is not working yet for negative values
//TODO dirdival: add missed conditions for all enum values
```
It might be short, it can be long (with full story and links), but the most
important -- it should say exactly what have to be done. Clear message,
_readable_ by other developers.

Most of editors, IDEs, support this good practice and they highlight `//TODO`
, `//FIXME` comments. That technique:
* prevents bugs -- and save your time
* allows quickly find places to improve, even if they are in many files
* checks _how far are you from release a feature?_

---

OK, how does it work in real life?  Let's take a look closer on a medium size
project (`~30` developers):

```bash
$ find ./ \( -name '*.c' -o -name '*.cpp' \) | wc -l
1897
$ find ./ \( -name '*.c' -o -name '*.cpp' \) -exec cat {} \; | wc -l
651965
```
So, we have `1897` files which weight `651965` lines. BTW, it gives us `~343`
lines per one file, quite nice result. To be honest I expected something near
`1000` lines. We have _C/C++_ here and nowadays many youth developers call
them  _low level languages_.

How much _interesting_ comments we might have here? I check most common tags:
`TODO`, `FIXME`, `HACK`, `BUG`.  I assume that they are not used as name of
functions, variables, etc.
```bash
$ find ./ \( -name '*.c' -o -name '*.cpp' \) -exec grep -i '\<todo\>' {} \; | wc -l
359
$ find ./ \( -name '*.c' -o -name '*.cpp' \) -exec grep -i '\<fixme\>' {} \; | wc -l
204
$ find ./ \( -name '*.c' -o -name '*.cpp' \) -exec grep -i '\<hack\>' {} \; | wc -l
70
$ find ./ \( -name '*.c' -o -name '*.cpp' \) -exec grep -i '\<bug\>' {} \; | wc -l
9
```
But wait a second. You said that we are using this tags in development, so
what are they doing here? It's production code.

Welcome in real world! It has _limitations_. Everything have limitations: Earth
surface area, your life, even a feature which you are creating. In some
beautiful day your Project Manager might say: _Our customer said that we need
that feature NOW_. So here is my _executive order_: _Push it NOW, just like it is._

OK, let's take a look on some comments. I changed them (you know -- license),
but sense is preserved:

```cpp
//FIXME: it doesn't work
```
> Is not obvious, what doesn't work? Function, your whole code from commit,
> your brain? Please be more specific.

```cpp
//TODO: fix me
```
> Similar to above.

```cpp
//BUG: random crashes here - need deeper investigation
```
> Not so bad info, but you should never see it in production code.

```cpp
//HACK: I have no idea how to do it correctly but we have to go on production
```
> No comment.

This list requires some comment. Of course we know about _deadlines_ but the
last three examples should never appear in main branch. Somebody didn't say:
_Sorry, but we are in middle of nowhere and we can't PUSH_.
This comments make me sick when I see them. But believe me -- _I wish_ see
them, because opposite version, without them, is even worst.

So yes, leave a comment for other developers about your problems, ideas,
thoughts about a code, but make it _readable_. Just after an _executive order_
make same notes, in your bug tracking system, or force adding them by
your PM. You should _lave a message_ that not everything were done like it
should.

When I said _go on production_ it means -- go on review, after that tests and
push to main branch -- shorter path does not exist.

---
And small reflection on the end. How you can see tags `TODO`, `FIXME` are
so popular. If your source code is big and you wish use them you can meet 
small problem. During searching `TODO` in editor you might visit not only
yours comments. Good idea is to have some pattern which is unique. I use my
nick name:
```cpp
//TODO dirdival: add error checking
```
Easy to find and you always know _who is guilty_ when you see it.
