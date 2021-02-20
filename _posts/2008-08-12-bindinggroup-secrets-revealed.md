---title: "BindingGroup Secrets Revealed"
date: 2008-08-12 13:57
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---
When Visual Studio 2008 SP1 Beta was released it was [hinted](http://blog.spencen.com/2008/05/15/bindinggroup-in-net-35-sp1.aspx) that new data binding functionality would be available for WPF applications using a new BindingGroup class.
  

Well with the release of SP1 RTM the BindingGroup surfaces – no longer such a well guarded secret. On the other hand it hasn’t exactly been promoted as a key new feature? I’ve yet to find a mention of it aside from the original reference by Brad Abrams.
  

I’ve only taken a quick glance thus far – but what I’ve seem looks promising. I was hoping for a method to overcome the clumsy “[find a bound control](http://blog.spencen.com/2008/05/02/how-to-get-a-list-of-bindings-in-wpf.aspx)” hack, but couldn’t find it. However, it does provide a *BindingExpressions* collection which provides an easy mechanism to identify all bindings for a given FrameworkElement (including all its children). Well – for those that care – here’s the [MSDN documentation for BindingGroup](http://msdn.microsoft.com/en-us/library/system.windows.data.bindinggroup.aspx).
  


  


  


  


  


  


  


  


  


  


  


  

Also of interest, but swamped by the SP1 RTM headlines a [CTP for the forthcoming WPF DataGrid](http://windowsclient.net/wpf/wpf35/wpf-dg-preview-ctrl-investments.aspx)!
  

<font color="#ff0000">**UPDATE**: </font><a href="http://blogs.msdn.com/vinsibal/archive/2008/08/11/wpf-3-5-sp1-feature-bindinggroups-with-item-level-validation.aspx"><font color="#ff0000">Vincent Sibal explains BindingGroup in full</font></a><font color="#ff0000">.</font>


