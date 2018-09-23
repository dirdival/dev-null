---
title: "Spam tools"
---

I recognise them easily. A post comes from a person that has just created a new account.
She or he always set some random information about self and the game has just begun.

Titles are quite repeatable: _Im happy I finally signed up_, _Im glad I now registered_,
_Just wanted to say Hi!_

Body of a post also has a pattern. It starts from small greetings or introduction like:
_I am the new girl here_, _Beneficial info, Thank you_, _This is really interesting, You are a very skilled blogger._

Finally we have a clou: _Stop by my page_, _My website_, _Here is my weblog._

It makes me crazy when on Linux forum I read new posts about poker online or loans.
To be honest, some of them are incidentally fun, like this:
_I love your blog.. very nice colors & theme. Did you design this website yourself or did you hire someone to do it for you?_
This quote comes from forum created in last century and it still has look from that ages.
I can imagine that exists somewhere a geek, which today decided use the oldest and not supported version of `PLD`
Linux distribution, but in this particular case, I expect something more than: _I am the new girl ...  Also visit my website ..._.

---

Who is responsible for a spam? It's obvious -- we see link to a page. This kind of cheap _advertising campaign_ mostly aim at
[crawling bot][link_crawling_bot]. The goal is simple, higher [page rank][link_page_rank]
means higher position on list, when you type something in search engine. Web services try to reduce amount of unwanted messages,
they require email address and send confirmation link to prevent simple adding comments.
Also, quite commonly used is [`CAPTCHA`][link_captcha]. But even though services use this techniques, they still have a problem with
spam.

Since months I observe increase number of unwanted messages. I decided to find out something more about spam.
How do they do it? Is it hard to become a spammer?

---

Because I'm a moderator, I started gathering information about this posts like: IP address, titles, descriptions and referred
web pages. After a few weeks I've recognized some patterns. Spam mostly comes from:
* new accounts,
* windows machines -- according to user agent,
* IP address each time is different that used before, even if it refers to the same page.

For me as a programmer, it's obvious that some tools are used here. As I said, I started also collecting bodies of posts for future _investigation_.
I waited for spammer's mistakes. I caught some, but in most cases they looked like this:

    title: ________
    body: Feel free to visit my weblog - _____ ____________________________

It's a quote, real message, underscores are part of it.

Moreover, I suppose that there is a spammer which signs his _work_ with sentence `[X-N-E-W-L-I-N-S-P-I-N-X]`. On a first glance it looks like a mistake,
but from my point of view it's not. After small research I discovered that it happens to often and since many months.

But one day, a few weeks ago I saw this:

    lytter til venstre midtbane Men den vindende formel for Coli
    %spinfolde-D:\Articles\dk_ubb%

`%spinfolde` part looks suspicious. I used `google` to find out more about it and I [nailed them][link_nailed]. A spammer used
[GSA Search Engine Ranker][link_gsa_ranker]. On their [main page][link_gsa_main] I also found links to products:

* GSA Search Engine Ranker
* GSA Captcha Breaker
* GSA Auto Website Submitter
* GSA URL Redirector PRO
* GSA PR Emulator
* GSA Content Generator
* GSA Image Analyser
* GSA Email Spider
* GSA Proxy Scraper

and much more. You can find here well made [tutorials][link_gsa_tutorials] hosted on `youtube`.
 They show how to use all this tools, I've watched some of them.
What is _GSA Website Contact_ and what is it used for? [Here][link_gsa_website_contact] you can find answer.
Small spoilers:
* how to use private proxies for sending (see -- 3:30),
* add `gov` (government) domains to black list (see -- 7:20),
* `submission content` (see -- 8:35), very important section. You can see here how to make a _campaign_,
 how to set for each field like name, company, etc. random generated value.

I can also recommend you the film -- [How To Get Started With Captcha Breaker][link_yt_captcha_breaker].

---

It wasn't so hard to _discover_ this service. There is no doubt what kind of tools they make and who buy them.
The main question is: why this page is still working? Does anybody care about that?

Recently, according to [`Spamhaus`][link_spamhaus] one of the biggest spammers [Peter Severa][link_spamhaus_Peter_Severa] was caught.
How many mistakes he had to make? How many big campaigns he had to make until somebody started looking for him?
I'm afraid that big internet players doesn't care about spam problem like they should.

---
[link_crawling_bot]:https://support.google.com/webmasters/answer/182072?hl=en "Googlebot"
[link_page_rank]:https://en.wikipedia.org/wiki/PageRank "Wikipedia: PageRank"
[link_captcha]:https://en.wikipedia.org/wiki/CAPTCHA "Wikipedia: CAPTCHA"
[link_nailed]:https://forum.gsa-online.de/discussion/23714/spinfolder-macro-does-not-pick-one-keyword/p1 "GSA forum - thread about spinfolder macro"
[link_gsa_ranker]:https://docu.gsa-online.de/search_engine_ranker "GSA Search Engine Ranker"
[link_gsa_main]:https://docu.gsa-online.de/start "GSA main page"
[link_gsa_tutorials]:https://forum.gsa-online.de/discussion/11/gsa-search-engine-ranker-video-tutorials-updated-2017-03-27/p1 "GSA tutorials"
[link_gsa_website_contact]:https://www.youtube.com/watch?time_continue=345&v=JzwiYuaopqE "youtute: GSA Website Contact - Contact Form Marketing"
[link_yt_captcha_breaker]:https://www.youtube.com/watch?time_continue=530&v=dxpW6vvBNaY "youtube: How To Get Started With Captcha Breaker"
[link_spamhaus]:https://www.spamhaus.org/organization "About Spamhause project"
[link_spamhaus_Peter_Severa]:https://www.spamhaus.org/rokso/spammer/SPM893/peter-severa-peter-levashov "Peter Severa"
