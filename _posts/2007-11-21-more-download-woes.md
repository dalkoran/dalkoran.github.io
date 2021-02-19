---
layout: post
title: "More Download Woes"
date: 2007-11-21 07:10
author: spencen
comments: true
categories: [General]
tags: []
---


So <a href="http://blog.spencen.com/2007/11/20/microsofts-download-mismanager.aspx" target="_blank">last night</a> the whole Download Manager thing was annoying and somewhat astonishing in terms of Microsoft being so relaxed about delivering such&nbsp;a dismal download experience for their enormous&nbsp;MSDN subscriber base.
 

This morning things have gone past annoying - I'm now pissed off. What are the two critical functions of a download manager?
 

1.  The ability to recover from intermittent network failure with minimal data loss.  The ability to select a download location for the file(s). 

So I can now accurately report that the Akamai Download Manager meets neither of these requirements. According to my logs I had a minor network glitch on my ADSL connection last night lasting a few minutes. The download manager recognized this outage - displayed a verbose and unhelpful message and then failed to resume. It gets even better though - I responded to all prompts in an effort to resume the download with no avail - it even deleted the 3 Gb file that it had downloaded (93%). I mean just check out the <a href="http://msdn2.microsoft.com/en-us/subscriptions/bb153537.aspx#install" target="_blank">frequently asked questions</a> page for another idea of how bad this software is.
 

So it actually turns out that the Akamai Download Manager is&nbsp;as reliable as right-click "Save As..." but with the limitation that unlike "Save As..." you have no option about where the file will be located on your machine.
 

So how is this a Download Manager - how is it helping anybody? I've used 3 Gb of my quota for nothing - Microsoft will have me try again - so they've also wasted 3 Gb, it delays me getting the RTM version so I get more frustrated by the Beta 2 instability (working with Cider).
 

Has anyone had a successful download under Vista?
 

[Just for the record I run Vista as an Administrator, UAC turned off and no Virus Checker.]
 

**UPDATE**: 17:29
 

Hah! Works sooo much better under Windows XP. You can actually choose the location of your download file! Of course the resume still doesn't work so there's little chance of ever actually completing a download...
 

![Akami Download Manager Failure](/images/Akami%20Download%20Manager%20Failure.png) 
 

I wonder who at Microsoft I should be contacting?
 

![Akami Download Manager Do You Want To Keep](/images/Akami%20Download%20Manager%20Do%20You%20Want%20To%20Keep_1.png) 
 

Don't be fooled by that friendly looking warning sign - this is a trick question. It doesn't matter whether you pick "Yes" or "No" your download is doomed. There is no ability to resume an interrupted download.


