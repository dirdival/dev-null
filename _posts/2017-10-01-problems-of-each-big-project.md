---
title: "Problems of each big project"
---

Each company after many years of development has the same problems. The funniest thing
of that is that not so many people talk or write about it. I will tell you a story.
Take a sit and have a good time during reading.

# Part I -- there is no rules

Yours company has to have at least one customer. Somebody
has to pay for your work. They decide what has to be done. And to be clear -- they
really don't know what they wish to have. Believe me. Customer never ever know what have to be done.
There are only slogans: _We wish to have a portal about puppets! We would like
to have mobile application for searching empty parking spot. Make a bank/shop for us. We will
send man to the Uranus - make a rocket!_ Whatever. It's not important what you
have to make. The most important thing is that the _Customer_ paid for that and you have to make _It_.

So, some day yours company started. Somebody wrote first line of the code, made first commit, etc.
What was the most important in that days for your company (from a developer's perspective)?
Did anybody care about quality of code? Did developers use good practices?
Did they write tests? The answer on all that question is  -- *no*.
When a company starts nobody cares about that. *It's the best time of development ever!*
There is no rules! There is no backward compatibility! There is no bugs, because there is
no product yet! Life is beautiful and you can use any technology you wish.

OK, let's back to the Earth. Yours company signs some _contract_. There is
no any technical information there. Forget it. As we where talking -- there are only slogans. No info about
hardware, no info about software, no info about users, no info about
anything important. _So what is there?_ -- you may ask. There is the most important information for
both sides: _the Company will make -- it -- for the
Customer for XXX (here money), Deadline is YYY (here date)_. That's it.

And here the game begins. Project Manager (if any is present) says -- _make It ASAP_.
So first developers just start making _It_. Whatever it is. How do they make it? They make some _reasonable_
assumptions according hardware and software limitations,  number of users and other things which define _It_.
Do they write that things in some document and share it with the Customer via PM? Don't be silly.

Rapid development has started. The Company wishes to have a product as fast as possible, show something
The Customer. Developers have a lot of things to write -- but they love it -- creating many lines of new code,
without limitations, without rules is what they wish to do all the time. Does anybody plan something? Does
anybody think about architecture or tests? Forget it. There is not time for that _stupid_ things during rapid development.
We are going forward and nobody will stop us!

# Part II -- release 1.0

... and developers made _It_, or I should write: _they made something_. This is _critical_ time for the Company. Most
of small firms (9/10 in Poland) don't _achieve_ this level. First release is crucial and feedback from the
Customer in most cases looks like this: _What is it? I didn't order it. Did I said a rocket? Are you sure?
I said spaceship. No, no, wait. I recalled more things. I said  -- starship -- like Enterprise. Don't look at
me that way. We paid for that. Yours salesman said that you will make it. And don't look in that document (contract).
It's not valid.  Important Is what I'm saying right now ..._

Who is guilty in this situation? Owner who signed a contract, the Company which didn't give any feedback, requirements,
Salesman or PMs? And again, answer on all that questions is -- *no*. Guilty are developers and they have to _fix it_. ASAP.

If the Company is not _under water_ now starts new act of that play. Both sides start thinking about _it_ but
in a different way. The Customer says that everything is wrong, _it_ is not an application, it's bag full of bugs, etc.
The Company says that there were no information about _anything_, so they made some assumptions. In most cases
after _the first disaster_ real cooperation starts. Appears some reasonable requirements about hardware, software appear. Sometimes
info about technology which should be used -- as always it's a surprise for developers, because they used something
different. Because a lot of work has been done to release 1.0, developers start fixing _it_. Forget about Model View Controller,
forget about Hardware Abstraction Layer and other _good practices_. The time has come for more spaghetti code, quick fix
of issues -- hacks, and much more undocumented code. Since that time, part of developers start complaining -- _We came
here to create new software, not to fix bugs_. Words like overtime, overwork start to appear. Some of developers might just go,
but in most cases people wish to see a final product, because they were hard working on _it_.

# Part III - Where are we going?

Let assume that the Company released version which satisfied the Customer. But it's not end of game. Both sides want more.
The Company want make more money, it may sell _it_ to other customers, or start making a new thing.
The Customers want new features or new products and fixes of bugs found in older version.

Situation is similar to described in previous section but we have new factors:
* customisation -- we have more customers and they wish to have different logic, view of _it_. Developers might do it in few ways. The
bad way is just clone whole application made for one client and start changing it for another. Why it is bad solution? It might work
for two customers. But when the Company grows and have for example 10 similar products, then problems are easy to see.
First, products are different -- obvious -- and they are in different states of development. Some features made for one
customers might be sold for other, but code is not merged. The same issues, bugs might be reported by different customers, even
worse, the same bug might be fixed many times by different teams of developers. How to resolve problem with customization? It's quite
easy and well known solution called: continuous integration. It requires design, planning and thinking about code -- all things
which were not done before -- but gains are huge when it works properly.
* backward compatibility and never ending support -- writing new code is fun, but since release the version 1.0 _It_ has to work on the same hardware
in the same way to the end of the world. Even worst -- you have to make backports of fixes when a customer found some issue for prehistoric software.
 It doesn't sound  scary, does it? But imagine that a customer has release `1.0` of _it_ but current the newest one is `6.0`. So many things were
 changed in code, there were made so many refactorings and now -- bang -- fix it. You can't make patch between versions, it doesn't work.
 In most cases you have to find the same issue in old code and fix it.
* third party dependences -- so, you have released _It_ and you bought or used source code from another company. Sounds great, somebody
has same good piece of code and you can use it. But after few years this company or support of this code no longer exists and of course
some customer wish to have some new feature related to that code. Another problems are with old toolchains -- you wish to use some new
features from new version of compiler on other tools but you can't because you have to work with the old toolchain -- because customer,
or other company doesn't agree to use the new one. And you have to work on _C++03_. New developers where come to your company, they are
just after university, they were using _C++11, C++17_ and they complain about it. Sad thing is that there is not easy solution here. For
compiler flags specially new warnings you can check current used version of software and enable them, but after a few years of that practices the makefile looks awful.
* still growing source code -- developers are writers and they love write new code. After few years source code is huge. I have seen so many times ...
Each new developer thinks that he know _better_ how to write data from buffer into file. When I made some small research I have found 7 different
functions which make exactly the same logical action in a different way. Of course, it's interesting when you look on them but each implementation
had different problems, issues. Another problem are _poor_ developers which make  _copy/paste_ code, make huge functions, etc. Some of these  problems
might be fixed by a coding standard, which should describe how function should be named. But, who reads _codding standard_ and does The Company have
any? Code review also helps but each team has different standards and nobody knows all features, functions which were written. In other words, developers
start to write the same code again, again because they didn't find it in huge code.
* new and old developers -- old developers _know_ what they made. Sometimes it's a crappy code, especially in release _1.0_ but if they made
any refactoring, and removed spaghetti code it might have _some_ shape. Of course nothing is great, there are places without tests and documentation
but they know how to deal with them. The newcomers have to learn how to _touch_ that places. Sometimes they make wrappers, additional layers,
because they don't understand how it works and they afraid to change it. From the other side, new developers might easily recognize some wrong  API, strange
constructs, poor code. Allow them ask _stupid questions_ (they are not exist) and listen them carefully because sometimes they are right.

---

It's only brief description how projects work in the real world. I was working for
few, small and big companies and each of them have some described issues. Of course your company might work in different way. Many of described problems are related to
reusing old code -- specially in _C/C++_ languages. If you work for example in _Python_ or _Ruby_ probably you don't care about backward compatibility.
You just make a portal and after sell you don't care. If customers wish have something new you start from scratch and you remove old crap. I would really
write something good at the end about long term projects, but I can't. Working with them is some kind of masochism.
And if you are or try to be programmer you should know about it.

