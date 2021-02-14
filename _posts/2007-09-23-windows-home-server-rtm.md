---
layout: post
title: "Windows Home Server RTM"
date: 2007-09-23 13:06
author: spencen
comments: true
categories: [General, Windows Home Server]
tags: []
---


Just setup my Windows Home Server machine to the RTM version (from RC). Performed a manual backup using a single drive (320Gb) which does an amazing job of backing up my main workstation and the home theatre PC [this isn't a full backup given the workstation alone has over a terabyte of disk - but its most of the important stuff]. However, I then included the second drive into the storage "array" and the backups started failing again. This was the same problem I had with the RC - basically the server machine just reboots halfway through a backup. It seems like a hardware issue given how sudden the reboot is (there is nothing untoward reported in any of the event logs) - but as to which device is failing...
 

The most obvious candidate would be the second hard drive - a 400Gb Seagate drive. Thing is I've run all of Seagate's own diagnostics over this drive (SeaTools) and it comes up fine every time. Maybe its the SATA controller - but that's an on-board device which also is operating the primary drive. Maybe its the Intel gigabit on-board LAN controller - but why does it only fail when the second drive is operational. The Seagate drive is marginally faster than the Western Digital - but both drives are less than 6 months old. The S.M.A.R.T. diagnostics for both drives also look pretty normal - with both showing near-perfect health.
 

Another annoying problem is that I can't seem to install the Windows Home Server Connector software on my sons PC. It was all working fine under the RC but for some reason refuses to un-install correctly and hence can't install the update. Frustrating - I was about to resort to some sysinternals tools to track exactly where it was getting stuck - but figured its probably time to just rebuild the machine anyway given its a very old box whose OS has probably hasn't been re-installed for 6 years or so. Was thinking of upgrading it from XP SP2 to Vista but my 4 ½ year old son has some nostalgic feelings towards XP and isn't ready for the leap (stumble?) to Vista.
 

As for the Windows Home Server box itself... dunno - I checked out some motherboard and PSU pricing tonight. Its currently running a crusty old P4 2.8GHz and given its going to be running 24 hours a day I figured I might opt for a more energy efficient choice with a silent PSU to boot. The home theatre PC is silent from any distance greater than 30cm and uses a CoreDuo mobile chip. Seems like a Home Server box should probably be running the same sort of hardware... which leads me to wonder why I need both a Home Server and Home Theatre PC - both of which run 24/7. Oh well - I think its a common sentiment that hopefully the Home Server team will address in the next version.
 

______________
 

Slowly but steadily working my way through the WPF book. There are some huge similarities between a lot of what's in WPF and that I've been working on (in my day job) over the last 3 years. This isn't surprising given that we based a lot of our work on some early XAML samples from Microsoft (and others). I'm already imagining how it would be possible to build an infrastructure framework over the top of WPF to more easily create business applications. So should I dig into Acropolis or just go ahead and build my own?


