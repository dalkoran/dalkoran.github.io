---
layout: post
title: "WHS Remote Access"
date: 2007-10-17 14:31
author: spencen
comments: true
categories: [General, Windows Home Server]
tags: []
---


One of the features of Windows Home Server (WHS) that I really like is the ability to use the server as a remote access "hub". It means I can remote desktop into any one of my home machines from anywhere on the planet. This is kinda cool and can be used to:



*   Fire off disk defrags, backups, video renderings (try rendering an hours worth of HD multi-track video with effects! - even on my own mini render-farm it takes forever).
Use WebGuide to setup my TV recording schedule (of course if I allowed external access to WebGuide through HTTP I wouldn't need WHS for this).
Do some coding on VS 2008 Beta 2 via my home workstation to try out some stuff. Handy when I don't want to install the Beta on my work laptop but need to do a quick proof on concept.


But for this to work it means I need to keep all my computers running 24 hours a day right? Nope - WOL to the rescue - that's "wake on lan" which has been sitting there on all my BIOS's for years now but I've never used it!



So how does it work - well in picture format it goes something like this:



Login to my WHS box using [https://{myUniqueName}.homeserver.com](https://{myuniquename}.homeserver.com/) .



<a href="/images/Home%20Tab.png" target=_blank atomicselection="true">![Home Tab](/images/Home%20Tab.png)</a> 



From here I can access all my Shared Folder content - kinda neat for uploading images whilst on holiday etc. to make sure they're all backed up nicely as part of my home-wide backup policy.



Clicking on the Computers Tab shows a list of all the computers configured in my current home setup so far (I haven't gotten to all of them yet).



<a href="/images/Computers%20Tab.png" target=_blank atomicselection="true">![Computers Tab](/images/Computers%20Tab.png)</a> 



From this we can see that of the three machines only the Media Center PC is currently running (as per the WHS box it also runs 24/7). The WHS box itself isn't listed below since but we can click on the "Connect to your Home Server" to bring up the WHS dashboard.



<a href="/images/Dashboard%20Backup%20Tab.png" target=_blank atomicselection="true">![Dashboard Backup Tab](/images/Dashboard%20Backup%20Tab.png)</a> 



The WHS dashboard actually runs as a windowed remote terminal *application*. This is kinda weird and takes some getting used to - seeing pop windows rendered in the WHS host theme rather than the local theme. The default page shows the backup status for each machine.



Clicking on the Wake On Lan tab gives us the following screen which shows which of the machines are online (switched on and connected to the network)&nbsp;together with their MAC and IP details.



<a href="/images/Dashboard%20WakeOnLan%20tab.png" target=_blank atomicselection="true">![Dashboard WakeOnLan tab](/images/Dashboard%20WakeOnLan%20tab.png)</a> 



So from here we click on the machine that we wish to start and then click on the "Wake Up" button. At this point we logout of the WHS dashboard and go make a cup of coffee whilst that machine boots up.



<a href="/images/Computers%20Tab%202.png" target=_blank atomicselection="true">![Computers Tab 2](/images/Computers%20Tab%202.png)</a> 



After a few minutes we see that the machine is now available for connection. Woohoo! Simply click on the "available" machine and we're away with a full-screen (optional) remote desktop connection. I don't get glass - but hey I can live with that ![](http://blog.spencen.com/emoticons/smile.png)



![Login src=](/images/Login.png) 



I've tried the remote desktop from a few locations and it works surprisingly well even though I've only got a standard ADSL connection at home with a very limited 256k upload speed.



This is probably a good time to mention that the Wake On Lan feature does not come out-of-the-box with WHS. Its one of the cool Add-Ins that are floating around on the net - this particular one by Evangelos Hadjichristodoulou. I've played with the <a href="http://msdn2.microsoft.com/en-us/library/aa496121.aspx" target=_blank>WHS SDK</a> myself and its pretty cool - certainly very easy to make a simple add-in.


