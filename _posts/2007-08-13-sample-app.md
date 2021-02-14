---
layout: post
title: "Sample App"
date: 2007-08-13 03:55
author: spencen
comments: true
categories: [Development]
tags: []
---
<div>I'm going to try and use this blog to post details regarding a sample app that I'm writing for myself. The application itself is really more of an applet - I don't really have a fixed feature set for it - it actually forms part of something much bigger that I've been tinkering with for some time.<br><br>The real purpose of this applet and indeed posting it online is so that I can use it as an exercise in learning WPF. I plan to convert all the existing features from the current WinForms version into a new WPF application. This should be a great learning experience for me - although I imagine there will also be a good deal of pain as I struggle to re-learn windowing fundamentals in the new framework.<br><br>Here's a quick feature list that I'm aiming to include before I begin the WPF version:<br></div> 

*   Split frame code out of Photo class into a separate Renderer. Do the same for the PhotoPage class background.  Create a new Photo frame Renderer using simple triangular semi-transparent picture holders. Give the ability to swap between the existing Renderer and this new one.  Make photo frame borders optional.  Create a popup/tool window to display the EXIF data that I'm currently reading.  Add the ability to update/create captions and store in EXIF.  Create a simple "how to use" web page - host online and hook to F1 key.  Create an options window to specify album and application level defaults.  Do a quick optimisation run - particularly in the "load from folder" code path. 

Next post should include some screenshots of the current version and the ClickOnce install path.


