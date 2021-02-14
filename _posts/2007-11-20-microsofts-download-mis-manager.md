---
layout: post
title: "Microsoft's Download Mis-manager"
date: 2007-11-20 13:44
author: spencen
comments: true
categories: [Development]
tags: []
---


Well - its old news - Visual Studio 2008 has hit the MSDN download site. To herald in the occasion some bright spark from Microsoft has decided to ditch the embarrassingly stodgy download manager and replace it with something much more exciting. Now don't get me wrong, the old download manager did its job - it could download a file to a location on your hard disk with minimal interaction. No bells and whistles - and it was obviously written by someone with great skills at low level byte encoding and network data transfer. That's to say by someone that designed butt ugly user interfaces that would have been considered clumsily constructed even back when it was running in Windows 3.1.
 

So enough about the old download manager... what bright vision of the future do Microsoft have in store for us. Well its got some meaningless name - its delivered via a browser delivered ActiveX and a popup window. Those two reasons alone should have you running for the hills. So disable all security checks and let the thing own your system.
 

When/if you actually get it to fire up (I couldn't manage to relax IE's security settings enough on my server so eventually resorted to using my desktop) it prompts you for a download location. If you're using Vista don't be fooled - you have no choice in the matter. Select locations on your hard disk to your hearts content - it will refuse to use all of them and eventually resort to some virtualized folder deep within the Users branch. 
 

That's just the tip of the iceberg. If you want the full run down on this terribly piece of software then I suggest you read <a href="http://richardsbraindump.blogspot.com/2007/11/problems-with-new-msdn-download-manager.html" target="_blank">Richard's post</a> who does a great job of pointing out a few of its many flaws.
 

Oh - and it even uglier that the previous one - I think the door graphic on the Exit button is circa Windows 2.0! I seriously began to consider that this entire thing was a hoax and that the application must be a Trojan - I mean a download manager released by Microsoft that is difficult to install (ok - I guess that's a Microsoft trademark) but then fails to let you pick a location to store a 3.3 Gb file, and would look old-fashioned running on any operating system built in the last two decades!
 

This is was what finally made it so obvious. Someone at Microsoft must have got irked at all the bad criticism aimed at the old download manager - presumably the very bright but user interface design challenged developer that put the thing together. In all that hate mail screaming "use bit torrent", "ClickOnce" etc. someone must have laid down the challenge...
 

> 

**"...could you find an uglier, less featured, more cumbersome download manager anywhere on the planet!".** 


 

And so the seed was planted and no doubt after years of searching that nameless irked Microsoft employee has found (in places no browser should be forced to go) the Akamai Download Manager. 
 

<a href="http://blog.spencen.com/images/83489-72989/Download%20Manager_4.png" target="_blank">![Download Manager](http://blog.spencen.com/images/83489-72989/Download%20Manager_thumb_1.png)</a>


