---
layout: post
title: "Developing for Windows Mobile – XAML for Win Forms"
date: 2009-11-06 14:51
author: spencen
comments: true
categories: [.NET, Development, Windows Phone]
tags: []
---


Many years ago (2004/5?) I was given the task of writing a XAML engine for System.Windows.Forms. It was a great experience and to do the job properly (I hope) it took quite some time. It had support for most of what’s available in WPF’s XAML – namespaces, markup extensions, attached properites (IExtenderProviders in WinForms speak), type converters, late bound binding, styles, triggers etc. plus a bunch of stuff that isn’t - #include, using parameterized constructors, simplified referencing etc.
  

In my Windows Mobile UI Framework the idea of having the capability of defining screens in XAML was kind of an afterthought. I had truly forgotten how painful it is to define UI’s programmatically. I’ve also never been a fan of form designers – I think they are slow, in-accurate and generate hard to maintain code/XAML. To this day I do 90% of my XAML using an XML editor (VS or Kaxaml).
  

So as per my [last post](http://blog.spencen.com/2009/10/27/developing-for-windows-mobile-ndash-mobile-xaml.aspx) I whipped up a very simplified XAML parser/renderer to use with my Mobile UI Framework. However, the latest control that I added to the framework was **FormHost** which allows a System.Windows.Forms control to be hosted within a **DrawingElement** (my UI base class). Of course when I say “hosted” the FormHost is really just acting as a placeholder so that the hosted control can be positioned during the render pass.
  

Here’s the code I had to add for FormHost:
  

<font size="1"><font face="Verdana"><span style="color: blue">using </span>System;
<span style="color: blue">using </span>System.Drawing;
<span style="color: blue">using </span>System.Windows.Forms;
<span style="color: blue">using </span>Spencen.Mobile.UI.Primitives;
<span style="color: blue">namespace </span>Spencen.Mobile.UI.Controls
{
<span style="color: blue">public class </span><span style="color: #2b91af">FormHost </span>: </font></font><font size="1"><font face="Verdana"><span style="color: #2b91af">DrawingContainer
</span>{
<span style="color: blue">private </span><span style="color: #2b91af">Control </span>_hostControl;
<span style="color: blue">public </span>FormHost( <span style="color: #2b91af">IDrawingHost </span>host ) : <span style="color: blue">base</span>( host )
{
_hostControl = host <span style="color: blue">as </span><span style="color: #2b91af">Control</span>;
<span style="color: blue">if </span>( _hostControl == <span style="color: blue">null </span>)
<span style="color: blue">throw new </span><span style="color: #2b91af">ArgumentException</span>(  
                       <span style="color: #a31515">&quot;FormHost must have an IDrawingHost that is a Windows.Forms.Control.&quot;</span>, <span style="color: #a31515">&quot;host&quot;</span>);
</font></font><font size="1"><font face="Verdana"><span style="color: green">// By default we have no background or border
</span>Background = <span style="color: blue">new </span><span style="color: #2b91af">SolidBrush</span>( <span style="color: #2b91af">Color</span>.Transparent );
Stroke = <span style="color: blue">null</span>;
}
<span style="color: blue">public </span><span style="color: #2b91af">Control </span>HostedControl { <span style="color: blue">get</span>; <span style="color: blue">set</span>; }
<span style="color: blue">public override bool </span>SupportsRotation { <span style="color: blue">get </span>{ <span style="color: blue">return false</span>; } }
<span style="color: blue">protected override void </span>OnRender( <span style="color: #2b91af">GraphicsContext </span>context )
{
<span style="color: blue">base</span>.OnRender( context );
<span style="color: blue">var </span>transformedBounds = TransformedBounds( context );
<span style="color: blue">if </span>( !_hostControl.Controls.Contains( HostedControl ) )
_hostControl.Controls.Add( HostedControl );
<span style="color: blue">var </span>newLocation = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( transformedBounds.Left, transformedBounds.Top );
<span style="color: blue">if </span>( newLocation != HostedControl.Location )
HostedControl.Location = newLocation;
<span style="color: blue">var </span>newSize = <span style="color: blue">new </span><span style="color: #2b91af">Size</span>( transformedBounds.Width + 1, transformedBounds.Height + 1);
<span style="color: blue">if </span>( newSize != HostedControl.Size )
HostedControl.Size = newSize;
}
}
}</font></font></pre>

    
    <a href="http://11011.net/software/vspaste"></a>So pretty simple – but what was extra nice was the fact that it meant I could immediately declare my hosted System.Windows.Forms controls using XAML without any changes to the XAML parser/renderer at all.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;?</span><span style="color: #a31515">xml </span><span style="color: red">version</span><span style="color: blue">=</span>&quot;<span style="color: blue">1.0</span>&quot; <span style="color: red">encoding</span><span style="color: blue">=</span>&quot;<span style="color: blue">utf-8</span>&quot; </font></font><span style="color: blue"><font size="1" face="Verdana">?&gt;
&lt;</font></span><font size="1"><font face="Verdana"><span style="color: #a31515">View
</span><span style="color: red">xmlns</span><span style="color: blue">=</span>&quot;<span style="color: blue">http://mobileui.codeplex.com/v1</span>&quot;
<span style="color: red">xmlns:x</span><span style="color: blue">=</span>&quot;<span style="color: blue"><a href="http://mobileui.codeplex.com/xaml&quot;">http://mobileui.codeplex.com/xaml</span>&quot;
</a>  <span style="color: red">xmlns:WinForms</span><span style="color: blue">=</span>&quot;<span style="color: blue">System.Windows.Forms,System.Windows.Forms</span>&quot;
<span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">Unbound</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">
&lt;</span><span style="color: #a31515">DrawingPanel </span><span style="color: red">x:Name</span><span style="color: blue">=</span>&quot;<span style="color: blue">container</span>&quot; <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">Unbound</span>&quot; <span style="color: red">Background</span><span style="color: blue">=</span>&quot;<span style="color: blue">{null}</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextElement </span><span style="color: red">Text</span><span style="color: blue">=</span>&quot;<span style="color: blue">Text</span>&quot; <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">150,60</span>&quot; <span style="color: red">AutoSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">True</span>&quot;/</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt; </span></font></font><font size="1"><font face="Verdana"><span style="color: blue">
&lt;</span><span style="color: #a31515">RectangleElement </span><span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">50,80</span>&quot;/</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">    &lt;</span><span style="color: #a31515">EllipseElement </span><span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">40,70</span>&quot;/</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">RegularPolygonElement </span><span style="color: red">NumberOfSides</span><span style="color: blue">=</span>&quot;<span style="color: blue">6</span>&quot; <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">80,80</span>&quot;/</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">RegularPolygonElement </span><span style="color: red">NumberOfSides</span><span style="color: blue">=</span>&quot;<span style="color: blue">3</span>&quot; <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">90,70</span>&quot;/</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;  
        </span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">RegularPolygonElement </span><span style="color: red">NumberOfSides</span><span style="color: blue">=</span>&quot;<span style="color: blue">8</span>&quot; <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">80,80</span>&quot;/</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">    &lt;</span><span style="color: #a31515">Star </span><span style="color: red">NumberOfPoints</span><span style="color: blue">=</span>&quot;<span style="color: blue">5</span>&quot; <span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">80,80</span>&quot;/</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">
&lt;!-- </span><span style="color: green">Just to spice things up - here's some WinForms controls - just don't expect them to rotate! </span></font></font><font size="1"><font face="Verdana"><span style="color: blue">--&gt;
&lt;</span><span style="color: #a31515">FormHost </span><span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">240,40</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">FormHost.HostedControl</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">WinForms:TextBox </span><span style="color: red">Text</span><span style="color: blue">=</span>&quot;<span style="color: blue">Hello WinForms</span>&quot; <span style="color: red">Multiline</span><span style="color: blue">=</span>&quot;<span style="color: blue">True</span>&quot; <span style="color: red">BackColor</span><span style="color: blue">=</span>&quot;<span style="color: blue">240,255,240</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;/</span><span style="color: #a31515">FormHost.HostedControl</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">FormHost</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">FormHost </span><span style="color: red">DesiredSize</span><span style="color: blue">=</span>&quot;<span style="color: blue">240,80</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">FormHost.HostedControl</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">WinForms![](http://blog.spencen.com/emoticons/tongue.png)anel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">WinForms![](http://blog.spencen.com/emoticons/tongue.png)anel.Controls</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">WinForms:RadioButton </span><span style="color: red">Checked</span><span style="color: blue">=</span>&quot;<span style="color: blue">True</span>&quot; <span style="color: red">Text</span><span style="color: blue">=</span>&quot;<span style="color: blue">Is WinForms?</span>&quot; <span style="color: red">BackColor</span><span style="color: blue">=</span>&quot;<span style="color: blue">255,255,223</span>&quot;   
                                                   <span style="color: red">Dock</span><span style="color: blue">=</span>&quot;<span style="color: blue">Top</span>&quot; <span style="color: red">Height</span><span style="color: blue">=</span>&quot;<span style="color: blue">20</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;</span><span style="color: #a31515">WinForms:RadioButton </span><span style="color: red">Checked</span><span style="color: blue">=</span>&quot;<span style="color: blue">True</span>&quot; <span style="color: red">Text</span><span style="color: blue">=</span>&quot;<span style="color: blue">Is MobileUI?</span>&quot; <span style="color: red">BackColor</span><span style="color: blue">=</span>&quot;<span style="color: blue">255,255,223</span>&quot;   
                                                   <span style="color: red">Dock</span><span style="color: blue">=</span>&quot;<span style="color: blue">Fill</span>&quot;</font></font><font size="1"><font face="Verdana"><span style="color: blue">/&gt;
&lt;/</span><span style="color: #a31515">WinForms![](http://blog.spencen.com/emoticons/tongue.png)anel.Controls</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">WinForms![](http://blog.spencen.com/emoticons/tongue.png)anel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">FormHost.HostedControl</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">FormHost</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">DrawingPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">View</span><span style="color: blue">&gt;</span></font></font>

<a href="http://11011.net/software/vspaste"></a>


![MobileUI_Screenshot 5](/images/MobileUI_Screenshot%205_1.png "MobileUI_Screenshot 5")


