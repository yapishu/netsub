---
title: "Building native Windows Urbit binaries"
date: 2021-04-23T18:13:19-05:00
draft: false
toc: false
images:
  - /img/winbin/header.gif
tags: 
  - urbit
  - windows
  - msys2
  - mingw
  - locpyl-tidnyd
---

![](/img/winbin/header.gif)

| **Late 2021 Note**: This guide is now largely outmoded; Urbit now has official Windows binaries, and [Port](https://github.com/urbit/port/releases) now has a native Windows installer |
| --- |

[Last month,](https://github.com/urbit/urbit/pull/4675) a PR was published by `locpyl-tidnyd` to the Urbit Github, containing instructions and a build environment for the compilation of static, Windows-native Urbit executables(!), for the eventual purpose of an official Windows binary distribution. I am frankly amazed that this is even possible.

As somebody who has encouraged many friends to try out a comet, the biggest hurdle has been convincing them to set up a Linux environment -- the source of [two](http://localhost:1313/posts/urbit-wsl2/) previous [tutorials](http://localhost:1313/posts/urbit-windows-docker/) on this blog. WSL2 is a fabulous tool but still veers toward the geekier side of things, and ordinary people cannot be blamed for wanting a simpler method.

This achievement is pulled off using [MSYS2](https://www.msys2.org/), a fork of [Cygwin](https://www.cygwin.com/), and [MinGW](https://sourceforge.net/projects/mingw/), a Windows port of `gcc`. The codebase for `vere` was also ported to `mingw` by `~locpyl-tidnyd`. This tutorial will walk you through the steps necessary to build Windows-native binaries, and execute them in an ordinary command prompt. 

I include a [download link](https://urbits3.ams3.digitaloceanspaces.com/sitful-hatred/urbit-1.3-win.zip) for my compiled binaries in case you have difficulty; however, I **very strongly recommend** that you compile your own, rather than downloading executables from a stranger on the internet! This is a simple process and should only take about ten minutes.

## Installation

Download the [latest installer](https://www.msys2.org/) for MSYS2 from the official website, and install it with all default options. After completion, leave the 'run now' option checked off.

A `wintty` will pop up with a bash shell; let's update all our packages:

{{< terminal "MSYS2">}}
$> pacman -Syu
 
 
{{< /terminal >}}

Once finished, the window will close. Go ahead and launch `MSYS2 MSYS` from the start menu, and run another update, then install our toolkit: 

{{< terminal "MSYS2">}}
$> pacman -Syu
$> pacman -S --needed base-devel mingw-w64-x86_64-toolchain
 
 
{{< /terminal >}}

Now head over to the [linked PR](https://github.com/urbit/urbit/pull/4675) and download the [tarball](https://github.com/locpyl-tidnyd/urbit/archive/refs/heads/mingw-port.zip) for the `mingw` urbit port. Extract the contents, then copy them into your `MSYS2` home directory: 

```
C:\msys64\home\user.name\home\
```

![](/img/winbin/6.png)

## Compiling

Open your start menu, and pull up `MinGW 64-bit` -- note that this is *not* the same as `MSYS2` despite the similar icons.

![](/img/winbin/5.png)

Navigate to your newly copied directory, and run the `configure` script:

{{< terminal "MINGW">}}
$> cd urbit-mingw-port/pkg/urbit
$> ./configure

 
{{< /terminal >}}


Once this has finished running, build it:

{{< terminal "MINGW">}}
$> make build/urbit build/urbit-worker

 
{{< /terminal >}}

Locate and copy the executables out of their directory:

```
C:\msys64\home\user.name\home\urbit-mingw-port\pkg\urbit\build
``` 

## Execution

Open an ordinary `cmd.exe` command prompt, or Windows Terminal if you have it installed -- you do not need elevated privileges, but you may need to allow it through the Windows firewall. 

Navigate to the directory where you have copied your executables, and try executing `urbit.exe` to print the help menu:

{{< terminal "cmd.exe">}}
C:\> cd c:\users\user.name\urbit
C:\Users\user.name\urbit> .\urbit.exe

 
{{< /terminal >}}

If all is successful so far, you're ready to boot a comet.

{{< terminal "cmd.exe">}}
C:\Users\user.name\urbit> .\urbit.exe -c mycomet

 
{{< /terminal >}}


![](/img/winbin/8.png)

Let your ship download its source and compile -- this will probably take 15-20 minutes to complete. Once done, and you're in your dojo, request an OTA update, and produce your Landscape access code:

{{< termdojo "urbit" "~/urbit/sampel_wanzod" >}}
~sampel_wanzod:dojo> |ota ~wanzod %kids
~sampel_wanzod:dojo> +code
 
  
{{< /termdojo >}}

Navigate to [http://localhost:80](http://localhost:80) to access your ship, and paste the password produced by `+code` to enter Landscape. 

## Addendum

The process of downloading and compiling your ship is significantly slower than on a Linux machine in my experience, even compared to running a ship on WSL2. That said, it was still totally functional if slightly less responsive.

While I have successfully tested and used this Windows build, I cannot guarantee their stability -- these have not been released into production, I assume for this reason. I would not recommend booting a planet with this just yet, but this should be replaced by an official release in the near future.