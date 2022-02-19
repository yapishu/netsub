---
title: "Urbit Apps for Umbrel"
date: 2022-02-17T20:57:00-06:00
draft: false
toc: false
images:
tags: 
  - urbit
  - umbrel
  - bitcoin
  - selfhosting
  - mopfel-winrux
  - pindet-timmut
---

![](/img/urbitumbrel.gif)

[Umbrel](https://getumbrel.com) is a software project targeted at Raspberry Pi's which shares some common spiritual threads with Urbit. It's a personal server -- an operating system or stack you can load onto a device, and straightforwardly run your own Bitcoin full node with minimal technical intervention. I've been describing it as 'a full node with an app store', but it's aiming to be more general-purpose than that -- more like an easy out-of-the-box mini-server that specializes in self-hosted, self-sovereign alternatives to Megacorp services. In addition to Bitcoin and Lightning, some of the apps you can install include NextCloud, BtcPayServer, SimpleTorrent and Gitea. They also sell a sleek prefab version called [The Bitcoin Machine](https://thebitcoinmachines.com/). Its real strength is the minimal setup and maintenance burden, though -- enthusiasts might enjoy managing these tools, but most people don't want to spend hours on this kind of thing.

Last week, Umbrel's new update included a couple of apps you might recognize:

![](/img/umbrelapps.jpg)

For a couple of weeks, `~mopfel-winrux` and I held mini-hackathons in our DMs, figuring out what it would take to get Urbit to run on an Umbrel, and how we could let the Urbit's Bitcoin wallet use the Umbrel as a backend. We were elated at how quickly we made progress, and I was impressed by how simple it was to package and submit the apps -- I am not a developer, but I can put together Docker images. 

`~mopfel-winrux` has more programming chops than me, so he put together the Urbit management interface, and I threw a simple theme together. We agreed that a simple, web1 vibe would be ideal; a portal from the old internet into the next one. Even better, it uses no javascript, just CSS and screen refreshes, like driving a stick-shift. 

![](/img/umbrel-ui.png)

The app includes GUI affordances for basic ship management -- you can boot a comet with a button, or upload a keyfile to boot an Azimuth point like a planet. You can also start and stop the ship from the interface, so you never need to touch a command line. A bug slipped through release that rendered the pier upload function broken, but a fix is already submitted. 

![](/img/btc-connector.png)

The second app, which I took the lead on, was a simpler affair; the Bitcoin full node stack for `%btc-provider` includes three components; `bitcoind`, `electrs`, and a Node Express proxy. The first two were already present and configured correctly out of the box on Umbrel, which just left the proxy. Even better, `~master-forwex` and I had worked on a [full node stack](https://hub.docker.com/r/wexpertsystems/urbit-bitcoin-node) Docker container a few months before -- all that was left was to strip out the extraneous components, pass the environment variables to the container, make some light modifications to the proxy's source code, and put a simple informational web interface on top. It works like a charm, though I'd like to add a button to automatically configure the `%btc-provider` settings.

If you have an Umbrel, you can install these apps from the app store and boot up your ship with a few mouse clicks, self-hosted on your own server, and even host your ship's own Bitcoin wallet provider on the same device. This is a *massive* reduction in complexity compared to doing these things the traditional way; I'm very proud of how successful our project turned out. 

![](/img/urbitapp.jpg)

While `~mopfel` and I were chipping away at these, I began talking to the `@getumbrel` Twitter account with technical questions, and tried to explain the project to the person on the other side. Unbeknownst to us, `~pindet-timmut` of [Tirrel Corp](https://tirrel.io/) was thinking of the same problems and app ideas as us concurrently, and pitched them the Umbrel founders without knowing we were working on it. I think it is thanks to his efforts and standing in the Bitcoin community that they were so receptive to our project, and generously gave it headlining attention in their app update and on Twitter. 

Reception has been truly fantastic -- we had many enthusiastic reactions and brought a whole mess of comets onto the network just in time for L2. Even better, `~mopfel` and I have begun partnering on other projects -- much more lies ahead!