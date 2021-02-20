---
title: "Smugmug API"
date: 2011-03-21 01:19
author: spencen
comments: true
categories: [.NET, Development]
tags: []
---

[Smugmug](http://www.smugmug.com). I can upload and view images at their original size, they offer effectively unlimited storage, support video streaming, protected sharing facilities and the website doesn’t suck. It comes with a price tag but I’m fine with that.
 

Over the last six months I’ve been playing with the Smugmug API. To date its been a somewhat painful experience. Roughly three months ago they introduced an additional security measure that meant most of the API calls required an additional parameter – which wasn’t documented on the [API wiki](http://wiki.smugmug.net/display/API/API+1.2.2) (the _su cookie).
 

[New York Windows Phone User Group](http://nypug.groups.live.com/) I switched to [RestSharp](http://restsharp.org/) (using VS NuGet package installer) which made the code much more concise.
 

My most recent attempt was to add a feature to the application I’m writing that would allow me to view statistics for my Smugmug galleries. The website offers access to these statistics but in a pretty limited fashion. Having found an API ([method.albums.getStats](http://wiki.smugmug.net/display/API/show+1.2.2?method=smugmug.albums.getStats)) I figured I could create a stats summary that was exactly was I was after. After several frustrating hours of tweaking the API call and getting nothing but zero hit counters back I resorted to Google. Sure enough – it seems that the API method [doesn’t work](http://www.dgrin.com/showthread.php?t=169830), and in fact has been broken for months. Would be nice to have that kind of information on the API Wiki page, no?


