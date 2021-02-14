---
layout: post
title: "Windows Mobile SDK disables Sierra 880U NextG Wireless Card"
date: 2009-07-13 13:45
author: spencen
comments: true
categories: [.NET, Development]
tags: []
---


On my 50 minute bus trip home I often get out my laptop – whether its to finishing checking-in my days work via VPN, getting a headstart with projects that I’m working on at home, of even just catching up with blogs/mail etc.
  

Tonight I fired up the laptop, inserted my [Telstra Wireless NextG card](http://blog.spencen.com/2008/08/29/wireless-internet.aspx) and… nothing. Just the message “Card not detected”. Tried both other USB ports no luck. Opened up device manager and there it was – exclamation marks against all the Network Adapters and Ports associated with my Sierra Aircard 880U. Not good!
  

I tried all the usual, time honoured, general purposes fix measures:
  

*   Reboot the machine
*   Disable and re-enable the devices
*   Un-install the devices and do a hardware scan to re-install them  

No luck. When I got home I removed the drivers and tried to re-install from the original installation disk that came with the device. The installer failed right after selecting the modem device. So I re-installed the driver manually from the CD. The installation worked but still the device wasn’t detected.
  

Time to backtrack – it was working flawlessly last week. Once I’d got to this point the culprit became obvious. I had installed Windows Mobile 6.0 and 6.5 Developer Tools on the weekend so I could start doing development for my [new phone](http://blog.spencen.com/2009/06/24/htc-touch-diamond2.aspx) on my laptop. It made a quirky kind of sense that this installation could have somehow installed some virtual drivers that were interfering with the real HDSPA modem.
  

Sure enough. After uninstalling Windows Mobile 6.0 Developer Tools and Windows Mobile 6.5 Emulators then re-inserting the modem it worked immediately. Great! But how do I get to have a working NextG Wireless Card *and* developer for a Windows Mobile device!? Is this just some quirky hardware issue on my laptop/install?


