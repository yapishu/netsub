---
title: "Planetary drive"
date: 2020-08-30T14:28:42-05:00
draft: false
toc: false
images:
  - /img/pavepaws.png
tags: 
  - urbit
  - luks
  - encryption
---

[![](/img/pavepaws.png)]()

Urbit is a manifestation of the cypherpunk ethos -- your communication is relayed through a decentralized network, with all traffic encrypted all the time. Well, most of the time; it's not encrypted at rest, meaning physical access to your device means full access to its history. There's no technical reason that it must remain this way, though, and I've decided to run my pier from an encrypted disk.

For about the last year or so, I've run my urbit planet `~sitful-hatred` on a Digital Ocean VPS. However, I decided that I would like to have my planet on a device in my physical possession. I have a sentimental and practical desire to keep my important data secure and at-hand; I also had a spare SSD collecting dust inside my desktop, and this was the first good idea I had for making use of it -- encrypting a bunch of books and music didn't seem as cool.

I think decryption at rest by default is a sane default for something that aspires to be a major platform -- leaving your phone's data on Apple's servers is [insecure](https://www.engadget.com/2020-01-21-apple-dropped-plan-for-encrypted-icloud-backups.html), which gets the feds off their back (to some degree). This can of course be trivially sidestepped, by setting a real passcode to unlock your phone and disabling cloud backups, but nobody does this. Decryption at rest for your ship by default means only the technically-inclined or adventurous of spirit will bother to secure their pier; but it also allows for easier recovery of data if something goes wrong, or a password is forgotten. It probably is best for most people, most of the time.

But encryption is a blanket. Encryption is a moat between you and the world. Encryption is how you take care of what's important and personal. 

So, to cut a long introduction short, the following is a tutorial in running your urbit from an encrypted drive or partition on Ubuntu. The steps taken to encrypt the drive are shamelessly pillaged from [here](https://davidyat.es/2015/04/03/encrypting-a-second-hard-drive-on-ubuntu-14-10-post-install/) -- total credit to Mr Yates, because worked out the hard part.

One last word of warning: messing with partitions and disk formatting is very easy to screw up! If you don't understand what is happening when you follow these steps, it may be better to skip this one, though you probably won't do anything unrecoverable if you screw up -- just double and triple check that you're performing operations on the correct device.

---

### 1. Clear the partitions

Before you can encrypt the drive with LUKS, you'll need to delete the partitions on it. Using gparted, select the device you're using in the `Gparted-> Devices` menu. It will display a graphical representation of the drive, along with any existing partitions. Delete all partitions, create an unformatted partition of the desired size, make a double check that you're doing this to the correct device, and hit apply.

You will not be formatting the drive quite yet. Do not mount it, or interact with the device outside of these instructions until you are finished.

### 2. Setting up LUKS

** Replace `sd?X` with the name of your device (e.g, `/dev/sdc`). You can identify the device name in Gparted, or by running `lsblk`.**

Now we will run `cryptsetup` to set up the device's encryption:

{{< terminal "terminal" "~" >}}
$> sudo cryptsetup -y -v luksFormat /dev/sd?X
 

{{< /terminal >}}

Enter a strong password and write it down. You won't need to remember it so you can go the extra mile with complexity, but at some point in the future you will probably need to recover it. 

Next, perform the following operations:

{{< terminal "terminal" "~" >}}
$> sudo cryptsetup luksOpen /dev/sd?X sd?X_crypt
$> sudo mkfs.ext4 /dev/mapper/sd?X_crypt
 

{{< /terminal >}}

This will decrypt the partition, and format it in [ext4](https://www.wikiwand.com/en/Ext4). Now, it's ready to mount:

{{< terminal "terminal" "~" >}}
$> sudo mount /dev/mapper/sd?X_crypt /<mount-point>
 

{{< /terminal >}}

Your mount point can be arbitrary, but on Ubuntu is usually a directory inside `/media/` -- mine is `/media/Data/`. You can interact with your newly formatted drive in this folder.

### 4. Keyfile & auto-mount

A cool trick I didn't know about until reading the previously-linked tutorial is that you can make your machine automatically mount a LUKS drive using a keyfile. In order to do so, we'll generate a file of random noise and add it as a key for the drive:

{{< terminal "terminal" "~" >}}
$> sudo dd if=/dev/urandom of=/root/.keyfile bs=1024 count=4
$> sudo chmod 0400 /root/.keyfile
$> sudo cryptsetup luksAddKey /dev/sd?X /root/.keyfile
 

{{< /terminal >}}

Next we'll need to tell the computer to mount the drive using the keyfile automatically. We'll need the device UUID (a unique identifier for the hardware), which we can find by looking at the output of:

{{< terminal "terminal" "~" >}}
$> sudo blkid
 

{{< /terminal >}}

Look for the device you're working with, find where it says `UUID="longstringofnumbersandletters..."` and copy the part between quotation marks. Next, use it to edit this command, replacing `<device UUID>`: 

```
  sd?X_crypt UUID=<device UUID> /root/.keyfile luks,discard
```

Make sure you're looking at the UUID of the device (e.g, `/dev/sdc`, **not** `/dev/sdc_crypt` or `/dev/sdc1/`).

Now we'll add the mount instructions to fstab: 

{{< terminal "terminal" "~" >}}
$> sudo nano /etc/fstab
$> /dev/mapper/sd?X_crypt  /<mount-point>   ext4    defaults        0       2
 

{{< /terminal >}}

Be sure to replace `/<mount-point>` with your preferred mount location (e.g, `/media/Data`). 

Now to make sure that your device has the appropriated permissions, run the following command with your username and mount point replacing the original:

{{< terminal "terminal" "~" >}}
$> sudo chown <user>:<user> /<mount-point> -R
 

{{< /terminal >}}

Now restart your device to test whether it successfully mounts on reboot. Voila, a fully encrypted disk you don't have to remember the password for!

### 6. Moving your pier

This is the easy part. I moved my urbit from a remote server, so all I had to do was shutdown the process on the server and rsync the pier folder into my new drive: 

{{< terminal "terminal" "~" >}}
$> rsync -av reid@subject.network:/~/sitful-hatred /media/Data
 

{{< /terminal >}}

Urbit piers are completely portable; as long as you have the static binaries on another device and it's not trying to run in two places at once, you can simply pick up your data and move it with a copy operation. This is one of the reasons I wanted to devote a disk to my ship -- I can swap it in and out of devices I may go through in the future, but maintain a physical token of my digital homestead. One day we'll all have e-ink mesh-networked urbit devices that run on solar panels and Optane drives, but for now I have my `+code` and an SSD.

---

*This post is also available on Urbit, where you can post comments. Join [~matwet/networked-subject](web+urbitgraph://group/~matwet/networked-subject/) and open the Networked Subject notebook.*