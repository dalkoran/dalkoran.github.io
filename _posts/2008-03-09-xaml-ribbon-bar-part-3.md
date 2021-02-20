---title: "XAML Ribbon Bar - Part 3"
date: 2008-03-09 15:13
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---
Doh! Various remembered references to WPF element z-ordering and ClipToBounds properties suddenly all came together with the realisation that negative values are allowed on the Margin property!
 

So the ["removal" of the line between the tab header and the border](http://blog.spencen.com/2008/03/09/xaml-ribbon-bar--part-2.aspx) around the tab content becomes so much nicer using the following:
 

*   Set the element containing the ItemsPresenter (tab headers) to have <span style="color: rgb(255,0,0)">Panel.ZIndex</span><span style="color: rgb(0,0,255)">="5"</span> so that it gets painted *after* the content border element.
*   Set the TabHeaderBorder to have <span style="color: rgb(255,0,0)">Margin</span><span style="color: rgb(0,0,255)">="0,0,0,-2" </span>so that it intrudes a few pixels into the content border's area. Because the z-index ensure the TabHeaerBorder gets painted after the border this effectively lets the header overwrite the content border.
*   Tweak TabHeaderBorder OnRender() so that the background extends a few pixels (default of 2) past the curvy border. The *extra* background will be used to cover up the border (which means the bottom gradient of the tab header must match the top gradient colour of the content area - which is the whole effective I'm trying to achieve anyway).
*   Use a Trigger to <span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(255,0,0)"> Property</span><span style="color: rgb(0,0,255)">="ClipToBounds"</span><span style="color: rgb(255,0,0)"> Value</span><span style="color: rgb(0,0,255)">="True"/&gt; </span>on the TabItem when <span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Trigger</span><span style="color: rgb(255,0,0)"> Property</span><span style="color: rgb(0,0,255)">="IsSelected"</span><span style="color: rgb(255,0,0)"> Value</span><span style="color: rgb(0,0,255)">="False"&gt;</span>. This ensures that if a mouse over effect is used to render a background it won't intrude into the content area unless its the selected tab. 

So I can now ditch the whole [TabContentBorder](http://blog.spencen.com/2008/03/09/xaml-ribbon-bar--part-2.aspx) class - which was thoroughly nasty anyhow.
 

I've modified the XAML and uploaded <a href="http://www.spencen.com/Downloads/RibbonBarPart3.xaml" target="_blank">here</a>. I've changed the uploaded version to use a plain Border instead of my TabHeaderBorder so you won't get the ultra curvy tabs but this means it can be run as a stand-alone XAML file. All the image references fail of course so there are no pretty images.


