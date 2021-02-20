---
title: "Developing for Windows Mobile – Getting Started"
date: 2009-07-28 14:38
author: spencen
comments: true
categories: [.NET, Development]
tags: []
---

Now I have my new Windows Mobile 6.1 device I’ve decided to delve once more into the mysterious realm of developing for mobile devices. My previous forays have both been very lightweight. The most recent was simply displaying bus timetable information within a ListView control. Despite its simplicity its actually something I find very useful on my daily commute – rather than digging around in my bag for the paper version and then tuning in to the times and stops that I’m interested in.
  

Here’s a screenshot of my original Bus Timetable applet (its just too trivial to be awarded the title “application”):
  

[UIFramework](http://code.msdn.microsoft.com/uiframework) from MSDN I managed to get some screens that I was pretty happy with. The icons and selection had gradient fills and the icons were superimposed on their corresponding “frames” with a transparent background.
  

[WinForms Transitions](http://blog.spencen.com/2007/12/11/winforms-animation-part-2.aspx) code to the Windows CF. Even this was a no-go though – because although it contains plenty of base classes and helper methods for transforming and hit testing objects it relies heavily on using GraphicsContainer to do the actual transforms. These aren’t supported by the Compact Framework. So, I started from scratch, building a simple graphics library that will let me scale, rotate and translate graphic primitives. So far I have a spinning/zoomable rectangle – guess you gotta start somewhere.
  

![MobileTransitions](/images/MobileTransitions_1.png "MobileTransitions")


