---
title: "Enigmail and gpg groups"
---

When I came to the current work, almost 9 years ago, everything was simpler. We had great admins
with huge knowledge. They were experienced, and they had full control over the internal network.
Now after three takeovers, when the company became a part of corporation, things are different. 

---

Each corporation uses smart, _pro_ solutions, sometimes called outsourcing. Now we have an external
IT department. Your issue goes through many hands, and it takes ages until somebody will start working on it.
More or less I learnt how to deal with it -- stop creating any requests! Your friends from the whole world
have to do something, and they are confused. You, a C/C++ developer, have to pass them
`iptables` rules, because your _Cisco-certificated_ friend missed training about firewalls.

---

In many cases, the first thing which is outsourced is a mail server. A corporation has to have
the same suffix in a mail. After that change, your communication with developers from the same room lefts
the company and goes _somewhere_. Things got worse when a company decided to use [`Advanced Threat Protection (ATP)`][link_atp].
Since now, each mail with an url looks like spam, and it's [not only my opinion][link_rewriting].
_What to do?_ -- you may ask. The answer is simple -- encrypt, encrypt everything!

---

When you use encryption, you can share with other passwords, and other credentials in a secure way over the network.
Moreover, a sent message can't be changed -- read -- your urls look like they should. Thankfully, most of my aware
coworkers know it and use [`Enigmail`][link_enigmail]. You can even only sign a mail if you hadn't exchanged keys with
your recipient. In this case, the text also can't be malformed.

---

There is only one problem -- mail groups. It's a good idea to have only one mail to share with others your ideas/problems.
But with `Enigmail`, you have to pick one by one recipient keys to build the whole list.
To be honest, the plugin has the option
`Edit Per-Recipient Rules`, but it doesn't work correctly, or I have no idea how to convince it to work. I had tried many
things, but I can't edit the filed `set Enigmail Rules for`.

---

In spare time I started digging for solution, and I found it. You need only create a group of recipients but in `gpg2` (`Enigmail`
uses it by default). The most important part of this solution is: *the name of the group has to be the mail group*. If you make
a group that doesn't look like a mail address, then `Enigmail` doesn't accept it.

Here is an example. My mail group is `dirdival@somwhere.over.the.rainbow.com`, and it has two members.
You need only add in the file `~/.gnupg/gpg.conf` this line:

    group dirdival@somwhere.over.the.rainbow.com = dirdival_grab_whole_spam@onet.pl 31670276+dirdival@users.noreply.github.com

You can use fingerprints or mails as recipients. Please remember that you need the public keys of all members of the group.

