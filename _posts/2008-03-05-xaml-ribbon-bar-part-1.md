---
layout: post
title: "XAML Ribbon Bar - Part 1"
date: 2008-03-05 12:28
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


I've been trying to dream up a functional but elegant user interface for my mystery app. At work I've been using the DevExpress WinForms control suite and I have to say I've been very impressed. Their ribbon bar in particular is pretty swish and seems to be very popular at the moment.
 

I'd already gone down the path of having a tab control to separate the functional modes in which the application operates. Keeping the UI relatively simple by only giving access to the functions that meet the current mode of operation. But my "out of the box" WPF tab control was looking pretty dull. 
 

So... how to convert the standard WPF controls into something more elegant - something looking a little like a Ribbon Bar perhaps? Well, I must say I had a blast doing this - and finally ended up with a prototype that managed to get reasonably close without having to use anything but XAML. All the magic is done simply using the standard WPF controls (TabControl, ToolBarTray, ToolBar, Button etc.) and some styles and templating.
 

<a href="http://www.spencen.com/Downloads/RibbonBar.xaml" target="_blank">![XAML Ribbon Bar](/images/XAML%20Ribbon%20Bar_1.png)</a> 
 

You can run this XAML file (click <a href="http://www.spencen.com/Downloads/RibbonBar.xaml" target="_blank">here</a> or on image above) to see how far I got with this approach. I tried hard to get the curvy tab headers and to remove the line between the tab and the page but couldn't come up with a solution in XAML alone that didn't involve fixing the tab header width. I took the button images out so the file could run stand-alone.
 

Once all the styles are defined the code to actually mark up the Ribbon Bar is identical to marking up a TabControl with embedded Toolbars - something like:


       <span style="color: rgb(139,0,139)">&lt;TabControl </span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="tabControl1" </span><span style="color: rgb(255,0,0)">SelectedIndex</span><span style="color: rgb(0,0,255)">="3"</span><span style="color: rgb(139,0,139)">&gt;
&lt;TabItem </span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="tabDream" </span><span style="color: rgb(255,0,0)">Header</span><span style="color: rgb(0,0,255)">="Dream"</span><span style="color: rgb(139,0,139)">&gt;
&lt;ToolBarTray&gt;
&lt;ToolBar </span><span style="color: rgb(255,0,0)">Header</span><span style="color: rgb(0,0,255)">="Holidays" </span><span style="color: rgb(139,0,139)">&gt;
&lt;Button </span><span style="color: rgb(255,0,0)">HorizontalAlignment</span><span style="color: rgb(0,0,255)">="Left"</span><span style="color: rgb(139,0,139)">&gt;
&lt;StackPanel </span><span style="color: rgb(255,0,0)">Orientation</span><span style="color: rgb(0,0,255)">="Vertical"</span><span style="color: rgb(139,0,139)">&gt;
&lt;Image </span><span style="color: rgb(255,0,0)">Source</span><span style="color: rgb(0,0,255)">="Add.png" </span><span style="color: rgb(255,0,0)">Width</span><span style="color: rgb(0,0,255)">="24"</span><span style="color: rgb(139,0,139)">/&gt;
&lt;TextBlock&gt;</span><span style="color: rgb(0,0,0)">New Holiday</span><span style="color: rgb(139,0,139)">&lt;/TextBlock&gt;
&lt;/StackPanel&gt;
&lt;/Button&gt;
&lt;/ToolBar&gt;<br>                     ...</span>
<a href="http://11011.net/software/vspaste"></a>


Next I'll tidy up the XAML, its a straight brain dump at the moment and went through several iterations of trial and error - there's bound to be some unused portions in there still. I've also made a very simple Decorator for the TabHeader - but more on that later...


