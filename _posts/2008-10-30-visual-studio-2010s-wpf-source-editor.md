---title: "Visual Studio 2010’s WPF Source Editor"
date: 2008-10-30 13:04
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---
The last couple of days I’ve been enjoying reading/listening to the announcements coming out of PDC – as I’m sure has most of the Microsoft Developer community.
  

Tonight I watched the on-demand keynote. The highlights for me:
  

*   Windows 7 Taskbar Enhancements
*   Windows 7 Multi-touch
*   WPF demos! In fact, lots of WPF love in general!
*   Visual Studio 2010 shell and editor moved to WPF
*   “Much improved” WPF design experience (including Silverlight)
*   Programming against the Live Mesh API  

And the ultimate highlight was watching Scott “Gu” show has easy it was to extend the design time source editor in Visual Studio 2010 which is rendered in WPF (around 1:30:00 in the video). As simple as implementing an interface (ITextViewService), decorating with an “Export” attribute and then dropping the assembly into the extensions folder. No other registration required. Interestingly, Visual Studio is itself using the new Managed Extensibility Framework, including its support for Add-Ins. What was even cooler (from my perspective) is that Scott’s demo “add-in” showed how easy it was to re-render XML source comments with a custom UserControl declared in XAML. 
  

![VS2010 Source Code Adorner](/images/VS2010%20Source%20Code%20Adorner_1.png "VS2010 Source Code Adorner")&#160;&#160;&#160; 
  

Hey – I think [I predicted/wished for that feature](http://blog.spencen.com/2008/04/17/source-code-comments--time-for-a-revamp.aspx) :-p. This is even better – now I can build my own and customize however I like!