Test part. Create an encrypted message:

    dirdival@discworld:~$ gpg2 --encrypt --armor -r dirdival@somwhere.over.the.rainbow.com > /tmp/message.txt
    hello
    dirdival@discworld:~$ cat /tmp/message.txt
    -----BEGIN PGP MESSAGE-----
    
    hQIMA7z4d62sernkAQ//UEo3LIAbyxRVz1apiI7tSDVt/cowYESwrkRg5cYfMOZs
    SapdqC29ffZFGrIzB9ZhzcTGVUwhPTAf2CfEnbOpVwbFg5vFs3fjOiJsE/SnG2MA
    j5M+unJrICQuZPYeXZZBlGYDRNW+7noCMMSa9emeMMILlCEZ+QP2D/pvAQupoDjD
    IGOVvN0zWYAnvWSfcyLTdV/PfL0s2sAme2/SyXK9uUbL8Nu6r2iQde50Dyfs/srh
    N4Dtf6T60c6Z4Y4Nov+U/H3OpbXuvCxW3UsKRLwOr7Fl3tDJlTY/SAe8Cu6tWPbt
    Y49ARmSFvtTnnGufv9gndJnUHey65uGm/q33deUXRs/bysnIx0jBQVwFIps8g5yP
    1FJAXd+QNsUkv3ATja/hw4A9GIzOOlCr4wCt0cmGVVaeScH462YHcutm8dmVM4jr
    B7B9wjsGQB3cdvyTCimAYoVb8JOnSDMO6a1EYOOdiijwdn3KF2ynbBjur47+E8Fc
    wDEqSNk7jSmLgV0jyR3bk0i9goGY0xTLy22UuDwfegiBsCmDnDPHxpUimyKOHHiz
    n3+l4ZYG8koqg0MGlqSFnEyzSTSd6xR1yDfhc42p2pbv4vgZJVMG9o362luRgFqV
    LqtoFRjLLYJTJSyOU2wJSNINzBVl/ExeEltG/6MKtyOGOCAxchLjI4Usr+eq6ciF
    AgwDfCs34xjLXL0BEAClP6SPGtuaJVwr5nUSHWrb4YTIGf/mJY/wEM1qgAreBJHx
    gtNxXoZ48MNgSO9nHrkAbzbXzyNiuOBfsTk90oYrqeT4w7FsTpL1GuvQApd1TI0p
    3tq6+Y3gvfd6WRcxfvO62Met1Y7NiE3PeUpRrQQBt/syj9gKaKwYI5W86GTfRWVy
    Gocik/SHto2zuPp5PyC3QirDBSjFsRtCqukOvFKIt5eOalwMGyR6GQnu0ElleunY
    n4kix1mPrEl51lnC+I0xHStcU3vx/NKNSWhBWtRAzomm1Xt1l0hF2DVCl/oyi0hw
    5bGjbNeW6u9Lpx8yHN9HM7WO19XhSAo0Rb6WKga8FRakjZqIsGqOFZjkWyaFZp2b
    wRCxIqBzxtx4cS2pkaeIVlrSWZmev+qHuHZQJfFnIyLenWATeHTFQr28Y2xH+44b
    ddzdIYfRBznU+4+u1dB3l/bH647oXBhKWHmBCHyhDILmIVVs0zGI6pnXsF2j9oj+
    A6HRBXnWXPdRYxK9OZ2WkAhfsoVN7unpIZG490t4mQn1iEf5l96OWLk/6aETtgu9
    dScvJAqsgoRM+XAz1kQz2A1IuXyFyIg0zbNBHn29vo4vVO0nGHOf3AT9vk71+xIj
    AqI3HDzg+99E69/axMRWA+PtZefcftaK5qTsVkvQaX3zTHdqRZcA7lHkdD2MTdJB
    AQnKRneUhWyx6R87ZrwoEn6H8NAN6JOsh8RRbtf1yvzm+fPULnZmXPcHXLYqGK1O
    zri530oIHrk5rGz0k99FBb0=
    =pvw9
    -----END PGP MESSAGE-----

Decryption:

    dirdival@discworld:~$ LC_ALL=C gpg2 --decrypt /tmp/message.txt 
    gpg: encrypted with 4096-bit RSA key, ID 7C2B37E318CB5CBD, created 2017-09-09
          "dirdival (github gpg - don't contact with me via this email - I don't read it) <dirdival_grab_whole_spam@onet.pl>"
    gpg: encrypted with 4096-bit RSA key, ID BCF877ADAC7AB9E4, created 2017-09-09
          "dirdival (github noreplay email) <31670276+dirdival@users.noreply.github.com>"
    hello

`Thunderbird` should mark the _encryption_ button on your mail group if you made everything correctly.

---

The last thing. Almost all of my encrypted mails, which go through `Office 365`, are mangled. Thankfully, `Enigmail` has the button `repair` to prevent a _bad touch_.


[link_enigmail]:https://www.enigmail.net/index.php/en/ "Enigmail - the main page"
[link_atp]:https://docs.microsoft.com/en-us/office365/servicedescriptions/office-365-advanced-threat-protection-service-description "Office 365: Advanced Thread Protection"
[link_rewriting]:https://langbein.org/microsoft-outlook-office365-url-rewriting-in-e-mails/ "article: Office365 URL Rewriting in E-Mails"
