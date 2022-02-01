---
title: "Self-hosting MinIO S3 for Urbit"
date: 2021-01-14T17:32:26-06:00
draft: false
toc: false
images:
images:
  - /img/minio/minio-urbit.png
tags: 
  - urbit
  - minio
  - s3
  - free
---

![](/img/minio/minio-urbit.png)

## Background

This tutorial will walk you through the process of configuring MinIO on your server, which will allow you to host images and content for your ship, but without relying on or paying for a third party host.

S3 is a service offered by Amazon's web service division that allows you to cheaply host data that you can programatically access with other applications. S3 is wildly popular, and as a result has an API that is widely supported by a lot of software. Other companies also offer services that can use the same API as Amazon's service.

Urbit has support for S3-compatible APIs -- we [previously covered](/posts/configuring-s3-urbit/) how to use DigitalOcean's S3 service, Spaces. Think of this as sticking a big external hard drive onto your ship, that allows you to upload images and data from inside of Landscape, the web UI.

MinIO is a highly performant, S3-compatible object storage application, with both client and server binaries. Most importantly, it's open source software that you can run yourself on the server hosting your ship, and you don't have to pay additional fees to DO or Amazon. This tutorial will assume you are using Ubuntu, but the instructions should not deviate much across OSes. 

An important qualification: S3 requires a subdomain that matches your bucket name, and Urbit's implementation requires https, so you will need to have a domain to generate certificates for -- fortunately, if you're using a planet, you can use a free arvo.network subdomain. If you have your own domain, I believe you can substitute 'sampel-palnet' throughout this tutorial with a subdomain of your choice, though I wasn't able to fully test this due to my particular setup.

All credit is due [once again](/posts/free-cloud-oracle/) to `~socsel-pacted` for the heads-up, and for figuring this out!

*Edit:* You can find alternate instructions by `~tomnyr-tirsyr` [here](https://www.notion.so/MinIO-S3-Urbit-a18a584293b3459e8785339e60145b11).

## Server prep

As I mentioned, you'll need to have your planet set up with a domain for https -- if you haven't already done so, you can register your ship's IP on arvo.network with the following command in dojo (replacing 1.2.3.4 with your IP -- make sure you have a period in front of it): 

{{< termdojo "urbit" "sampel-palnet" >}}
~sampel-palnet:dojo> -dns-address &dns-address [%if .1.2.3.4]
 
  
{{< /termdojo >}}

(In this and all tutorials, `dojo>` indicates your urbit's command line, and `$>` denotes your computer or server's).

Unfortunately, it may take a few days for this to go into effect if you don't already have this set up -- registering these subdomains is a manual process as far as I know, running the command just registers a request. You'll be able to tell it has worked when navigating to `sampel-palnet.arvo.network` (with your planet name) pulls up your ship.

While we're setting up, make sure port 9000 is accessible. On your server, you can run:

{{< terminal "terminal" "~" >}}
$> sudo ufw allow 9000/tcp
 

{{< /terminal >}}

If you're using an Oracle VPS, create a TCP [ingress rule](/posts/free-cloud-oracle/#configuring-software) that allows port 9000.

## Installation & TLS

Once you've gotten everything prepared, let's run certbot to generate a TLS certificate. First, shut down your urbit or any running web server to free up port 80, then install and run `certbot`:

{{< terminal "terminal" "~" >}}
$> sudo apt install certbot
$> sudo certbot certonly --standalone
 

{{< /terminal >}}

Follow the instructions to generate your certificates; when prompted, enter `sampel-palnet.arvo.network`, substituting your ship's name. Once completed, you can find them in `/etc/letsencrypt/live/sampel-palnet.arvo.network`.

Now, let's download and run Minio so we can manipulate its configuration:

{{< terminal "terminal" "~" >}}
$> cd
$> wget https://dl.min.io/server/minio/release/linux-amd64/minio
$> chmod +x minio
$> ./minio
 

{{< /terminal >}}

This should print the help menu. Now we can copy our certificates into the appropriate folders: 

{{< terminal "terminal" "~" >}}
$> mkdir ~/.minio && mkdir ~/.minio/certs
$> sudo cp /etc/letsencrypt/live/sampel-palnet.arvo.network/fullchain.pem ~/.minio/certs/public.crt
$> sudo cp /etc/letsencrypt/live/sampel-palnet.arvo.network/privkey.pem ~/.minio/certs/private.key
$> sudo chown user:user .minio/certs/public.crt
$> sudo chown user:user .minio/certs/private.key
 

{{< /terminal >}}

(replace `user:user` with your username on this device -- i.e. `jsmith:jsmith`).

Now let's set environment variables for Minio's access and secret keys (basically username/password). First, let's generate a nice strong password:

{{< terminal "terminal" "~" >}}
$> openssl rand -base64 18
 

{{< /terminal >}}

This will spit out a long random string. Copy it and paste it in the first following command, and set the second one to whatever you'd like: 

{{< terminal "terminal" "~" >}}
$> export MINIO_SECRET_KEY=yourlongrandomstringhere
$> export MINIO_ACCESS_KEY=loginname
 

{{< /terminal >}}

Similarly, let's go ahead and set the root login the same way:

{{< terminal "terminal" "~" >}}
$> export MINIO_ROOT_USER=your_root_username 
$> export MINIO_ROOT_PASSWORD=anotherstrongpassword 
 

{{< /terminal >}}

Think of this as writing down your credentials on a post-it note for your programs to look at. Now let's make a folder for our minio to put data in:

{{< terminal "terminal" "~" >}}
$> mkdir minio-data/sampel-palnet
$> ./minio server minio-data/sampel-palnet
 

{{< /terminal >}}

And finally, launch minio (after spawning a tmux window to hide it in):

{{< terminal "terminal" "~" >}}
$> tmux new -s minio
$> ./minio server ~/minio-data/sampel-palnet
 

{{< /terminal >}}

[You may recall this trick for hiding your urbit's command line -- you can hide this window by pressing `ctrl-b` followed by `d`; this will leave it running in the background. You can bring it back to the fore by entering: `$> tmux a -t minio`]

If it worked, you should see something like this:

![](/img/minio/minio2.png)

Note the certificate information at the bottom; this means it is serving over https.

### Bucket configuration

Let's open up the control panel: 

`https://sampel-palnet.arvo.network:9000`

Login with the access and secret keys you set previously. Once logged in, click the `+` icon at the bottom right to create a new bucket. The bucket name has to be the same as your subdomain (i.e, `sampel-palnet`):

![](/img/minio/minio3.png)

Now, in the left sidebar, click the icon on the right to edit the access policy. Enter `*` in the text field and select 'read & write'.

![](/img/minio/minio4.jpg)

![](/img/minio/minio8.jpeg)



## Urbit configuration

Finally, lets return to our ship. Go to Landscape and click your name at the top right, then go to 'ship settings' in the left sidebar. Scroll down to 'S3 credentials'.

![](/img/minio/minio6.png)

Enter `arvo.network:9000` as your endpoint (*not* your subdomain), and enter the credentials we set earlier. Beneath, under 'S3 buckets', enter `sampel-palnet` -- if you are using a personal domain rather than arvo.network, note that this must match the subdomain.

![](/img/minio/minio7.jpeg)

Click save, go to `Options -> Activate` next to the name of the bucket, and you have completed this tutorial! Head over to a chat and look in the text field you type into -- there should be a paperclip icon on the right. Click it to add attachments.


---


I hope you found this useful; thanks again to `~socsel` for the assistance. If you run into any difficulties, feel free to reach out in the chat room at `~matwet/networked-subject`.
