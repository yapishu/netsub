---
title: "Easy Urbit TLS with Caddy"
date: 2021-01-26T19:22:55-06:00
draft: false
toc: false
images:
  - /img/caddyheader.gif
tags: 
  - caddy
  - caddyserver
  - urbit
  - tls
  - letsencrypt
  - dns
  - digitalocean
---

![](/img/caddyheader.gif)

One of the first tutorials written for this site was a [rundown](/posts/urbit-nginx-letsencrypt/) on setting up an https reverse proxy for your Urbit using Nginx and Letsencrypt. Nginx and LE are fantastic pieces of software, but steps involved may seem a little intimidating if you've never set up a web server before. 

In the last couple of weeks, a few people around the network (thanks `~silsyn-wathep`!) have suggested using [Caddy](https://caddyserver.com/) (alt. Caddyserver), a very lightweight and fast HTTP server suite. A big bonus with Caddy is that it handles all the TLS configuration in the background -- you don't have to set up Letsencrypt manually.

I've been trying it out, and I love it; it's a huge timesaver when I'm throwing together a ship on a server. I would strongly recommend it, and will probably move towards using it exclusively in the future. This process should only take you about fifteen minutes total, and once you've done it once, even less than that in the future.

This tutorial assumes you are using Ubuntu. If you are using another distro or OS, the instructions should be the same, minus the installation of Caddy. You can consult the official installation documentation [here](https://caddyserver.com/docs/install).

## Preparation

In order to make use of TLS, you will need a domain name to point at your server. Go to your registrar or host and create an `A` record pointing at the IP address of the server or PC you want to use -- for example, create an `A` record for `sampel-palnet.mydomain.io`, which directs to `1.2.3.4`. This should propagate quickly if the primary domain is not brand-new.

If you are using a home PC like me, go ahead and set up [port forwarding](https://www.noip.com/support/knowledgebase/general-port-forwarding-guide/) rules for ports 80 and 443. You will need to set your router to forward those ports to the computer hosting your ship -- assign a static private IP to that computer as well if you haven't already.

If you are using a hosted server, like [DigitalOcean](/posts/urbit-nginx-letsencrypt/) or [Oracle Cloud](/posts/free-cloud-oracle/), make sure your firewall has ingress rules for ports 80 and 443.

Go ahead and [launch your ship](https://urbit.org/using/install/) if you haven't already. Do not use the `setcap` command that gives it low port privileges -- we want it to bind to port 8080 by default. Navigate to `your.ip.address.here:8080` in your browser and make sure you can reach your ship. 

## Installation

Let's proceed; you will need to add the Caddy repos to your computer so you can install it and update it automatically in the future. Enter the following commands -- the first installs requisite software if you don't have it already, the second imports the key for the software repository, and the third adds the repository itself to your repo list.

{{< terminal "terminal" "~/urbit" >}}
$> sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
$> curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
$> curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list

 
{{< /terminal >}}

Now let's install Caddy and give it port 80 privileges:

{{< terminal "terminal" "~/urbit" >}}
$> sudo apt update && sudo apt install caddy
$> sudo setcap 'cap_net_bind_service=+ep' /usr/bin/caddy
 
 
{{< /terminal >}}

## Configuration

Configuration is where Caddy really shines; if you've seen the walls of text in the Nginx tutorial, this will be a breath of fresh air. 

Nagivate to your home directory and create a text file called `Caddyfile`:

{{< terminal "terminal" "~/urbit" >}}
$> cd
$> nano Caddyfile
 
 
{{< /terminal >}}

In `nano`, enter the following two lines, adjusting for your desired domain:

```
sampel-palnet.mydomain.io
reverse_proxy 127.0.0.1:8080
```

That's literally all you have to do -- how cool is that? Press `ctrl-o`, `ctrl-x` to save and exit. 


## Execution

Now let's run the server and make sure it works. There are two commands that will launch Caddy -- `caddy run` will launch it in the foreground. We will do this for the first go-round:

{{< terminal "terminal" "~/urbit" >}}
$> caddy run
 
 
{{< /terminal >}}

Caddy should automatically generate TLS certificates using Letsencrypt, and once it has successfully tested the connection, it will be up and running!

Press `ctrl-c` to stop the Caddy process. Let's run it again, this time in the background: 

{{< terminal "terminal" "~/urbit" >}}
$> caddy start --config ~/Caddyfile
 
 
{{< /terminal >}}

If you restart Caddy in the future, you will need to use this command to point Caddy at the configuration file you generated. You can turn off Caddy at any time with `caddy stop`. 

If you're comfortable with it, you can also add a `systemd` service to [automatically run Caddy](https://github.com/caddyserver/dist/tree/master/init). Make sure the user running the service can read your `Caddyfile`!

One last note -- if you're using Cloudflare, make sure you set your TLS on Cloudflare to "Full (strict)", or your site will not be connectable. This is an issue with Caddy that manifests as "too many redirect" errors, but it's a simple fix.