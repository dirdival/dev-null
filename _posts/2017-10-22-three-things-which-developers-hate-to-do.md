---
title: "Three things which developers hate to do"
---

There are things which you should do, not because you like them but
because they might save your time in the future. We all know that
they are important, we all know that they are part of our work, but
we hate to them.

# Documentation

Quotes from real life: _We (I) don't need documentation. Our (my)
code is self documented_. If you have heard something like that
you have to know that something wrong is with a code or with a developer.

---

All of us use documentation. Would you be able use _STL_ without documentation,
without any example of usage? Are you are able write valid code only when
you look at header with undocumented declarations? Are you? Really? Let's try:

```cpp
int select(int nfds, fd_set *readfds, fd_set *writefds,
           fd_set *exceptfds, struct timeval *timeout);
```

No, you can't. Some developers _cheat_ and look into definition but it doesn't
help at all if it's not your code. Just try with any header from _C++_ which
uses templates.

You need some examples, some clues how to deal with function, module, library. So,
why don't you write documentation for your code? The most common answers look like this:
* I don't have time -- _Why you don't have time?_ -- because you are writing
new code which uses not documented functions and you don't know how it really works,
* nobody read it -- _wrong answer_ -- you are always looking for a good documentation
with _pretty_ code snippets to make better code,
* writing of documentation is boring -- _to be honest, I also know much more fun things than
that_ - yes, it mights be insipid work, but it doesn't have to. When you write a good
documentation it's similar to talk to _rubber duck_. You may find some problem with
a code or improve algorithm. Just food of thought.

---

From my point of view real causes of not writing documentation are:
* no one requires that -- developer always has excuses, list of them you can find above.
If one team of developers write documentation and other don't, it's really hard to
convince all of them to start doing it. It _has to_ be part of development process --
like _executive order_ -- you can't pass a review without it,
* rapid development -- you write code quicker than you think -- API changes all the time,
even several times during a week. I agree, write a documentation during that process is useless,
because it might be valid only few hours. So what you can do? Add it on the end -- before code
review. And again, crucial thing, each reviewer has to _complain_ when it's not present,
* PM laments that resolving an issue takes too much time -- she/he gave a customer
information that problem will be resolved until some deadline. And we are not talking about
consulted with developer possible date, so it comes from reading the tea leaves. Customer pushes
PM to have fix and PM pushes you. Yes, I know it's easy to say that: _don't care about it and
do what you have to do_. So, what to do?  If you really can't take more time and others developers
agree for _short path_ you have to add a new task into bug tracking system and describe what is not done
yet. When a new sprint starts it should has the highest priority.

# Tests 

Let's start from quotes: _My code is working. It doesn't require tests. I know what I'm doing and
write tests only slow me_. If you don't want to do something you will always find some explanation.

The main problem with tests is that developers which don't write them don't understand why they are
important. Their point of view look like this: _I made my job and somebody pushes me to make tests.
I'm not a tester_. Also, _as alywas_ quality part of development is not important for PM's. If you ask
any person in your company: _Is is quality of product is important?_. Most of answers are: _yes_.
But it's a lie. Salesmans, PM's don't care about a code. It's your part. They are
only care about sale a product and _happines_ of a customer. If you said -- _I made my task and right
now I'm during writting tests_ --  they stop listening you. They think that all things were done. So,
never ever say that something was done if you are in the middle of your work.

_Why tests are important?_ you may ask. They check assumptions made by other developers according to a code.
I other words, they check that _everything_ is working like it was. Tests show current quality of a product
and might dramatically increase speed of fixing new issues.

---

The main problem of developer which hates to write tests is that she/he doesn't think about them. What do I mean?
If you write a code and you assume that it will be tested, you think about good API, components, small
pieces of code to check it in easy way. But if you don't care then write tests is difficult. Spaghetti
code and not well defined dependencies, strictly connected components makes writing tests harder.

The time has come for the hardest part. Tests have to be started each day, automatically, and each developer
should care about his part of code. You need some environment which execute tests for you. I recommend _Jenkins_,
is powerful, easy to use and you can _bother_ poor developers day by day sending emails to them. And remember
to add PM in `CC` -- it resolves many problems and speed up fixing of issues.

And the last remark in this part. Watch out on cheaters! Some bad _guy_ mights crack a system and disable or remove
tests. If you found that _genius_ you should know what to do with him.


# Bug fixing

This is not _fun_ part of development. I know about it much more that you may think. Developers are writers and
they _love_ write a new code. It makes us happy. If you made something really good you are proud of it and
you don't afraid about your future. But you are working with other people and they might have a little bit
different approach to development -- let's call them -- botchers. They don't do that things described
above and they don't care about them. Result of their work is well know as _f*k up_ -- sorry for words, but they
deserved for that. They made something, troubles appear and they just _gone_ to a new work.

First rule in this case -- if developer is still working, she/he have to fix an issue.
It mights be _painful_ for developer but he may learn something about quality of code and good practises.
If an issue happens in my code I really enjoy to find out what it is, because I can learn something new: _Did I make 
some wrong assumptions? Do I really understand a flow of data in an application?_ Fixing yours bugs make you
better developer. Fixing _hard_ bugs made by other make you better too.

---

Why developers hate to bug fixing? In most cases you didn't make this issue and  you have to clean up mess after someone
else. You don't know a code, assumptions made by author. You read some _strange_ code and you try figure out what
is wrong. Even you fix an issue it might occurs that in other place something is not working after changes. Cascade
of hidden bugs pop up and you have to resolve all of them.

Second reason is that, as I mentioned, developers are writers. When you fix a bug in most cases fix contains one or
few lines. You were working on it one week, you are _exhausted_ and you might feel that you didn't make something important.
All people around you are writing new things, and you have to _touch it_ which other expect that it works.
If yours team or a developer made a fix of not his issue you should give him some _extra interesing task_ which he
might enjoy. He deserved for that. Assigning only bugs to one person is a rule for a catastrophe. This
developer leaves your company very quickly.

---

Of course, there are more _things_ which developers hate to do, but top of that list always contains things described above.
Each development team has to know how to deal with them and without strict process which prevents against creating code without documentation,
without tests you can forget about quality of your product.
