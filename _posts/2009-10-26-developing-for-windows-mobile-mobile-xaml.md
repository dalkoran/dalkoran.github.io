---
layout: post
title: "Developing for Windows Mobile – Mobile XAML?"
date: 2009-10-26 15:39
author: spencen
comments: true
categories: [.NET, Development, Windows Phone]
tags: []
---


I’ve now included a “first cut” XAML parser (ultra simplistic) within v0.2 of my [Mobile UI Framework](http://mobileui.codeplex.com/).
  

Allows me to convert:
  

<font size="1"><font face="Verdana"><span style="color: blue">&lt;?</span><span style="color: #a31515">xml </span><span style="color: red">version</span><span style="color: blue">=</span>&quot;<span style="color: blue">1.0</span>&quot; <span style="color: red">encoding</span><span style="color: blue">=</span>&quot;<span style="color: blue">utf-8</span>&quot; </font></font><span style="color: blue"><font size="1" face="Verdana">?&gt;
&lt;</font></span><font size="1"><font face="Verdana"><span style="color: #a31515">DrawingPanel
</span><span style="color: red">xmlns</span><span style="color: blue">=</span>&quot;<span style="color: blue">http://mobileui.codeplex.com/v1</span>&quot;
<span style="color: red">xmlns:sysDrawing</span><span style="color: blue">=</span>&quot;<span style="color: blue">System.Drawing</span>&quot;
<span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">Unbound</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">GradientRectangleElement </span><span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">Unbound</span>&quot; <span style="color: red">StartColor</span><span style="color: blue">=</span>&quot;<span style="color: blue">240,240,240</span>&quot; <span style="color: red">EndColor</span><span style="color: blue">=</span>&quot;20<span style="color: blue">0,80,0</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;</span><span style="color: #a31515">TextElement </span><span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">400,500</span>&quot; <span style="color: red">Foreground</span><span style="color: blue">=</span>&quot;<span style="color: blue">200,200,200</span>&quot; <span style="color: red">Text</span><span style="color: blue">=</span>&quot;<span style="color: blue">Demo</span>&quot; <span style="color: red">Angle</span><span style="color: blue">=</span>&quot;<span style="color: blue">315</span>&quot; <span style="color: red">AutoSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">True</span>&quot;   
                          <span style="color: red">HorizontalAlignment</span><span style="color: blue">=</span>&quot;<span style="color: blue">Center</span>&quot; <span style="color: red">VerticalAlignment</span><span style="color: blue">=</span>&quot;<span style="color: blue">Center</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;</span><span style="color: #a31515">DrawingPanel </span><span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">Unbound</span>&quot; <span style="color: red">Background</span><span style="color: blue">=</span>&quot;<span style="color: blue">{null}</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">DrawingPanel.LayoutEngine</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">StackLayout </span><span style="color: red">Margin</span><span style="color: blue">=</span>&quot;<span style="color: blue">8,4</span>&quot; <span style="color: red">Padding</span><span style="color: blue">=</span>&quot;<span style="color: blue">4</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;/</span><span style="color: #a31515">DrawingPanel.LayoutEngine</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ButtonBar </span><span style="color: red">Text</span><span style="color: blue">=</span>&quot;<span style="color: blue">Animations</span>&quot; <span style="color: red">SecondaryText</span><span style="color: blue">=</span>&quot;<span style="color: blue">Animate properties of graph primitives</span>&quot;   
                         <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">UnboundAxis, 80</span>&quot; <span style="color: red">Background</span><span style="color: blue">=</span>&quot;<span style="color: blue">{null}</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;</span><span style="color: #a31515">ButtonBar </span><span style="color: red">Text</span><span style="color: blue">=</span>&quot;<span style="color: blue">Trasitions</span>&quot; <span style="color: red">SecondaryText</span><span style="color: blue">=</span>&quot;<span style="color: blue">Optimized bitmap animations</span>&quot;   
                         <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">UnboundAxis, 80</span>&quot; <span style="color: red">Background</span><span style="color: blue">=</span>&quot;<span style="color: blue">{null}</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;</span><span style="color: #a31515">ButtonBar </span><span style="color: red">Text</span><span style="color: blue">=</span>&quot;<span style="color: blue">Primitives</span>&quot; <span style="color: red">SecondaryText</span><span style="color: blue">=</span>&quot;<span style="color: blue">Polygon, Ellipse, Image, Text</span>&quot;   
                         <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">UnboundAxis, 80</span>&quot; <span style="color: red">Background</span><span style="color: blue">=</span>&quot;<span style="color: blue">{null}</span>&quot; </font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;</span><span style="color: #a31515">ButtonBar </span><span style="color: red">Text</span><span style="color: blue">=</span>&quot;<span style="color: blue">Behaviours</span>&quot; <span style="color: red">SecondaryText</span><span style="color: blue">=</span>&quot;<span style="color: blue">Drag with slide</span>&quot;   
                         <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">UnboundAxis, 80</span>&quot; <span style="color: red">Background</span><span style="color: blue">=</span>&quot;<span style="color: blue">{null}</span>&quot; <span style="color: red">Command</span><span style="color: blue">=</span>&quot;<span style="color: blue">{Binding DragDemoCommand}</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;</span><span style="color: #a31515">ButtonBar </span><span style="color: red">Text</span><span style="color: blue">=</span>&quot;<span style="color: blue">Layout</span>&quot; <span style="color: red">SecondaryText</span><span style="color: blue">=</span>&quot;<span style="color: blue">Stack, Wrap and Radial</span>&quot;   
                          <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">UnboundAxis, 80</span>&quot; <span style="color: red">Background</span><span style="color: blue">=</span>&quot;<span style="color: blue">{null}</span>&quot; <span style="color: red">Command</span><span style="color: blue">=</span>&quot;<span style="color: blue">{Binding LayoutDemoCommand}</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;/</span><span style="color: #a31515">DrawingPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">DrawingPanel</span><span style="color: blue">&gt;</span></font></font>

<a href="http://11011.net/software/vspaste"></a>


Into:

![](http://i3.codeplex.com/Project/Download/FileDownload.aspx?ProjectName=mobileui&amp;DownloadId=89373)


I was getting really sick of doing it the “old fashioned way” ![](http://blog.spencen.com/emoticons/smile.png)



So far I support simple value type converters (float, bool), pen and brush converters, size and padding converters, resource naming (x:Name), complex properties (DrawingPanel.LayoutEngine) and loading types from other CLR namespaces via XML namespace (xmlns:sysDrawing).


