---
title: "Windows (Home) Server 2008"
date: 2008-06-25 14:47
author: spencen
comments: true
categories: [General, Windows Home Server]
tags: []
---

Last weekend I went down to the [local (geek) hardware store](http://www.allneeds.com.au) and bought myself a 640Gb drive and 2Gb of RAM for my home made Windows Home Server box (for a total of AU$150). After a painless hardware upgrade I then downloaded and installed the latest Virtual Server 2005. I installed the 640Gb drive as a “back-up drive” instead of adding it to the Windows Home Server “disk pool” using the Windows Home Server Power Pack 1 Beta. This gave me two benefits – first it meant that I could then use the drive to actually backup the server shares automatically, and secondly it meant that the drive was not going to be affected by the routine “disk balancing” that Windows Home Server performs on the storage pool.
  

<a href="/images/Windows%20Home%20Server%20Power%20Pack%201.png">![Windows Home Server Power Pack 1](/images/Windows%20Home%20Server%20Power%20Pack%201.png "Windows Home Server Power Pack 1")</a> 
  

So with plenty of spare RAM and disk space I could then easily create a virtual machine instance running Windows Server 2008 with a copy of SQL Server 2008 RC0. This is the first real chance I’ve had to take a look at the new features in SQL Server 2008 (the main reason for the install). Initial thoughts… new DataTypes – nice! Built in Change Tracking in conjunction with ADO.NET Sync Services… funky!


