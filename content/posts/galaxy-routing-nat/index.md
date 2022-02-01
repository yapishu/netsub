---
title: "Ames indirect connection fixes"
date: 2021-04-20T18:00:35-05:00
draft: false
toc: false
images:
  - /img/ames.gif
tags: 
  - urbit
  - nat
  - networking
  - ames
  - posput-borfes
---

![](/img/ames.gif)

Urbit's networking protocol, `%ames`, uses peer-to-peer connections to let your ship talk to others, with all traffic transported over encrypted UDP. However, in order to allow your ship to discover the IP addresses of ships you want to talk to, you have to query the network -- specifically, your ship asks your star, which asks the galaxies. The answer gets relayed to you, and then you send your packets where they're meant to go. 

Sometimes, however, your packets will not get through, or vice versa. After a 30 second timeout, your ship will try routing your connection through your galaxy instead. This helps you get around  firewalls and home routers that won't allow connections through, at the expense of the speed of your messages arriving.

(As an aside, this service will eventually be performed by stars, but is done by galaxies for the time being.)

If you are experiencing significant latency between yourself and another ship, it may be because one of you is having your connection proxied through a galaxy. Besides very slow messages, there's no obvious way to tell whether this is the case, but there are a couple of tricks:

## Detection

### One-liner

> The command to use in the Urbit dojo is:

{{< termdojo "urbit" "sampel-palnet" >}}
~sampel-palnet:dojo> =/(ss .^(ship-state:ames ax+/=//=/peers/~zod) ?>(?=(%known -.ss) route.ss))
~sampel-palnet:dojo>
 

 
{{< /termdojo >}}



> Replace ~zod with the actual ship name.

> You'll see something like this as a response, if your ship is in fact directly linked to the other one (instead of being indirectly routed by a star):

{{< termdojo "urbit" "sampel-palnet" >}}
> =/(ss .^(ship-state:ames ax+/=//=/peers/~zod) ?>(?=(%known -.ss) route.ss))
[~ [direct=%.y lane=[%.y p=~zod]]]
 
{{< /termdojo >}}


([Source](https://rudd-o.com/linux-and-free-software/how-to-find-out-if-your-urbit-ship-is-directly-or-indirectly-routing-to-another-urbit-ship) -- thanks, `~posput-borfes`!)


If your output instead says `direct=%.n`, this means your connection is indirect -- i.e., proxied through a galaxy. 

### Debug

Alternatively, you can run this in the dojo:

{{< termdojo "urbit" "sampel-palnet" >}}
~sampel-palnet:dojo> |start %dbug
 

{{< /termdojo >}}

Then switch to Landscape, and go to `/~debug` (e.g. http://localhost:8080/~debug ), and click on the ames tab. This will show your route for each ship.

![](/img/debug.png)

Look for the value next to 'Route' -- 'Indirect' means your connection is proxied.

After you're done, you may want to turn off `%dbug` -- I found it slowed down my ship noticeably:

{{< termdojo "urbit" "sampel-palnet" >}}
~sampel-palnet:dojo> |fade %dbug
 

{{< /termdojo >}}

--- 

## Fixes 

### Home router

If you find that your ship is routing through your galaxy for all connections to other ships, it's because your ames port is being blocked by your firewall or router. 

If you are behind a home router, look up a guide to [configure port forwarding](https://www.noip.com/support/knowledgebase/general-port-forwarding-guide/) for your router's model or manufacturer. 

You will need to forward your ames port -- by default, this is a random UDP port between 50000 - 59999. You can forward that entire port range to the computer running the ship, or you can restart your urbit with a flag to specify a particular port, and forward that one:

{{< terminal "terminal" "~/urbit" >}}
$> urbit -p 54321 sampel-palnet
 
 
{{< /terminal >}}

Keep in mind that if you are forwarding to a specific IP address, that device should have a static private IP -- i.e., if your home deskop runs your urbit and its IP address is 192.168.0.10, you will want to configure it so that it always claims that IP address rather than having a random one assigned via DHCP next time it reboots. Google 'static ip' + your operating system if you aren't sure how.

A simpler fix is to instead boot your ship with `urbit-king`, (aka King Haskell) an alternative binary. `urbit-king` includes UPnP support, which will automatically configure port forwarding for compatible routers. 

If you want to try it, you may want to take a look at the parameters, as they're slightly different from the `urbit` binary written in C. To boot your existing pier:

{{< terminal "terminal" "~/urbit" >}}
$> urbit-king run /path/to/sampel-palnet
 
 
{{< /terminal >}}

(Make sure you've shut down your ship before booting it!)

`urbit-king` also has native support for disconnecting and reconnecting to your instance without having to use `tmux`. 


### VPS

If you are running your ship on a remote server, there are two possibilities for your traffic's obstruction -- a local firewall on your VPS, or a firewall controlled via the dashboard provided by your provider. This latter type can't be adjusted from inside your VPS -- you'll have to adjust it on your host's web control panel.

Fixing this for a local firewall is simple enough -- just set an iptables rule to allow UDP ingress on that port. For instance, assuming you have bound your urbit to port 54321:

{{< terminal "terminal" "~/urbit" >}}
$> iptables -A INPUT -p udp -m udp --dport 54321 -j ACCEPT
 
 
{{< /terminal >}}

The latter type of firewall is a little less uniform, and I can't provide a universal guide here, but you can take a look at the example of Oracle's firewall [here](https://subject.network/posts/free-cloud-oracle/#configuring-software). The main thing to keep in mind is that you want to allow UDP ingress traffic on whatever your ames port is, or the port range it uses.