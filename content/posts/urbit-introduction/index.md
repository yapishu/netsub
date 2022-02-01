---
title: "Urbit: An introduction"
date: 2020-08-25T14:38:09-05:00
draft: false
toc: false
description: A presentation written for the inaugural Urbit DFW meetup.
images:
tags: 
  - Urbit
  - introduction
  - essay
  - Urbit dfw
---

*Note: this is a copy of a presentation initially posted on my [personal blog](https://hyperstition.al/post/Urbit-an-introduction/). I am posting it here in the interest of centralizing Urbit-related content that I produce.*

This essay was written to be given as a presentation at the (temporarily suspended) Urbit DFW meetup group. You can watch it on youtube [here](https://www.youtube.com/watch?v=RI1TS0PWQAA).

### introduction

[![](/img/slides/slide1.png)](/img/slides/slide1.png)Urbit is a personal server. that is to say, it's a computer acts as an agent on your behalf to talk to other computers on the internet. Right now, we all use computers that belong to megacorporations and serve their interests; when you log into Facebook and talk to your friend, all the real computation and storage is done on servers that belong to Facebook. In a very real sense, they own your data. It's very difficult to avoid at this point. The goal of Urbit is to address the technical issues that led to this state of affairs, so that everybody can run and control their own software. there are a lot of projects that have tried to address one or two of the many problems that Urbit tries to solve, but I doubt there is any other current project as ambitious or farsighted. Urbit is an open source project primarily developed by a company called [Tlon](https://tlon.io), in San Francisco, and there's another company in Austin also working full-time to develop it, called [urbit.live](https://urbit.live).

Urbit is famously complicated to describe; this is due to a combination of complexity, an opaque naming scheme, and its genuinely novel ideas and clever recombinations of old ones. It can seem like it leaked into our world from an alternate timeline. Thinking about Urbit requires you to modify a lot of conceptual models that you're used to using to think about computers and the internet. I'm not any kind of expert on any on these subjects but i've spent a decent amount of time trying to digest the technical parts of Urbit, and my goal here is to convey that understanding to you, and connect the technical decisions and constraints to the problems being solved. This presentation is aimed at someone who has heard of Urbit but doesn't quite get it yet. So, to begin, here is a list of questions:

What if your computer was deterministic -- a minimal, strictly defined mathematical function that given an event sequence, always ended up in the same state? How would it be different from the computers we're used to, and what advantages might it have? What would a network of these computers look like? What if that computation was standardized, so that an app I write today runs on another arbitrary computer in ten years, or the computer I'm using now still works when I pass it down to my kids?

Why can't I take my social graph with me when I leave Facebook? Why isn't facebook just a protocol? Why has the internet transformed from a peer-to-peer assemblage into a surveillance panopticon? Why are we doing all our computation on some corporation's mainframe as if it's the 70's, while they build dossiers on us? What happened to the dream of using networked computers to liberate individuals?

How do you grow social software to internet-scale without running into the obstacles produced by the conflicting values of different communities? How do we preserve the freedoms of association and speech while still allowing for the enforcement of values within groups? How do you disincentivize spam and bad actors in an open system?

As you may guess, some of the goals of Urbit are to answer these questions and challenges in an elegant way. I will give you a cursory overview of the system and then talk more about the ideas it exists to implement.

[![](/img/slides/slide2.png)](/img/slides/slide2.png)

### the stack

These problems are very difficult to address within the constraints of the current internet, but Urbit is an entire networked computing stack from scratch. The core thesis of Urbit is that all of these are technically solvable problems if you approach them at the right scale. I'll try working from the bottom up here and give a cursory overview without getting too bogged down in details. I apologize if this is a little dense, but bear with me and it should cohere. The second half of this is less technical.

[![](/img/slides/slide3.png)](/img/slides/slide3.png)

Nock is a turing-complete function. You can think of it as the functional assembly code that underpins the entire Urbit stack. Its formal spec is typically described as "small enough to fit on a t-shirt", and it has 12 opcodes, or basic instructions. This is a couple of orders of magnitude smaller than the x86_64 instruction set that your pc uses. Nock is pared down to the minimum elements of what might make up a useful computer; for instance, it defines a single recursive datatype, a binary tree of integers; and its only arithmetic operator is increment.

But a combinator whose only arithmetic operator is increment sounds unusable as a computer; How do you subtract, for instance?

As you may know, if your computer can increment, you can derive all of the rest of the math you'd like out of that operation -- for instance decrementing x by incrementing up to x-1. This is extremely inefficient, but Urbit has a cool hack for this: the standard library has all the operations you would expect implemented as "jets", written in optimized c, which manipulate values in a formally equivalent manner to a specification written in Nock. That is to say, if you ran an incredibly slow version of your urbit without jets that did all its math the hard way but otherwise had the same inputs, its state would end up identical to the jetted version.

[![](/img/slides/slide4.png)](/img/slides/slide4.png)

Nock is virtualized with an interpreter programmed in C called Vere. Vere sits between your urbit and your computer, handing over keyboard and packet inputs to your urbit, and outputs back to your Unix computer to be displayed in its terminal or sent off through its network interface. Right now Urbit runs as a Unix process, but one can imagine Urbit on hardware meant to run Nock and jets directly. For now, Vere is what runs your urbit on your Macbook, VPS, or WSL.

[![](/img/slides/slide5.png)](/img/slides/slide5.png)

Hoon is the system programming language for the Urbit operating system, Arvo. Hoon is a strongly-typed, functional higher-level language that compiles to Nock; it's effectively a collection of Nock macros. Expressions are denoted with 'runes', which are digraphs of ASCII special characters. Hoon code is made up of these expressions and as a result can be kind of scary to look at, but I promise it's not as hard to use as you may think!

[![](/img/slides/slide6.png)](/img/slides/slide6.png)

Arvo is the Urbit operating system. It has a minimal kernel, around 1000 lines of code, plus some kernel modules, called vanes. Arvo's vanes handle the networking, apps, a web server, a version-controlled file system, etc. Your urbit instance's Arvo begins with a blank slate, then processes any events (packets and keystrokes), writes them to a log, and outputs its effects, producing new events and its new state. If you play back this log, your urbit will end up in an identical state.

Urbit is deterministic. Your urbit's state is one giant binary tree that is modified by an event log. Nock takes your state and some new event, applies the event to the state, and outputs a new state and a list of effects. Arvo orchestrates events between vanes.

[![](/img/slides/slide7.png)](/img/slides/slide7.png)

The Urbit network protocol & kernel module is called ames. Ames is peer-to-peer and encrypted, and runs over UDP. It has a global immutable namespace; an instance's name is permanent, and is both an identity and routing address.

Urbit's address space is 128-bit, just like IPv6. the total number of addresses (or 'ships') is effectively unlimited. However, Urbit's address space is also partitioned into classes with different privileges. 8-bit address ships are called galaxies, which are core infrastructure nodes that sign software updates. Underneath galaxies are stars, 16-bit address ships that act as routing infrastructure. Beneath stars are planets, which are ships meant for individual humans. Planet addresses are 32-bit like IPv4, so there's about 4.2 billion of them. Galaxies issue stars, and stars issue planets.

Urbit uses a base-256 phonemic scheme for pronouncible numbers called @p. Galaxies have single-syllable names like `~zod`; stars are two-syllable, like `~marzod`. Planets have two two-syllable names, like `~dalsum-simreg`. This is your identity -- think of it as a combination IP & email address. the words are human-memorable and sound like cool sci-fi names.

[![](/img/slides/slide8.png)](/img/slides/slide8.png)

How do you own these names? Urbit's public key infrastructure is a set of Ethereum contracts collectively called Azimuth. Azimuth acts as an ownership registry for ships and consensus mechanism for network governance. If you receive a ship, your proof of ownership is an ERC-721 token, which is an Ethereum standard for non-fungible tokens. You own this token like you own Bitcoin; as long as you have the master password, it belongs to you forever.

Azimuth technically exists outside of Urbit as a general-purpose PKI; Urbit just uses it as a source of truth. If you dislike Ethereum for political or technical reasons, keep in mind that it's not a permanent intrinsic part of how Urbit works, it's just the current scheme -- though in my opinion it's a pretty elegant solution.

### philosophy

Urbit has very strong opinions, and some design decisions that might not make immediate sense. This is a system meant to replace the internet we've spent half a century building; it needs some really compelling wins to be serious.

For instance, why make a computer with such a minimal core? So you can freeze it and standardize it.

[![](/img/slides/slide9.png)](/img/slides/slide9.png)

A basic idea underlying the design of Urbit is that many of the problems we have today with the way networked computers work is that the technologies composing them accrued in layers atop each other in ways that produce unforeseen conflicts. We have a million identity systems because each layer reinvents everything in the layers below it in mutually inoperable ways. A universal identity system, a secure peer to peer networking stack transparent to the software using it, and key management and cryptocurrency management baked into the core allows developers to offload those problems onto the operating system without worrying about re-implementing them at the app level and fretting over security, dependencies, translating data between layers, etc. These features are basic traits of the platform, available to all software on the system to use.

Urbit approaches the problem of cascading system complexity by introducing a hard brake on the core components, called kelvin versioning. Instead of incrementing version numbers, the versions of Nock and Hoon count down to absolute zero, at which point they can never be changed again. Nock is at 4k at the moment, pretty close to its final state, and Hoon is around 150. This is meant to ensure future interoperability and compatibility, one of the most technically ambitious parts of Urbit. this is a system designed so that your kids can execute code you write today on their future computers, or even boot up your entire ship after an arbitrary amount of time offline or on different hardware. Nock's frozen status means everything above it can be upgraded on the fly, including live upgrades of the kernel, and ensures architectural compatibility forever. I should also mention that Urbit's codebase is much, much more compact than the computers you use every day; the whole stack is somewhere in the neighborhood of 50k lines of code. The Linux kernel alone is somewhere around 12 million.

Standardization also allows for commoditization. Any computer that can run an Urbit can run any other Urbit and all its software. The platform is an open standard, which allows commodity providers to compete over performance without users being locked in or out by proprietary changes or interoperability problems. In practice, this will look something like Urbit hosts running your instance on their hardware for a fee, but you being able to download your instance and all your data to run on another provider, or even your personal computer if you decide to do so. It can also look like hardware implementations of Nock, and Urbit-native computers manufactured by anybody who wants to, like PC clones.

[![](/img/slides/slide10.png)](/img/slides/slide10.png)

As you're probably aware, the internet of today is built on top of the client-server model. You want to connect to other computers, but you don't or can't leave your computer on all the time, so you offload that duty onto a dedicated server run by somebody else. This model grew out of the timesharing mainframes of yesteryear, and it has served us well, but the limitations have become clear; he who runs the server, owns the data. Google runs some excellent email servers and Facebook is fabulous at showing you pictures of people you've dated, but it's 2020 and I don't want to be spied on anymore; why don't we just run this stuff ourselves? First, it's hard, and second, network effects.

Running a personal server today is technically possible. You can rent a VPS, set up and maintain an email server, throw a Mastodon instance on it, connect a bunch of chat services, etc, but it sucks and is a huge pain to maintain and troubleshoot. Worse, you have to be significantly more technically inclined than average to get that far. There is a reason almost nobody does this. Unix is industrial machinery; Urbit is meant for individuals. Your urbit is intended to be manageable like an iPhone, through a simple user interface that allows you to configure the software on it without having to touch the command line (though it's there if you prefer). Unlike an iPhone, it is open source and extensible, and will likely develop an ecosystem of competing interfaces. Today, the interface is called Landscape, a minimal and attractive web UI that you control via your browser.

Network effects on the other hand are a tricky thing to overcome; why should we be confident that Urbit will be preferable to facebook? Why would anybody use a hypothetical Twitter-like service on Urbit instead of actually-existing Twitter, or an alternative like Mastodon? Because, these things are not mutually exclusive. Urbit is a computer, not just a social network. Existing networks and services have API's that allow you to access them via third party clients. Urbit is a general purpose computer that you can straightforwardly program to scrape, display, and store data. Why not program your urbit to scrape your all your feeds and messages in one place? 

Because Urbit is an entire computing platform, every piece of functional utility added to it by software increases the network effect of Urbit; once enough people are using Urbit for whatever disparate purposes they desire, be it running a Bitcoin-trading bot or collated Twitter feeds, they will all find themselves on a decentralized network with a universal identity system and basic messaging utilities. When you and your friends get hooked into some Urbit functionality or another, the external megacorp services are redundant -- so why bother?

Urbit's model is everyone running their own server. A hypothetical Reddit or Usenet-like service on Urbit is an app you install, that communicates with your friends' Urbits, which are also running the app. Existing social platforms can be conceived of as a relatively simple set of rules for displaying content, comments and messages; the hard part is getting everyone in one place, which Urbit's identity and network layers provide for anything built on top of it. It's exciting to imagine the possibilities for rapid experimentation that it makes possible.

[![](/img/slides/slide11.png)](/img/slides/slide11.png)

Urbit presents itself as a critique of the internet as it works today; this isn't purely on technical grounds, but also a vision of a more pro-social internet. The legacy internet was taken over quite quickly by spammers and hackers. It was designed originally to trust anyone by default. If a person sends you mean emails or a remote code exploit, there's little you can do in response -- IP addresses  change and email accounts are free and unlimited. Urbit's solution to this is in its finite address space.

I mentioned earlier that there are 4.2 billion planets, or addresses meant for humans. Your proof of ownership of your address is a cryptographic token. Right now these sell for about $20, though the price is ultimately determined by the market. However, the fact that they cost money is one half of the solution. The other half is that infrastructure nodes can blacklist bad actors. If it costs ten or twenty dollars to get an Urbit identity, and you start spamming or abusing people, people may ask the operators of the routing nodes above you to stop routing your traffic. The cost of a planet is ideally cheap enough to not be a burden, but expensive enough that you can't make up the cost from spamming or be worth burning because you're angry at someone.

One can also imagine reputation systems, where people individually set rules to automatically block crowdsourced lists of known bad actors. I should note that this system is theoretical at the moment; Urbit is designed to make this kind of thing easy to do, but right now it's small enough that everybody is nice to each other.

[![](/img/slides/slide12.png)](/img/slides/slide12.png)

To briefly recap the design of the network, at the top of the routing hierarchy are galaxies, which sign software updates for all the ships below them and issue stars. The star is expected to perform peer discovery and NAT traversal for the planets beneath it. Stars can issue planets, which are meant for people. The connections your server makes with your friend's are directly peer-to-peer where possible, and mediated by a star when it isn't.

An important qualification to understand here is that all traffic on Urbit's network is encrypted. Your host star tells you where to send the packets or gets them through the firewall for you, but nobody but the recipient can read them.

A simple illustration to understand routing: you buy a planet from a star. After your planet is up and running, you want to talk to your friend's planet. Your planet asks your star for its IP address. Your star asks its galaxy, which asks your friend's galaxy, which asks you friends star, and the answer comes back to you the same way. Now that you have his IP address, you send your encrypted packets back and forth directly. [*note: I'm not sure if this is a very precise description but my understanding is that it's something like this*]

The star or galaxy above you is called your sponsor. I described how stars could block abusive Urbits from reaching other ships, but that runs both ways. If your star is unfair or malicious, or you just don't like the guy running it, your planet can 'escape' to another star if it will have you. Stars provide services to planets and in the long run will probably be mostly operated as businesses, and it's a good idea to keep your customers happy.

This is a system of voluntary relations; your star has to like you enough to route your traffic, and you have to like him enough to trust his services and potentially pay him. This creates pressure on both sides to maintain good behavior, and gives a release valve for irreconcilable differences. The sponsor relationship also applies between stars and galaxies. In the long run, this also allows the network to fragment gracefully if it needs to.

[![](/img/slides/slide13.png)](/img/slides/slide13.png)

One of the major development milestones coming up in 2020 is the integration of native Bitcoin and Ethereum support into the Urbit kernel. This is basic wallet support and API calls for full nodes, but big things have small beginnings. What new possibilities are opened up by having secure digital identities and simple system calls for trustless money? Here's an easy example. Write a program that watches for Bitcoin transactions to a wallet you designate, grants access permissions to media in your filesystem for the Urbit ID's associated with the senders, then sends them a message. You now have a decentralized patreon.

[![](/img/slides/slide14.png)](/img/slides/slide14.png)

This is a bit of a personal angle, but I am very attached to the aesthetics and design that Tlon has brought to bear on this project. An example is the sigil system. in the same way that @p converts routing addresses into pronounceable names, sigils convert Urbit names into visual symbols you can recognize. They look kind of alien and mysterious. My girlfriend gave me a painting of mine that I put above my computer.

[![](/img/slides/slide15.png)](/img/slides/slide15.png)

All of this aims at a terminal goal of taking away the necessity of having other people's servers between you and your friends & family. When Urbit conquers the world, your digital life will be fully contained on a device you control, running on an open standard computing model. All your traffic will be private and encrypted, and all traffic meant for you will be addressed to you, personally. You will easily experiment with new social platforms with your friends, which will simply be protocol exchanges between your devices and theirs, and you won't be beholden to or spied on by the Googles of the world. Conflicts will be mediated within the groups you belong to instead of from above. Your home IoT devices will run child ships issued by your planet and be entirely under your control. You will be able to run the device orchestrating your social and financial data online in a high-assurance data center or your home PC. More than anything, your computer and data will belong to you in a way that it simply cannot in today's world.

There's a good deal I've skipped over and a few things I've undoubtedly gotten a little wrong in this already long presentation, but I hope you find this as exciting as I do.

### getting involved

Urbit is young but moving fast. There's plenty to dip your feet into if you find this stuff exciting.

**boot a comet**

> You can jump into Urbit without buying a ship. A 'self-signed' instance that isn't issued by a star or galaxy is called a [comet](https://urbit.org/using/install/#booting-a-comet), and you can spin one up for free. Comets can do almost everything a planet can, at least for now.

**read the docs**

> Urbit's [documentation](https://Urbit.org/docs/) is significantly more clear and robust today than when I first tried learning about it. There's a huge amount of stuff to read on the website, and you could do worse than looking through the [glossary](https://Urbit.org/docs/glossary/); one of the hurdles to learning about this is that it has a good deal of jargon. There's a [blog](https://Urbit.org/blog/) that's had some really excellent posts about design decisions and constraints of various parts of the stack posted recently. There's also a [whitepaper](https://media.Urbit.org/whitepaper.pdf) from a few years ago if you like reading those.

**hang out in u-h**

> The de facto community hangout spot is on `~dopzod/Urbit-help`. it's meant for technical discussion of Urbit, and it's a great place to ask questions, but it's also a pretty typical friendly chat room. [*note: as of late 2020, the defacto group is `~bitbet-bolbel/urbit-community` or `~dopzod/urbit-community`*]

**hoon school**

> Learning to program with Hoon is the single best thing you can do to understand how things work under the hood if you're technically inclined. You can do this yourself by reading documentation and asking questions in u-h, but if you're like me you may benefit from structure. Tlon organizes informal beginner and an advanced Hoon school sessions. You can [sign up](https://Urbit.org/community/Hoonschool/) for the waitlist on the website. [*note: [hooniversity](https://hooniversity.org/) is the new, free, community-run Hoon education course*]

> Hoon is pretty weird, especially if you don't have any experience with functional programming. Don't let me saying that discourage you though -- I completed the 101 course with almost no programming experience. I won't pretend it was a piece of cake, but it probably took 5-10 hours a week for me.

**meetups**

> There are [meetup groups](https://Urbit.org/community/meetups/) all over the world. If you're interested in helping me organize and promote the DFW meetup, feel free to talk to me about it.

**grants**

> Tlon is [incentivizing](https://grants.Urbit.org/) the community development of Urbit by granting address space in exchange for features. For instance, different components of Bitcoin support are being contributed in exchange for grants of stars. This meetup is also part of the grant program. It's worth taking a look!

Feel free to message me on the network at `~sitful-hatred`. See you on Mars.
