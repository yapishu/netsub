---
title: "A conversation about Tacen with ~mister-todteg"
date: 2021-01-24T11:35:41-06:00
draft: false
toc: false
images:
tags: 
  - tacen
  - urbit
  - mister-todteg
  - cryptocurrency
  - noncustodial
  - exchange
---

![](/img/tacen-header.png)

A sentiment you'll frequently come across is that the community on Urbit is very high-quality, and it's a great place to meet friendly and interesting people. `~mister-todteg` is an example of both -- his company [Tacen](https://tacen.com/) is developing a semi-decentralized noncustodial crypto exchange platform, and he was kind enough to spend a couple of days telling me about it over chat. In this interview we chat about him, the details of his company, his personal history with Urbit, and his vision for the possibility of future integration between the two. 

You can find ~mister around the network, or in one of his public groups: `~mister-todteg/tacen`, for info and discussion about his company, or `~mister-todteg/melody-mars`, a group for sharing pre-1990 pop music from Europe and Asia. 

---

![](/sigils/sitful-chat.svg#floatsigil): Thank you for doing this with me; so, you're ~mister-todteg, and we're here mostly to talk about your startup, a decentralized cryptocurrency exchange. I'm sure a lot of people reading this have seen you around the network -- how do you introduce yourself at parties, what was your story before your current venture, and what's your story with urbit?

>![](/sigils/mister-todteg.svg#floatsigil): Thank you for having me! Well, I don't get invited to parties, but if I do crash one, I tell them I worked in defense sector for 9.5 years. Usually people have either curious face or they cringe. I always get a kick out of that.

> Went to Georgia Tech and studied Aerospace Engineering. It was pre-9/11 so class was small - around 100 or so that year. I played a lot of MMO but worked my ass off to finish school. Didn't really know what to do so got into PhD program, only to find out that it would take 5 years. So I got my masters degree, thanked the adviser, and left. Wrote software - like pilot in the loop simulation, and later real time physics sim of rotor blades and calculation of aeroelastic effects on fuselage and such. I once sat through 18 hour meeting with DARPA folks. Quite an underwhelming experience.

## 'I think that there will be other broadly accepted models that will gravitate towards owning your data, auto-audit any application you run.'

> After that, I joined an ecommerce startup, writing webapps and doing data mining stuff. Which was a lot of fun. Did that work in a startup incubator. Startup was funded by Marissa Mayer and ex-Googlers, so I got invited to Mayer's Halloween Party. 

> After the ecommerce startup folded I went to SmartThings, wrote a lot of software for them, travelled to India and Korea for work. Managed teams overseas. Can't go into too much recent history stuff unfortunately, but Samsung's Suwon HQ is quite something. Then when I just about had enough with Samsung, I left and traded crypto full time for a while; this was around 2017-2018 period.

> I did OK, not fabulously well, just OK. Trading semi-professionally is very lonely. At some point I figured I should get back to writing software, so joined a crypto-fiat wallet app company Metal Pay, first as Blockchain Engineer, then later built a lot of traditional backend infra as Director of Engineering there. Working with stuff like... flow of funds, AML reporting and anti-fraud systems, plain old REST API services, archival data backend, user-retention and promo system, compliance backend, market integration... I think I wrote about 1/3 of the deployed microservices there.

> I was commuting to San Francisco every day, sometimes via BART, sometimes via Caltrain, and other times I just drove, so I stayed at work late too. At one point I took customer support roles during holiday season (promo and such require a lot of handholding). So I was personally getting burned out so I left and took about a month off, just trading full time again. That was around June 2019. Did OK, nothing to write home about. I was staying up so late that days merged into nights. Blinders in my room never changed its positions.

> I told myself I should do something a bit more productive so I started to write everything I hated about user experience in various crypto exchanges; one of them was KYC, and second was outage of websites everytime there was a spike in the interest. And then during a flight to somewhere I wrote out how I would design the system, centered around privacy, and then it dawned on me that I read a May 2019 guidence from FinCEN that basically said that properly decentralized DEXes are probably exempt from AML recordkeeping requirements. So I quickly wrote down how something like that could work and be compliant with US laws, incorporated, and started writing software. That was around July.

> As you know I said 2019, so it's been year and a half since I started the company, so I had to sell off a lot of assets - lawyers cost a lot! Definitely had ups and downs during last year and a half, but we're at a final stages here for getting something out the door fast.

> Ok, urbit story: First time I heard about Urbit was by chance I saw Clark Hat's [review](https://www.popehat.com/2013/12/06/nock-hoon-etc-for-non-vulcans-why-urbit-matters/) of urbit, then saw Andrea O'Sullivan's [article](https://reason.com/2016/06/21/can-urbit-transform-the-internet/). Around that time I saw that horse-computer dude (Gulliver's travel horse people name) Hhhhhmnnnn or something, I saw his article [Martian vs Human](https://ngnghm.github.io/blog/2016/06/11/chapter-10-houyhnhnms-vs-martians/) computing or something. I might not be recalling this correctly.

> I was really intrigued, I mean at that point I had written software for 15 years or so, so I had my share of horror stories about consistency, lack of idempotency, and indeterminant nature of computing. Wrote enough CRUD apps to fill my grave with the printouts of the codes. So when I read about what urbit was trying to do I was really excited -- here was a new computing platform that was trying to make my life easier. This was something that tried to provide primitives for writing distributed applications. I am not a comp sci grad, so I made all kinds of age-appropriate mistakes, so by when I first heard about urbit I was hooked.

> Then I heard Curtis was cancelled at Strangeloop. I kinda knew his controversial positions lol but honestly I just wanted to hear him speak about new OS. So when I heard he was going to LambdaConf, I booked a flight and saw him. I think I was already running a planet I got at that point, so it was quite a good feeling.

>  have a photo I took - one sec:

[![](/img/cgy.jpg)](/img/cgy.jpg)
* *c/o ~mister-todteg*

> I kept on using it, and I gotta say it recaptured level of excitement I had when I used to use BBSes on 2400 baud modem. Ok, this is becoming a rant -- quite a big problem of mine. In any case, I went to almost all early urbit meetups in SF, just kinda hung out there, asked obnoxious questions like... can you stream video via urbit? How would you make a FPGA implementation of urbit kernel? Since there is no HAL, how would you do sound? Could you do graphics acceleration maybe by writing a jet? But then it wouldn't be a jet, since you need corroboration with hoon. That sort of questions.

> I mean even despite it, (and I was really just asking beyond what urbit's core mission is at that point) there was one thing that I focused on as value prop: Deterministic Computing. 

> So I'm sure your readership is knowledgeable enough to know that JAVA's original big selling point was write once run it everywhere. I think the simplest way to describe deterministic computing would be... write once run it forever. I hate maintaining software, and I'm sure most do too. I mean it's unavoidable, but many of the maintainability issues, beyond just updating libraries or compilers or what have you, is userspace app issue, it's just no longer compatible with underlying system. 

> I might be talking out of my butt here, but I ran into, when I was writing embedded software for busybox stuff (poorly), I had several failure to replicate issues. I don't remember exact problem, but I remember it being due to what compiler flag I used.

> Anyways, feel free to jump in

## 'Exchange operators can block deposit, withdraw, freeze, or even run away with your money.'

![](/sigils/sitful-chat.svg#floatsigil): No, this is great, this is about you. I think a lot of us have a story about how we got really excited and sucked in after we figure out what urbit is about, I like hearing other peoples'.


>![](/sigils/mister-todteg.svg#floatsigil): haha thanks for indulging me. I feel that having people read about this is very flattering.

> So my work experience, which isn't that exciting tbh, informed me that urbit was onto something. I have been engaging with urbit community for a while, and I talked about iot application, need for simple apps so that people can de-google their lives, and so on. Where my own startup and urbit starts to intermingle is when I started to hire engineers.

> So the team at my startup... we have 15 people now, and now 8 out of them (I think) are urbit users. So other than Tlon, we're probably the first company to have majority urbit users as team. So you might ask... is Tacen integrating urbit into its system...

> Ok, I should start with what Tacen does first:

![](/sigils/sitful-chat.svg#floatsigil): Yeah, can you give an overview of what Tacen is, how it works, and what it aims to be?

>![](/sigils/mister-todteg.svg#floatsigil): So when you're working with a centralized exchange, you have two problems - custody and permission.

> Custody - someone else has your money, and 

> Permission - you need their permission to withdraw

> This poses a big issue - I think this is exactly how one would classify agent-principal problem. Exchange operators can block deposit, withdraw, freeze, or even run away with your money. So I think since 2016, crypto industry as a whole had $2 bil stolen from exchange hacks, and exit scams aren't even being taken into account. So it's a big problem.

> Second problem with centralized exchange is KYC. They have your PII - personally identifiable information. I had to set up KYC/AML system in my previous work, so I can tell you some of the preparatory info I have about PII blackmarket. If I remember the study correctly, you can buy someone's identity with 6 pieces of PII for roughly $20-$30, usually enough to do total account takeover in less secure service. You can get 9 piece PII for roughly $55, which includes things like SSN, last known address, passport number, and with that you could probably do account takeover of bank acct with some work. So PII out there is cheap, and fintech startups with all that PII data archived, when they go out of business... well, let's just say that some devops engineer might just take a archival copy and sell it somewhere when the startup goes out of business. It's highly criminal and unethical. 

> So you have customer data, which is now your (business) responsibility to keep it safe, but no one really knows what the lifecycle of these things are. Separate problem, going back to principal-agent issue, is that exchange operators can't be trusted. When they have high volume of money coming into the wallets they control, they can easily be tempted to run away with clients' money.

> So they have your data and your money. You are completely at their mercy.

![](/sigils/sitful-chat.svg#floatsigil): So Tacen is not an exchange in the traditional sense, and doesn't require a central service or trusting a third party for transactions -- what technology is it built on?

>![](/sigils/mister-todteg.svg#floatsigil): So tacen is a hybrid exchange, we have order matching done at centralized server, but the settlement is done peer-to-peer. Reason why I opted for that design is because DEX, while great, are inevitably slow. Blockchains are great at solving concurrency problem, at least practically solving it. They resolve disparate transactions, and order them in serial fashion, basically taking advantage of statistics to line the transactions to prevent double spending; but it isn't good at speed, so we centralize that part.

## '...there is no effective control from us. We can't withdraw, command to send fund, or do anything with trader's own escrow.'

> Backend is traditional - backend services are written in Nim, a nice programming language with great meta-programming support; settlement layer requires smart contracts, for now we are developing for chains supporting solidity; front end is a traditional webapp, with heavy web3 library integration - a dApp.

> Might be wondering - well, after all that talk about centralization being bad, why do that? So two fold - one is trader expectation, and second is time value of money.

> Traders' expectation is one of quick order entry and modification, roughly 90% of all orders are cancelled - perhaps they are automated mostly. When order modification takes longer time seconds and even minutes, the spread (price difference between highest bid and lowest ask) is going to be higher than system that can respond faster -- reason for it is that there is risk associated with time. I think best DEX out there can do 2 updates to the orderbook in a second. NASDAQ, Arca, and so on can do in 18 microseconds (not just update, but entire SIP processing). So quite a difference.

> Question then is... how does one prevent gaming of the system? I mean you should't trust central venues. I think the satisfactory solution I came up with is one that relies on several things:

![](/sigils/sitful-chat.svg#floatsigil): So striking a balance between autonomy and efficiency, sort of the classical cleft between decentralized and centralized systems

>![](/sigils/mister-todteg.svg#floatsigil): Yep. I mean settlement is and should be done on-chain, and exchanges should not be involved.

> So the solution I came up with for reducing trust requirement for central matching engine is: 

> * Rapid witnessing of peer order submissions 

> * Signed payloads for all order events 

> * Signed execution of trade by the exchange 

> * Separation of data locality to outside of the exchange company

> So 1: you could imagine a hypothetical situation where you have a car accident (hopefully no one was hurt) and bunch of people come by and take their own pictures from all kinds of different angle immediately,and then posts them everywhere; that is a pretty good indication that the event did happen - witnessing by independent parties. We can go into sybil attack issues, but that's the first cut at an answer. 

> All orders submitted should be signed by the person who owns that wallet - this is the easy part. When the orders are matched on the orderbook management, the exchange signs it. So an executed trade has three signatures.

> Executed trades, just like order modification, gets rebroadcasted to participants, and some of them are what we call Settlement Data Oracle. SDOs are going to be open sourced software that market participants run that will record market activity, and when settlement is requested, many of these will come together in a quorum to provide counterparty obligations to traders who are involved. Clearly the technical and governance challenges are to make sure collusion is not an issue. 

> What this model allows is cross chain settlement, so you could imagine trading RBTC for ETH -- (RBTC) being RSK's pegged BTC. DEXes currently do not have that capability because DEXes run on a chain, or if they do have cross-chain capability, they need third interoperability chain to mediate; huge complexity cost.

## 'So you might ask... is Tacen integrating urbit into its system...'

![](/sigils/sitful-chat.svg#floatsigil): I'm curious about how the SDOs work -- so are these oracles participating by staking, or is there some kind of reputation system, or what?

>![](/sigils/mister-todteg.svg#floatsigil): Participating SDOs will need to set aside some fund to cover settlement risk, since it requires a degree of coordination, while expected risk is low, we would still want data providers to have insurance fund set aside for parties affected. There could be some staking, but that detail is something we're working through at the moment. There could be a governance token that sets quorum requirements for data, as well as other platform wide decisions.

> SDOs will also be subject to data audits, we plan on automating the process, with peer audits being conducted. At the very minimum we would need, even before post-trade audits, a consistent record across quorum when settlment is requested.

> We have system in the works for ensuring honest SDOs survive over longer period of time, which is tricky, but we're still working on some part of it. Default failure mode for settlement is depositor getting the money back. Even before that though I think I should point out that traders create their own contract through function call on a contract factory. That way we ensure consistent rule across the board.

> The contracts, I mean the escrows, are non-upgradable, and we intend on getting it audited to demonstrate that there is no effective control from us. We can't withdraw, command to send fund, or do anything with trader's own escrow.

![](/sigils/sitful-chat.svg#floatsigil): You mentioned earlier that at least half of your employees are on urbit, and I think it's easy to see some philosophical and technical dovetailing between these two projects. Do you have any plans or ideas for any integrations with urbit?

>![](/sigils/mister-todteg.svg#floatsigil): Yep! One thing that is unique about Tacen is that we are not a protocol play. It's funny when one say this but many of the blockchain projects out there are reinventing the wheel. 

> We want to use proven backend for some of our components. So one could imagine a future where your urbit planet can become a SDO; I mean urbit is already has built in database, distributed messaging primitives, idempotent msg channels, and RPC clients to ETH and BTC. That is a no brainer integration. Imagine if Tacen's planet would broadcast to subscribed SDOs on planets, planets record, and at the same time listen to the constituent blockchains, same thing that currently what we're coding does, except perhaps we write less code and with more reliability. Throughput is a issue, but we can work on that. 

> Another exciting integration opportunity is connecting to your own planet's webapp that is basically a front end, and you connect and trade via exchange backend Tacen or any other exchanges provide; you host your own exchange app (at least start with front end).

## 'Imagine if Tacen's planet would broadcast to subscribed SDOs on planets, planets record, and at the same time listen to the constituent blockchains'

![](/sigils/sitful-chat.svg#floatsigil): That's a really cool prospect, it's exciting to watch all of this technology of personal autonomy assembling in front of us. So what does the horizon look like right now? I see on that site that you're aiming for a release later this year.

>![](/sigils/mister-todteg.svg#floatsigil): I think renewed focus on distributed apps, and owning your own data will change how applications are architected. What I'm trying to do is to separate out the data from function, and so far it's looking good.

> I think that there will be other broadly accepted models that will gravitate towards owning your data, auto-audit any application you run. Perhaps even the future of corporate model will center around providing maximum value, rather than monetizing customer data or try to profit from "hacking" their customer behaviors. I personally see this as American as one could get. Business should be solely focused on maximum value for their customers, rather than be distracted with customer behavior.

> As for Tacen... we're slated to do close alpha test soon (around late April) and launch open beta around Sept-Nov time period. We have the first cut at a working settlement flow, tested it against ETH, RSK, ETC, and other blockchains supporting solidity (easier us to develop for now). We did a cross-chain settlement test a few months ago and that works as intended
Backend is coming along nicely too

> Front end is serviceable, we'll get better at it as well. Only thing that's not fixed yet is the order summary text there lol -- placeholder for now.

[![](/img/tacen.png)](/img/tacen.png)

* *c/o ~mister-todteg*

![](/sigils/sitful-chat.svg#floatsigil): What are your expectations or worries about the regulatory environment? I'd imagine that plays a pretty big part in how you build these systems

>![](/sigils/mister-todteg.svg#floatsigil): Well, I think like weather, regulatory environment is unpredictable. There are factions within those regulatory bodies with wildly different interpretations about the law. What I do think is that laws themselves are unlikely to change drastically, meaning most of the laws impacting AML compliance are relatively static, but the interpretation and enforcement policy will change towards more control.

> What I think is that while many companies like Tacen strive for full compliance, there is a limit to what companies can do. For an example, companies can't do Beneficial Owners check for each entities at infinite depth. There is limit to how much check can companies do to prevent prohibited persons from trading crypto. However, there are many innovative ways to ensure maintaining privacy and compliance.

> For an example, while an exchange will not engage in financial surveillance, it can perform sufficient due diligence from publicly available info. Many do this with IP addresses, geo fencing, and other heuristics to make risk based assessment whether to allow service usage. What we're doing with Tacen is that we're trying to design a system that is fully in compliance, and to forecast where the interpretation of the laws will lead - in particular Securities Act of 1933, Securities Exchange Act of 1934, Bank Secrecy Act of 1970, and Title III of US PATRIOT Act of 2001.

> An example of this work is how we are designing the escrow contracts and where the locus of control exists for the purposes of peer settlement. By excluding the exchange from that process, we remain compliant. Forecast from compliance side of angle is the potential interpretation of financial obligation state propagation as being tentamount to money transmission. We avoid that altogether. Unfortunately I can't go into too much details on this, as your readers might get some kind of migraine.

![](/sigils/sitful-chat.svg#floatsigil): How about a last personal question to finish off -- you run a private group for firearms discussion and enthusiasts; what's your favorite gun you own and why?

>![](/sigils/mister-todteg.svg#floatsigil): Haha I own several guns - an AR, a pistol, a shotgun, and more... It's hard to choose, but my favorite is Ruger 10/22 Takedown. It's a neat little gun, fun to shoot and my friends love it too.

> It has virtually no recoil, and ammo's relatively cheap. Great gun to introduce people to firearms.

> Thanks again for having me. This was fun!

--- 

Big thanks once again to ~mister. You can read more about Tacen at the [official website](https://tacen.com/), join the [Telegram group](https://t.me/tacen_app), or the Urbit group at `~mister-todteg/tacen`.

---

*This post is also available on Urbit, where you can post comments. Join [~matwet/networked-subject](web+urbitgraph://group/~matwet/networked-subject/) and open the Networked Subject notebook.*