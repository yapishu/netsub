---
title: "Bitcoin on Urbit With ~timluc-miptev"
date: 2020-12-17T14:28:28-06:00
draft: false
toc: false
images:
  - /img/bitcoin-cli.png
tags: 
  - urbit
  - bitcoin
  - timluc-miptev
  - btc-provider
---
![](/img/bitcoin-cli.png)

**Note**: *Urbit's Bitcoin wallet was launched in May 2021. You can learn how to use it [here](/posts/btc-wallet-config/)*.

Recently it was announced that an Urbit [grant bounty](https://grants.urbit.org/bounties/2056919898-bitcoin-full-node-provider-and-wallet) was completed, for software that would act as an Urbit-native bitcoin wallet and a bridge to mediate between a ship and an external BTC full node.  [~timluc-miptev](https://twitter.com/basile_sportif), the developer who [fulfilled the grant](https://github.com/timlucmiptev/btc-agents), was generous enough to spend some time with me for an interview, conducted over the course of a couple of days in a chat channel. 	

If you'd like to hear more about this project from `~timluc`, be sure to check out his [Developer Call](https://www.youtube.com/watch?v=LaWNHMmxQag) and recent feature on [The Stack](https://soundcloud.com/user-628294386/episode-4-bitcoin-urbit-with-timluc-miptev), a new Urbit-centric podcast. Check out their episode about [the future of Bitcoin and Urbit](https://soundcloud.com/user-628294386/episode-5-buy-urbitcoin-with-christian-langalis) 	with `~pindet-timmut` as well. 

---

![](/sigils/sitful-chat.svg#floatsigil): First, a little about yourself: what's your background, your day job, your interests, and how did you get interested in Bitcoin and Urbit?


>![](/sigils/timluc-chat.svg#floatsigil): I got interested in Bitcoin in the summer of 2013 right before it went really crazy, but then didn't buy until the absolute top later that year, and not in large quantity. After that I mostly stayed out until 2016, and then got really heavily into Eth, which leads me into my Urbit interest.

>I was way more interested in Eth than BTC in 2016-2018, mostly because a) I didn't understand BTC's monetary properties at all and b) I thought Eth would scale, at least in layer 2.

>I found Urbit in early 2017, because I had read He Who Shall Not Be Named's blog and was wondering what he was up to. My initial reaction was "oh this is pretty cool" and my second reaction was "you can probably just build this all on Eth, so I'll work on that angle."

>As 2019 came around, it became more and more clear that Ethereum and applications on top of it had fundamental problems, and I disconnected from the space somewhat.

>I revisited Urbit in late 2019, and it was at the "barely useable but with some people on it" phase. I got pretty into it and learned Hoon, but it wasn't immediately obvious how to use it, and I had some other commitments.

>I finally dove in all the way in May 2020, when I learned Nock and [wrote the Nock guide](https://blog.timlucmiptev.space/part1.html). At that point I realized: "oh wow, there's a lot of really low-hanging fruit here, and if I don't pick it, it's just gonna hang there forever." That's informed my perspective since: Urbit is great, it's improving rapidly, and if you think something obvious should be done you need to just do it.

## ❛Think Urbit Substack -- we are probably 3-8 months away from a very very good and useable version of that existing.❜

![](/sigils/sitful-chat.svg#floatsigil): Great answers; so, the reason for this interview is the project you've been working on, Bitcoin support for Urbit. could you give a 1000-foot view of what exactly this project does?

>![](/sigils/timluc-chat.svg#floatsigil): Sure -- the initial version of the project has two major goals:

>1. Abstract away underlying BTC details so that you can say "send X amount of BTC to Y Urbit ship", and have it "just work", without having to manage addresses at all.
>2. During the process of (1), have your ship attach metadata to those payments that can be used by other Urbit apps. In the example above, if `~timluc-miptev` sends 0.001 BTC to `~sitful-hatred`, `~sitful-hatred`'s ship will record that the generated transaction on the BTC blockchain was initiated by `~timluc`.

>So now, `~sitful`'s ship has an receipt stored, saying that `~timluc`
sent 0.001 BTC on X date, and another app on `~sitful`'s ship could query that receipt store, and say "send a group invite to My Cool Group to everyone who has paid me 0.001 BTC". Obviously more will need to be layered on in terms of grouping invoices etc, but it's pretty easy to see how this expands quickly and injects some really interesting information into Urbit.

>So backing up one step to a simplified statement of the two initial goals:

>1. allow BTC payment between ships without worrying about underlying BTC details 
>2. generate receipts of BTC payments that other Urbit apps can use to give content access

![](/sigils/sitful-chat.svg#floatsigil): This sounds a lot like the services provided by several multi-billion dollar businesses. What are some of the immediate and second-order effects that it makes possible? What are the cool things that you want to see happen with it?

>![](/sigils/timluc-chat.svg#floatsigil): Right, so wrt those multi-billion dollar businesses: if Urbit gets a decent-sized userbase, and we can integrate Lightning, this absolutely heads down that road. I'm extremely optimistic on both counts.

>Immediate effects: can pay other Urbit ships easily and manage the information about who has paid you. The second-order effects should be a rapid growth in content-offering services on Urbit. Think Urbit Substack -- we are probably 3-8 months away from a very very good and useable version of that existing.

>Also member groups and forums should happen quite quickly as a result.

>The next-order effects after that probably look like mini-Urbit virtual economies and communities forming. Urbit users can already provide a wide range of services to each other, and have significant outside income in general, and so I'd expect to see that start to circulate internally a lot more.

>Looking back at my answers, I think your implicit question is something like: "what is different about this as opposed to existing BTC/Lightning capabilities outside of Urbit?"

>The answer is that Urbit's huge win is making your id+data programmable, and this adds "money" to that programmable id+data bundle, which is a ridiculously powerful upgrade.

>Urbit without money is still pretty cool, and BTC without Urbit is still pretty damn useful/critical, but the two combined are "civilization-complete" (h/t `~naltyc-wornes`)

## ❛Obviously that can be considered mostly a failed experiment now on Eth... but it's happening on Urbit every day.❜

![](/sigils/sitful-chat.svg#floatsigil): I love that 'civilization-complete' phrase too. I've also seen you use the term 'primitives' in relation to this stuff, which I think is suggestive of how this can work as a basic building block for sophisticated possibilities. If I recall correctly, you've mentioned elsewhere that you see this as kind of an alternative to previous efforts to build Eth layer 2 solutions that ran into intractable problems. What makes Urbit easier to work with for those purposes?

>![](/sigils/timluc-chat.svg#floatsigil): Let me start with "primitives", since it's an over-used word that also gets thrown around a lot wrt the hilarious (and kinda fun) ponzis on DeFi. For me it means that I try to keep my focus on the 2 really simple goals I outlined above, and I don't consider the project complete until they are really clean, simple, and robust. As you note, the sophisticated possibilities emerge quite rapidly from that point.

>In terms of Eth/Layer 2... as I mentioned, I got into Eth originally because I thought it could fulfill the promise of programmatic identity that could then participate in lots of different operations. One of the use cases that I really, really wanted to hit was offloading the networking/reputation-building that was typically done in cities to virtual worlds. Obviously that can be considered mostly a failed experiment now on Eth....but it's happening on Urbit every day.

>I didn't understand why Urbit was better until someone asked me to explain, clearly, why you couldn't just build an Urbit-like overlay in JS or whatever. As I worked it out and thought about why I liked programming on Urbit, I realized that it was because any project on these lines (letting multiple users interact on the Internet) ends up falling squarely into one of 3 buckets.

>1. Unix 
>2. "Lines in a database"--all the user interactions are managed by one central app that has simplistic possibilities (FB, Twitter, Insta)
>3. your own OS

>Most projects realize they're going to have to do 3, and 3 is really hard, so they end up doing 1 or 2.

>What makes Urbit easier to work with is simple: it stares 3 straight in the eye and goes for it.

![](/sigils/sitful-chat.svg#floatsigil): Stepping back a little bit, lets talk some about how this is actually implemented. This software gives a ship the ability to interact with the BTC blockchain by allowing a ship to talk to a full node, right? what external software is required, and what is the setup/configuration process like?

>![](/sigils/timluc-chat.svg#floatsigil): So for the full answer, I'd recommend the developer call:

{{< youtube LaWNHMmxQag >}}

>But at a high level, it's fairly easy to explain.

>Any Urbit ship can run a Urbit program called btc-provider, which lets you configure a full node to talk to. This is simply a matter of giving the URL of that full node and auth credentials. The most common case would be running the node locally.

>Then that `btc-provider` program can act as a proxy for any Urbit ships that want to query the BTC blockchain. The program has some simple whitelisting commands: it stores a list of ships that are allowed to query through it.

>There are currently 3 external software requirements for anyone who wants to run a full node and make it accessible through `btc-provider`:

>* Fully sync'd Bitcoin core (~350GB)
>* `Electrs` for address indexing (BTC core doesn't have fast lookups of "tell me the full history for X address")
>* A simple JS HTTP proxy that `btc-provider` calls out to, which makes calls to BTC core and `Electrs`, packages up their results, and sends back to the Urbit app.

>It's not the hardest install process, but core takes awhile to sync.

>Install/setup looks like:

>1. Have Mac/Linux 
>2. Install `bitcoind`
>3. Sync the blockchain
>4. Install Rust and then [Electrs](https://github.com/romanz/electrs) (fairly easy)
>5. Have Node installed
>6. Run a script to start all 3 at once locally
>7. Install `btc-provider` on your Urbit, and run a command like `:btc-provider|command [%set-credentials api-url='http://localhost:50002']`

>At that point your ship is ready to accept BTC queries from the wallets on ships you've whitelisted, including your own.

## ❛If anyone is interested, there is plenty to be done.❜

![](/sigils/sitful-chat.svg#floatsigil): If booting a ship can be a learning experience for most people, I'd imagine even fewer would be running their own full node. Do you expect this would mostly be service operated by stars/galaxies for their children, digital sovereignty enthusiasts aside? And how difficult or practical do you think a lightweight node vane or Gall app may be?

>![](/sigils/timluc-chat.svg#floatsigil): Short answer: yes, I expect this to be mostly run by stars, for the reasons you state.

>A light node Gall app is doable, it’s just a substantial project and I think getting the other stuff right is more important. Welcome to Urbit—way too much cool stuff to do, finite time. Order of operations matters.

>This is analogous to how hosting works now: I run my own ship, but for friends/family who don’t care about privacy yet, I run ships for them. There’s a clear upgrade path if/when they do want it, and often that’s the best you can do in these situations—clear upgrade paths with low switching costs.

![](/sigils/sitful-chat.svg#floatsigil): A question from my girlfriend: How novice-friendly is this implementation? Is this CLI-only or is there a GUI in the works?

>![](/sigils/timluc-chat.svg#floatsigil): GUI is happening. I’m polishing the backend now, and then I’ll be working with the Landscape team to integrate.

![](/sigils/sitful-chat.svg#floatsigil): Very cool! For somebody excited about this kind of thing who wants to pick up where you left off, what are the next steps you'd like to see taken, or features that require a little more investment, besides Lightning support?

>![](/sigils/timluc-chat.svg#floatsigil): So I think you're asking how programmers could contribute to this effort? The main area would be ongoing maintenance, polishing, and feature addition. I have a pretty clear roadmap for things that need to be added (one example: more efficient querying of block updates from the provider). Either I'll do those long-term, or else I'll pass them to someone else who is interested in this, which in turn would make me more available to start into Lightning or similar. If anyone is interested, there is plenty to be done.

>To be clear: I'm not "leaving off" until there is another good maintainer who is a single point of responsibility. If that never happens, I am fine to polish this forever myself; that's how important I think it is.

![](/sigils/sitful-chat.svg#floatsigil): Yes, that's what I meant. I think that's an admirable approach to take, because I also think this is going to be one of the premier features in Urbit that draws new users and use cases. Things really seem like they're setting in place w/r/t Bitcoin and cryptocurrency becoming a permanent part of the financial landscape -- obviously Bitcoin is at a stratospheric all time high as we speak, with lots of buy-in from big institutional investors that seems to me different from late 2017; I see more things starting to percolate out of the immediate sphere of crypto nerds and speculators, like Justin Murphy tokenizing his creative content; it looks like it's exceptionally good timing for this to be announced in terms of attention.

One last question and we'll call it a wrap, unless you have anything else you want to touch on: should Urbit move off of Eth, and if so, to what?


>![](/sigils/timluc-chat.svg#floatsigil): Man, this one is loaded. My answer is that Eth will leave Urbit no choice but to move off it because of scaleability issues, and my preference would be some sort of UrbitChain just for Urbit assets. We probably have a little bit of time left, and I'm ok with other things being a bigger priority, but it's an issue. There was a lot of activity about this on urbit-dev in the summer, and then Eth asset prices went down. I'm ok-ish if people want to go with something like optimistic rollups on Eth.

>And to comment on your prior part about BTC becoming more of the landscape: actual BTC technical work is going very fast now, and it's a very exciting place to be just as the price rises.

---

Check out the [Urbit grant bounties](https://grants.urbit.org/bounties) for more projects worthy of some attention. Big thanks again to `~timluc` for his generosity and time!
