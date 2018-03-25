---
title: "Encrypted DNS traffic"
---

I still have a fun with configuration of
[discworld]({{ site.baseurl }}{% post_url 2018-02-18-discworld %}).
Since last days I'm working on set up of `DNS` server.
I suppose that most of you know what `DNS` is and how it works, but if you don't,
small explanation in one sentence: it _takes human readable domain name_ like
`github.com` and translates it into IP address like this one `192.30.253.113`.
You use it all the time, when you connect to a new page first
your browser asks about an IP address _some DNS server_.

`DNS` is very old protocol and like each old network solution has security
problems. The most ridiculous thing is that a `DNS` package may be easy
malformed. It's one of the easiest method of _Man in the middle attack_ -- called
`DNS spoofing`.  You ask for an IP address of some page and instead of correct
information somebody sends to you different IP. When you enter your user and
password on replaced page then your money will gone from real account.

---

Because it's been a while when I last time set up a `DNS` server, I made a small
research to find out how to deal with mentioned problem.

First, I found [`DNSSec`][link_DNSsec]. I looks reasonable, the shortest 
description of this protocol is: _DNS with signatures_. I said -- _Wow_ -- it's
all what I need. But, hold on a second, this protocol is known since a few years 
and I didn't hear about it. It sounds suspicious. I started digging deeper and
I found [_an answer_][link_DNSsecAnser]:

    But, since most of the servers on the internet are not using DNSSec yet
	you will effectively cut yourself this way from most of the internet. 

What? So, in other words, we have the protocol which prevents `DNS spoofing`
and administrators don't use it. [Shame on you!][link_DNSsec_shame]

---

Second solution [`DNSCrypt`][link_DNSCrypt] was mentioned on many pages as an
alternative for `DNSSec`. And again, the idea is the same -- signatures.
I was keen on using it. I read this [tutorial][link_DNSCrypt_tutorial] describing
how to encrypt `DNS` and after that I realised that I can do it better. I don't
like the idea set up _yet another proxy_, because I have one and it also has to
deal with domain names. I use it from time to time and this time I suppose that
you heard about it -- [_Tor_][link_tor]. Most of people, when hear this name
think -- crime, _Silk Road_, hackers. The worst things. But, it's only a tool,
like a wrench -- you can do it with it good and bad things. You decide about
an usage.

---

Configuration of `bind9` with `Tor` as a `DNS` resolver is very simple.
In `/etc/tor/torrc` you need only add option `DNSPort` like this one:

    # dirdival part
	...
    # DNS
    DNSPort 9053

Also, in `/etc/bind/named.conf.options` you have to set _your new_ `DNS` server:

    forwarders {
	    127.0.0.1 port 9053;
	};

Moreover, in `/etc/dhcp/dhclient.conf` it's good idea to set:

    prepend domain-name-servers 127.0.0.1;

Without line above file `/etc/resolv.conf` might be changed by `resolvconf`.

After that changes, all user and all programs from my new machine use `Tor` to
resolve `DNS` names.

---

How it works? We can ask for IP of `github.com`

    dirdival@discworld:~$ nslookup github.com
    Server:		127.0.0.1
    Address:	127.0.0.1#53
    
    Non-authoritative answer:
    Name:	github.com
    Address: 192.30.253.112

Under the hood `bind9` sends query to our local `Tor DNS server` (port 9053).
`Tor` makes _magic things_ -- if you are curious read about `tor-resolve` and
[this section][link_tor_dns] from project page.

![github.com query]({{ "/assets/images/2018-03-25_wireshark_github.png" | absolute_url }})

As a bonus, we don't show our `DNS` queries others.

---

I hope you enjoy this post. The funniest things is that I even didn't mention
why I use my local `DNS` server. The purpose of this is ... maybe next time.

---
[link_DNSsec]:https://security.stackexchange.com/questions/11566/how-does-dnssec-work-are-there-known-limitations-or-issues "stackexchange: How does DNSSec work? Are there known limitations or issues?"
[link_DNSsecAnser]:https://security.stackexchange.com/questions/106688/can-you-force-your-pc-or-device-to-use-only-dnssec-verified-lookup-results "stackexchange: Can you force your PC or device to use only DNSSec-verified lookup results?"
[link_DNSsec_shame]:https://dnssec-name-and-shame.com
[link_DNSCrypt]:https://en.wikipedia.org/wiki/DNSCrypt "wikipedia: DNSCrypt"
[link_DNSCrypt_tutorial]:http://www.linuxandubuntu.com/home/how-to-encrypt-dns-traffic-in-linux-using-dnscrypt
[link_tor]:https://www.torproject.org "Tor project - main page"
[link_tor_dns]:https://www.torproject.org/docs/faq.html.en#WarningsAboutSOCKSandDNSInformationLeaks "Tor project: I keep seeing these warnings about SOCKS and DNS information leaks. Should I worry?"
