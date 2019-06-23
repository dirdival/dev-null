---
title: "fun with POSIX"
---

Write a [shell][link_shell] script seems to be easy and fun when you know the basics. You can check each separate command
in a console, combine results of them. Even, if you are not familiar with scripting, you can create powerful
code. A little more knowledge is required to write portable code. There is plenty of shells on Linux distributions, but only
a few of them are popular: `bash`, `dash`, `ash`, `zsh`. All of them have a common root called [`POSIX standard(s)`][link_posix].
How you can imagine, a common part of many things is small. It means that write portable code is much harder, you have to
deal with limited commands. When you try to use non-POSIX notation bad things may happen. Most common mistakes even have a name --
[`Bashism`][link_bashism].

---

Since many years I deal with `ash` used as a default shell on [`BusyBox`][link_busybox]. `Ash` is close to `POSIX` due to
its complexity. I have learnt many `tricks`, notations valid on each shell. To reduce the number of mistakes I also use shell
linter [`ShellCheck`][link_shellcheck]. But, as always, there are things you don't know, or I should say: _you know nothing_.

---

I'm during porting a company solution from one [`Jenkins`][link_jenkins] server used also as executor to a distributed solution with
many agents. Because the biggest part of old configuration, scripts are still valid I reuse them in the new environment. On `Jenkins`, one
of the most used and more powerful commands is `sh`. It executes -- surprise -- a shell script. When a [`shebang`][link_shebang]
is not present in your script the default shell is used. Which one is it on your computer? You can check it:

    dirdival@discworld:~$ ls -lhat /bin/sh
	lrwxrwxrwx 1 root root 4 sty 24  2017 /bin/sh -> dash

In my case it's `dash`. Let's back to _the story_. I migrated a new pipeline to the new solution and started. It seemed to work,
but a job failed. It looked like a wrong set up. After small investigation I reduced the problem to one line:

    sh ". set_up_environment.sh /a/some/data && call_program"

We have here two common shell _tricks_. Let's start with the second one. When we have in line:

    $ somthing1 && something2

it means that `something2` will be executed *only* when the first command `somthing1` passed without any problems (exit code 0).
The first `trick` refers to the `dot` command. And here is a small summarise how it works:

    dirdival@discworld:~$ LC_ALL=C . --help
    .: . filename [arguments]
        Execute commands from a file in the current shell.
        
        Read and execute commands from FILENAME in the current shell.  The
        entries in $PATH are used to find the directory containing FILENAME.
        If any ARGUMENTS are supplied, they become the positional parameters
        when FILENAME is executed.
        ...

I mostly use the `dot` command to manipulate an environment, especially when a script contains `exports`. It's useful in tests,
you can easily create a bunch of different set-ups. As a bonus, I use arguments to have more flexible scripts.

A simple example:

    dirdival@discworld:/tmp/test$ cat a.sh
    #!/bin/sh
    
    PREFIX=$1
    export DATAPATH=${PREFIX}/usr/local/share/foo

with call:

    dirdival@discworld:/tmp/test$ echo $DATAPATH
    
    dirdival@discworld:/tmp/test$ . ./a.sh /a/some/data
    dirdival@discworld:/tmp/test$ echo $?
    0
    dirdival@discworld:/tmp/test$ echo $DATAPATH
    /a/some/data/usr/local/share/foo

But what surprised me was the result of command on `Jenkins`. It complained about an invalid path.
I checked script with `ShellCheck`

    dirdival@discworld:/tmp/test$ docker run --rm -ti -v $(pwd):/mnt koalaman/shellcheck a.sh

no mistakes. I logged in into `Jenkins` agent machine and checked `sh`. It shows on `dash`.
In desperation I added `set -x` to see each executed line and called `sh` -- just for sure.

    dirdival@discworld:/tmp/test$ sh
    $ set -x
    $ echo $DATAPATH
    + echo
    
    $ . ./a.sh /a/some/data
    + . ./a.sh /a/some/data
    + PREFIX=
    + export DATAPATH=/usr/local/share/foo
    $ echo $?
    + echo 0
    0
    $ echo $DATAPATH
    + echo /usr/local/share/foo
    /usr/local/share/foo
    $ 

Whaaat? Why `PREFIX` is not set? It's the first argument passed to the script.
I called again:

    $ . --help
    + . --help
    sh: 6: .: --help: not found

I was amazed. I took the time until I realised my stupid mistake. I still executed
the script from `bash`. I started working on `dash` after called `sh`.

Part of `dash` man:

    Builtins
      This section lists the builtin commands which are builtin because they need to perform some operation that can't be performed by a separate process.
      ...
    
      . file
             The commands in the specified file are read and executed by the shell.

There is no list of arguments here!

---

Lessons I've learnt:

* always check currently used shell from process ID instead of `SHELL` value

```
dirdival@discworld:~$ echo $SHELL
/bin/bash
dirdival@discworld:~$ cat /proc/$$/cmdline; echo
bash
dirdival@discworld:~$ sh
$ echo $SHELL
/bin/bash
$ cat /proc/$$/cmdline; echo
sh
```

* don't pass arguments to `dot` command
 
[link_shell]:https://en.wikipedia.org/wiki/Shell_(computing) "wikipedia: shell"
[link_posix]:https://en.wikipedia.org/wiki/POSIX "wikipedia: POSIX"
[link_bashism]:https://mywiki.wooledge.org/Bashism
[link_busybox]:https://busybox.net/about.html "BusyBox main page"
[link_shellcheck]:https://github.com/koalaman/shellcheck "github: ShellCheck"
[link_jenkins]:https://jenkins.io "Jenkins main page"
[link_shebang]:https://en.wikipedia.org/wiki/Shebang_(Unix) "wikipedia: shebang"
