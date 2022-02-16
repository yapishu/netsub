---
title: "Updates & announcements: Late 2021"
date: 2021-11-04T14:00:13-05:00
draft: false
toc: false
images:
  - /posts/updates-announcements/cover.gif
tags: 
  - urbit
  - update
  - meta
---

![](/posts/updates-announcements/cover.gif)

This blog has been quiet for a few months, but you shouldn't confuse that with a lack of activity.

I was awarded the opportunity to fulfill a grant for the Urbit Foundation, doing writing for urbit.org. You can see the first results here -- the [Star operators' guide](https://operators.urbit.org/guides/running-a-star) and [Galaxy operators' guide](https://operators.urbit.org/guides/running-a-galaxy), on the new [Operators](https://operators.urbit.org/) microsite. During the summer I was very graciously hosted on an episode of [The Stack](https://soundcloud.com/user-628294386/networked-subject), a podcast about Urbit. I worked with `~master-forwex` on producing a Docker image and documentation for the [Bitcoin full node](https://hub.docker.com/r/wexpertsystems/urbit-bitcoin-node) necessary for `%btc-provider` -- anyone with a Linux instance and a few hundred gigs of disk space can run this with very little setup. You might have noticed a few changes to this site -- specifically, [planet sales](https://pay.subject.network) are now automated, using [BTCPayServer](https://btcpayserver.org/) (tutorial forthcoming!). `~matwet` was migrated to layer 2 as soon as it was made available, and purchasers no longer need to pay **any** transaction costs. Planets on layer 2 are also able to perform all Azimuth transactions (e.g. setting networking keys) without fees by using Tlon's roller. 

Urbit's first convention took place in October; [Assembly](https://assembly.urbit.org) was in downtown Austin, Texas, and by all estimations was a great success. The quality of the attending crowd was exceptional, and the event had a bubbling, excited energy. More than once, a stranger introduced himself as a fan of this blog 🙂. Plans were revealed, [lessons](https://natareo.github.io/assembly-workshop-2021/) were learned, strategies were developed, and I went home with a nearly manic sense of optimism about what lies ahead.

[![](/img/grid-2.jpg#floatright)](/img/grid-1.jpg)

A great deal has changed over the last few months on the network; specifically, the long-awaited launch of [layer 2](https://urbit.org/blog/rollups) lays the groundwork for large-scale adoption, and 3rd party software distribution has substantially simplified app developent. After only ~a month, there is a small ecosystem of 3rd party apps for Urbit -- for example: 

- A [friends list](web+urbitgraph://~paldev/pals) app
- WebRTC [video chat](web+urbitgraph://~dister-dotnet-ritpub-sipsyl/urchatfm)
- A self-hosted [RSS reader](web+urbitgraph://~tiddys-sammut-posnev/headline)
- [P2P chess](web+urbitgraph://~finmep-lanteb/chess)
- [Decentralized poker](web+urbitgraph://~bacrys/pokur) (developed by `~matwet` resident `~hodzod-walrus`!)

You can see new apps announced in [The Forge](web+urbitgraph://group/~middev/the-forge)'s `foundry` notebook. Beyond apps, the last few months have featured many other developments: 

- [Port](https://github.com/urbit/port/), the official GUI client, now has **native Windows support** (no more WSL!)
- Work has begun in earnest on [Nock hardware](https://github.com/mopfel-winrux/NockPU) designs
- The [Stardust](https://groups.google.com/a/urbit.org/g/dev/c/EQVU2-GKo04) project has culminated in [star.market](https://star.market/), which allows for the conversion of Urbit stars into fungible, fractionalizable ERC20 tokens
- DC Spark's [Urbit Visor](https://github.com/dcSpark/urbit-visor) browser extension exposes Urbit's APIs to Chrome browsers and extensions, opening up the possibility of Urbit as a personal backend for off-network web services
- Also by DC Spark: [Authenticate with Urbit](https://medium.com/dcspark/authenticate-website-users-using-urbit-id-e6dc8c4cb4fa) for the traditional web
- Native [Lightning network](https://urbit.org/grants/volt-lightning-on-urbit) P2P payment support draws near
- `Mars::Base::10`, a TUI [Landscape alternative](https://github.com/Zaxonomy/mars-base-10)
- [Speech recognition](https://github.com/hosted-fornet/ursr) for Urbit
- The number of [providers](/planet-sales) continues to grow
- A project to build collective ownership tools for [galaxy DAOs](web+urbitgraph://group/~winlud-dirnet/point-dao) has been undertaken
- Tlon launched a beautiful [network explorer](https://network.urbit.org/)
- A [funding ecosystem](https://www.the-combine.org/) has begun to take form
- [Tirrel](https://tirrel.io) is rolling out payment rails frameworks for Urbit apps and planet sales with [Studio](web+urbitgraph://~tirrel/studio)
- A new Urbit news site, [The Orbis Ledger](https://orbisledger.news/), was launched by the hosts of [The Stack](https://soundcloud.com/user-628294386) podcast
- Several other not-yet-public major projects will be announced soon

Things are now happening so quickly that I've had to update this draft repeatedly before publishing. This is only beginning; Urbit is approaching escape velocity. See you on Mars. 