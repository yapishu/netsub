---
title: "%duiker & %serval: bittorrent on Urbit"
date: 2020-10-10T17:32:53-05:00
draft: false
toc: false
description: duiker/serval is a software suite for running an Urbit-native bittorrent tracker.
images:
  - /img/duikers.jpg
tags: 
  - urbit
  - bittorrent
  - tracker
  - palfun-foslup
  - paldev
---

[![](/img/duikers.jpg)]()

%duiker/%serval is a [spanking-new](https://groups.google.com/u/1/a/urbit.org/g/dev/c/7hm3MGsmL_4) software suite for running an Urbit-native bittorrent tracker, developed by `~palfun-foslup` with `~paldev`. (Astute readers may note the [sly](https://github.com/WhatCD/Gazelle) [references](https://github.com/WhatCD/Ocelot).) 

This is both a tracker and a command line interface for users to post and retrieve magnet links, which hooks into Urbit's identity system, and is written in a combined \~1,400(!) lines of Hoon, the Urbit system programming language. 

Note that this is not a client for downloading torrents; think of it like a torrent site you can only access through your dojo. 

[![](/img/duiker-cli.png)]()


## %duiker

[%duiker](https://github.com/Fang-/suite/blob/master/app/duiker.hoon) is the frontend pictured above. Right now, you can use your urbit to connect to `~paldev`'s %serval instance and add or retrieve magnet links by running this in your dojo:

{{< termdojo "urbit" "sampel-palnet" >}}
~sampel-palnet:dojo> |link ~paldev %duiker
 

{{< /termdojo >}}

This will drop you right into the tracker; press `?` and it will provide you with a hefty wall of text on how to use it! The basic navigation is pretty simple, you can list torrents and page through them:

{{< termdojo "urbit" "sampel-palnet" >}}
.	reprint the file list
r	refresh the file list
[	previous page
]	next page
<	first page
>	last page
0-9	view file details
~paldev:duiker> 

{{< /termdojo >}}

The UI automatically refreshes as you press the keys; it's a very retro-computing vibe.

As of writing there are only about a dozen torrents posted, so there isn't a lot to traverse. You can also perform searches by text, tags, and uploader (`@p`).

Switch back to dojo or cli-chat by pressing `ctrl-x`. If you're ready to disconnect your %duiker session, simply unlink in dojo:

{{< termdojo "urbit" "sampel-palnet" >}}
~paldev:duiker> |unlink ~paldev %duiker
 

{{< /termdojo >}}

It's a little laggy, since you're basically connecting to a TTY, but I found it perfectly serviceable every time I've played with it.


## %serval

[%serval](https://github.com/fang-/suite/blob/master/app/serval.hoon) is the backend.  Under the hood, it's an HTTP torrent tracker. Like a traditional private tracker, you're given a private announce URL that's specific to your `@p`, which allows it to keep track of uploaders and up/down ratio per user. It also seems like it would be an easy thing to extend into invitation-only, or hook into other social software you're running with your friends.

`~palfun` also mentions in the announcement post a potential future goal of allowing users to upload and download .torrent files through a web UI, to make it more functional as a proper private tracker. At the moment it works solely with magnet links, which typically don't even require trackers, but use DHT.

[*Jan 2021 update:* A new version of %serval now supports .torrent files -- connecting to %serval in the web dojo allows you to download them.]


## Adding and downloading torrents

If you've ever created a torrent before, none of this will be new -- %duiker will print your private announce URL in the help menu you access with `?`. Use this URL to create a new torrent in your client of choice: 

[![](/img/create-torrent.png)]()

(*e. correction*) Do not mark the torrent private, as others will need to use DHT to download its metadata.

Once you've created your torrent and begun seeding it in your client, right-click it and copy the magnet link. You'll need to use it to craft a nice long `;add` command to punch into %duiker, formatted as `;add magnetLink 'Title' 'description here' tag`, like so:


{{< termdojo "urbit" "sampel-palnet" >}}
~paldev:duiker> ;add magnet:?xt=urn:btih:7221f09e518e4460203222131273bac81fd1caa0&dn=Fine%20Scale%20Modeler%20Magazines&tr=https%3a%2f%2furb.pal.dev%2f%serval%2f~sitful-hatred%2fy0uR4Nn0uNc3uRlH3Re%2fannounce 'Fine Scale Modeler magazines' 'a collection of PDFs related to skills and tips for building & painting scale models' pdf
 

{{< /termdojo >}}

I'd recommend putting this together in a separate window, because scrolling back and forth inside the line buffer can be a bit of a pain. Once it's assembled, just paste it in and hit return. You can press `r` to see it in the list of torrents.

[![](/img/torrent-list.png)]()

From here we can demonstrate downloading as well -- note the far left column, with numbers next to each item. Just enter the number, and it will spit out the description and a magnet link to copy into your client.

[![](/img/torrent-desc.png)]()

---

In the comments of %serval, `~palfun` outlines a few of the technical hurdles that exist before an urbit torrent *client* is feasible (e.g >2GB loom, a vane for raw TCP). This seems like it could be a possibility one day, but maybe not in the immediate future. 

For the moment, with Urbit lacking a proper package manager, so installing this on your ship is a bit more involved than using it on `~paldev` -- I haven't tried it, but I'll update this post with instructions if it's reasonably doable.

What I find most appealing about this project is the prospect of a simple community torrent tracker that you can spin up to go with your group chat; it demonstrates what kind of social software becomes possible when everybody has a server, and points to a new kind of localized internet; and it showcases the power of Urbit as a fully programmable computer that can interface with the rest of the internet. I find this kind of thing very exciting. Anyway, I uploaded a folder of cool French comics by a guy named Lewis Trondheim, you should hop on and grab them.

---

*This post is also available on Urbit, where you can post comments. Join [~matwet/networked-subject](web+urbitgraph://group/~matwet/networked-subject/) and open the Networked Subject notebook.*