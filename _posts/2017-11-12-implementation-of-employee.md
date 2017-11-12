---
title: "Implementation of employee"
---

Some time ago at coffee I had a chit chat about implementation of employee.
Some parts of this process are simple like:
* place to work, computer and other things,
* email account and access to internal systems,
* introduce to important people -- _He is a boss -- stop swearing when he
comes and don't mess with he_,
* and important places -- _Over there is a toilet, we don't make training
for that place_.

This part is easy and any current worker might make it. Now harder part starts
when developer has to find out more about code base and start working on it.
Many things depends on size of the Company, I wrote a little about it in [previous
post]({{ site.baseurl }}{% post_url 2017-10-15-three-sides-of-coin %}). Opposite
ways look like this:
* _here is your desk and start working_, or
* _during whole first week you will be have many meetings and you will find out
many things about our Company_.

Truth is out there. There is no the best way for implementation of employee.
But, to be honest, from my point of view bigger companies make the first part
better then smaller and both of them don't have many ideas how to deal with
second part. So, how may look _the best_ start in your new work?

---

After small trip through office with _guide_ you backs to your desk. You
should meet there your team leader or any other supervisor. He/She should
introduce you to other team members. Your first task is configure your computer
environment -- install and set up your favorite IDE and other stuff. On your
desk should waits for you card with basic information -- how to set up network
connection, where is FAQ page, etc. In case of any questions you should not
hesitate to ask about basic yours team members. The best idea is to _bother_
one person -- it might be team leader or any other appointed developer.
After set up, you should be introduced to your duties by team leader. He/she
should make small speech: _We are making Foo and we are using Boo_. It should
not be too short or too long. You together download the code into your machine,
build it and run a vanilla application. After that you have a brief view of code:
_here is Foo and the newest version of Boo you can find here. If you change
something remember to do that things._ In an issue tracking system a _starter_
task is waiting for you. In simple words is described what you have to do, how,
and what to do after.

---

It sounds very simple but to be honest I have never had yet that introduction.
As a team leader I made as close as possible introduction to described above,
for my and not my team members but believe me it's hard.
So, what might go wrong with this _perfect_ scenario?

---

Let's start from team leader. Most of them is or was a developer. It means that
they are good in writing code but they might have some problems with _personal
contacts_. Of course, people are different and they learn new things when
work together but it may happen. If you see that your interlocutor has _problem_
don't push him, find another team member. It will be better for both of you.

It might sound strange but sometimes your new company is _not ready_ for your
arrival. I'm not talking about things like: nobody know that it's your first day
or you don't have desk with stuff -- what a bummer. I mean that sometimes all
internal _systems_ don't know you yet. Common problem is an access to email,
issue tracker, code repository, code reviewer, etc. Many years ago I wondered
why it happens but right now I know the answer. Your company is big, has
many administrators, they are busy, they may work in different parts of a globe
and there we go.
Second possible answer, not so funny -- they have mess and it takes a time until
you become part of a company.

The time has come for key part -- source code and application. Pulls of it and
starts is not enough. Somebody has to say _something more_ about it and it's a _challenge_.
Everything depends of size of the code and quality of it. Is hard to describe
millions of lines, because any developer doesn't read all things. You can focus
on main modules, classes but in most cases it's too big view for a newcomer. 
Don't make a lecture -- it will be forgotten after few hours. Huge amount of information
is stressful for everybody. Small chunks of information strictly related to the
task is the best idea.
When I have to show _something_ a new developer I always start from simple things like:
_Hey just take a look. In this way we are add logs and here you can find our
STL. Please, take some time to be familiar with this functions._ As a first
task I always gives something simple which allows use this new basic knowledge.

Certainly I described perfect situation. Not always you or other person have
a time to sit with newbie whole day and talk about work. But, the first days
are crucial and you have to pass as many as possible information to your new
team member. During first task he/she has to go through full process:
* new issue,
* discuss solution with team leader,
* coding,
* tests,
* review -- small loop here -- we back to _coding_ item or even _discuss_,
* push a resolution after completed review,
* mark task as resolved,
* wait for confirmation from testers team -- close task.

And again, it sounds very simple but is not easy to obtain it, when all things
don't work together. I completely omitted here _human factor_ and bad or not
existing process of development. After first task you give another which touches
different part of code and you use the same pattern. Many iteration latter you
should have developer which know everything about code.
