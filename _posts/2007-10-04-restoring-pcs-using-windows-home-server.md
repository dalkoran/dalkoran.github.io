---title: "Restoring PCs using Windows Home Server"
date: 2007-10-04 13:16
author: spencen
comments: true
categories: [General, Windows Home Server]
tags: []
---
Over the last week I've had an unlucky streak with hardware on my home computers. Firstly my sons aging P3 tower had a hard disk failure. It wasn't catastrophic but it was preventing new data from being written to the drive. So I copied everything off onto the Home Server's shared folders.
 

Then I grabbed an old 6.4 Gb drive (yes they used to make them that small) and installed a vanilla copy of Windows XP. It took about 10 hours and 10 reboots to get this all the way up to Windows XP SP2 with IE 7 and all the latest patches. It also meant the drive had about 400 Mb free.
 

At this point I figure I'll copy across a couple of files from the Home Server only to find its not responding. Turns out that the CPU (an old P4) has fried itself. Bugger.
 

So a trip to the <a href="http://www.allneeds.com.au" target="_blank">local computer shop</a> and I've got an el' cheapo Core2Duo (4500), Asus motherboard (P5B-VM-SE) and 1 Gb of generic RAM. I rip out the old MB and replace with new components hoping and boot the machine. First thing I find is that it doesn't even recognize the CPU. A quick download and flash of the latest motherboard BIOS fixes that, but the machine still won't boot into Windows - reboots as soon as it hits the windows loader.
 

So I spend a few hours re-installing Windows Home Server. This is actually a lot less painful than Windows XP was - it has a neat feature that allows you to re-install the system partition without effecting any of the data backups or shares - very nice. During the installation though I can't help noticing an odd "pfft" sound that comes from my son's P3 which is supposed to be turned off. Turns out that the power supply (cranky old 230 W) has chosen this moment to commit suicide.
 

Next day - off to the shop again to get a new power supply. I opt for the slightly more expensive Thermaltake PSU with a 12cm fan. Theory being larger the fan slower the required rotation and therefore less noise. I was thinking I'd swap with the Home Server since that's going to be running 24/7. However, surprisingly the new PSU was louder than the existing PSU in the Home Server that had two 8cm fans - weird.
 

So now my sons P3 machine has a whopping 550W PSU - major overkill - most of the connectors aren't even compatible with the motherboard.
 

But now the cool bit. 
 

*   I boot up the P3 connect to the Home Server and do a full system backup. Easy - does this very quickly across my 1Gb network (all my machines have 1Gb LAN) and connect to a 1Gb switch with the exception of the Home Theatre PC which is currently wireless only. 
*   Next I power down the P3 - rip out the 6.4 Gb disk with the recently installed Windows XP and replace with a spare 30 Gb disk from my main workstation (I figure it can do with the 4 remaining newer drives). 
*   Then I reboot the P3 using the Windows Home Server Restore CD. This takes a while to load - but when its done it then allows me to follow the prompts and restore the previous system image onto the new drive. It doesn't care that the drive is a different size - as long as its &gt;6 Gb. It even lets me go in and create partitions on the new drive before I do the restore.  

Very nice! Well done Windows Home Server team!!


