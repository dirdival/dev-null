---
title: "pass and pwgen"
---

We use many services on the internet. Access
to some of them is very important for us, like for a bank account, email,
e-commerce platform. Other services might not be super critical, but all
of them require authentication mechanism. A password is the most commonly used
method and it doesn't look like that in near future it will change.

I suppose that you are familiar with basic good practices according to create
passwords:

* they have to be long,
* they should contain letters (small and capital), digits and special characters,
* they should be often changed,
* they should be _complicated_ (non-dictionary words).

But in the real world, if we are going to follow all these rules, there is a small
problem -- our memory. I assume that you are not a [savant][link_savant] and
when a number of services is bigger than 20, you might have a problem
to remember all unique password.

The worst thing, which you may do in this case, is using the same secret
for each internet page. You know what will happen with your money, private
data if you have one password. No matter how it's complicated you are in
the trouble when it leaks. From time to time, even the biggest services
have a problem with security and bad people have access to all user data.

How to help our memory? We know the solution from many centuries -- write
password down. You can do it in an old fashion way, on a piece of paper or
in a digital form. In this case, it's your new treasure, you have to keep
it in secret.

Modern solution, in digital form, is called [password manager][link_password_manager].
You should consider using it if you don't have any. Every good manager has
at least this features:

* data are stored in encrypted form,
* to have access to any password, you have to pass master password first,
* copy a password to clipboard without show it,
* a password in clipboard is available only certain, short period of time.

Moreover, we can divide password managers on local and online. I strongly
recommend you use only the first group. Why? [Here][link_clue] is a small clue.

---

I use [_pass_][link_pass], it's _Unix/Linux_ command line password manager.
It has all mentioned above features and even more. 
Keep in mind that for proper working it requires _gpg_, but I assume that
you are familiar with it.  I'm not going to describe
how to use _pass_, all information with examples you will find on [it's main page][link_pass].
But wait, there are a few things to discuss. What about hard passwords? How
to create them? The _pass_ is associated with _pwgen_ -- the program used to
generate password.

A simple example of strong, random generated passwords by _pwgen_:

    dirdival@discworld:~$ pwgen -cs -1 30 5
	EmApoG7x4F3RMawe2wQWWt2KxA62UJ
	1FT2OaFWgZALWGHvZ6zVAlmYxHiSLJ
	2EPIjhCVRpmZSLlfUSjrOB6T12ondl
	rBwXP4P3ttm6zTd6TwiQ577wGm0rss
	Mu6jpSFhtJZObd7CkVoeejFkpZIJ2g

There is one problem worth mentioning. When you use local password manager
you don't have access to it from other machines. Obvious fact. Sometimes
you have to have access to a service from non your dedicated machine.
How to deal with it? Should you consider usage of online password manager?
Definitely not. Instead of randomly generated passwords you can consider
usage of password generation with seed. What do I mean? In _pwgen_ help we can
find the option:

    -H or --sha1=path/to/file[#seed]
          Use sha1 hash of given file as a (not so) random generator

Here is the idea. You create your own _algorithm_ to generate a password
for a given domain, email. An example for domain _something.com_ and seed: _dirdival_.

    dirdival@discworld:~$ echo -n "something.com" > /tmp/something.com
	dirdival@discworld:~$ pwgen -1 --sha1=/tmp/something.com#dirdival -sc 30 5
	DaAGGQLx80HOF6iGIqrhHsSU3Es16Z
	v8QCJvRFhxSTMoFYHoiJUQdrJWJ9AL
	zAYvH0JG2uSij9QkOFGvv0vMlkegUq
	ApLlVcl7nOw7z7TdNugtEFRnHh3A3T
	W0D0dbgpnaFHCkwu767cm3nfzC085c

When you call above command many times, it will give results always in the
same order. You can easily use it on other machines. It's just an example,
you should consider adding a few steps to _algorithm_, like for example:

    dirdival@discworld:~$ echo -n "something.com" | rev | md5sum > /tmp/something.com
	dirdival@discworld:~$ cat /tmp/something.com
	0be50ab78fe6f42921c052af28ebc9e0  -

But even if it's not complicated, it seems to be impossible to find out
that password _DaAGGQLx80HOF6iGIqrhHsSU3Es16Z_ was generated with a domain pattern.

---

And the last thing. For the most important services, which you have only a few,
you should store your passwords only in the most secure place -- in your memory.

[link_savant]:https://en.wikipedia.org/wiki/Savant_syndrome "Wikipedia: savant syndrome"
[link_password_manager]:https://en.wikipedia.org/wiki/Password_manager "Wikipedia: password manager"
[link_clue]:https://www.zdnet.com/article/data-of-2-4-million-blur-password-manager-users-left-exposed-online/ "Data of 2.4 million Blur password manager users left exposed online"
[link_pass]:https://www.passwordstore.org/ "pass - the standard unix password manager"
