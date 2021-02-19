---
layout: post
title: "Porting WPF Word Puzzle to Windows Phone Silverlight – Part 1"
date: 2010-04-08 02:15
author: spencen
comments: true
categories: [.NET, Development, Windows Phone, WPF]
tags: []
---


To date I’ve avoided doing any serious development in Silverlight. Every time I’ve tried to tackle it I get so frustrated with all the missing pieces. Besides which I’ve never had a good reason to do any Silverlight work – I’ve never been a fan of applications that run in a browser.
  

With the release of the Windows Phone Series development tools however, I now have a good reason. So I figured I’d pick a relative simple, small scale WPF application that actually makes some sense to run on a mobile device. Rather than starting it from scratch I just wanted to port it from WPF – so I chose the [Word Puzzle program](http://blog.spencen.com/2009/03/12/word-puzzle-v02.aspx) that I wrote a couple of years back. I figured it was a good choice because it met the criteria above, plus I’d already stripped it back a little to make sure it could run as an XBAP application.
  

Inspired by [Rob’s posts on porting NProf to Silverlight](http://devlicio.us/blogs/rob_eisenberg/archive/2010/04/06/porting-nhprof-from-wpf-to-silverlight-day-7.aspx?utm_source=feedburner&amp;utm_medium=feed&amp;utm_campaign=Feed%3A+Devlicious+%28Devlicio.us%29) I thought it may be of some interest to list off the issues that I come up against as I go through the process of porting. This first list represents me starting a new Windows Phone project and copying over classes and XAML files to get *something* to compile and look recognizable. The following represents about 2 hours work:
  

<a href="/images/WordPuzzle_Stage1_4.png">![WordPuzzle_Stage1](/images/WordPuzzle_Stage1_thumb_1.png "WordPuzzle_Stage1")</a> 
  

However, along the way I came across this list of issues:
  

*   No Viewbox
*   No MouseDown or MouseUp
*   No UniformGrid
*   No Image.StretchDirection
*   x:Type is not supported
*   No Style.Triggers
*   No DockPanel
*   No RoutedCommand
*   No KeyGesture
*   No DataType on DataTemplate?
*   No ValueConversion
*   No DefiningGeometry on Shape
*   No BooleanToVisibilityConverter
*   No DynamicResource
*   No WrapPanel  

I haven’t verified the above list yet – save that they gave me compilation errors. I easily found a replacement UniformGrid, but there are a few items on the list that may pose more of a problem.
  

The next step is to get some level of interaction working.


