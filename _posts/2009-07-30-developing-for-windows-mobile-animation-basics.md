---
layout: post
title: "Developing for Windows Mobile – Animation Basics"
date: 2009-07-30 15:57
author: spencen
comments: true
categories: [.NET, Development]
tags: []
---


This post is a follow on from my <a href="http://blog.spencen.com/2009/07/29/developing-for-windows-mobile-ndash-getting-started.aspx" target="_blank">most recent efforts</a> creating some screens for a Windows Mobile application. I had decided to write a simple graphics library to help me do some nice transition effects. So over the last two nights I ported (actually rewrote) my original WinForms Transition demo to Windows Compact Framework.
  

So far I have support only for polygons, but that includes hit testing, transforms (rotate, translate, scale), animation (floats, points, colours, brushes) and easing functions (currently sine in/out and elastic out).
  

Hopefully I’ll post more about this (with source code) soon – but <a href="http://www.spencen.com/Downloads/winmo_animation_test.wmv" target="_blank">here</a> is a quick demo video that I captured using Expression 3’s new screen capture utility (very neat!).
  

<a href="http://www.spencen.com/Downloads/winmo_animation_test.wmv" target="_blank">![Windows Mobile Animations_Thumb](http://blog.spencen.com/images/83489-72989/Windows%20Mobile%20Animations_Thumb_3.jpg "Windows Mobile Animations_Thumb")</a> 
  

So far I’ve been really happy with the performance. Will be interesting to see how much it degrades once I start adding gradients, transparency and more complex shapes. [Note that the video doesn’t really reflect the performance too well.]


