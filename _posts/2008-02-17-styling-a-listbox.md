---
layout: post
title: "Styling a ListBox"
date: 2008-02-17 12:09
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


I've found that the WPF ListBox control is one of my most commonly used controls when designing screens. Its ability to use DataTemplates to control how each item is rendered, ability to override the layout mechanism of the items via the ItemsPanelTempalte and even some pretty cool grouping capabilities.
 

Over the last couple of weeks I've put together a few screens in a little app that I'm writing. It had got to the stage where data was appearing on screen and the back-end (consisting of LINQ for SQL) was doing a good job of extracting the information from the existing database. I figured it was time to allow myself a little time to "pretty up" the UI.
 

I applied some very simple backgrounds and a simple animation for "mouse over" highlighting of list box items. But then I hit a stumbling block. The SelectedItem was being rendered using the standard blue background, or gray when the control didn't have focus. What's more this was taking preference to any property changes I made in the IsSelected trigger. No problem - that should be easy to change right? Hmm... at least that's what I thought. 
 

> 

[solution](http://blogs.msdn.com/wpfsdk/archive/2007/08/31/specifying-the-selection-color-content-alignment-and-background-color-for-items-in-a-listbox.aspx) (to this common problem) that seemed to be the most prevalent on the net was certainly much easier, but it just seemed to be clumsy and error prone. You "highjack" the resource keys used to render the standard template's selection background in a local scope - either replacing with your own colour - or disabling by making them transparent and handle the rendering yourself via your own triggers (with animations etc.).


<span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ListBox</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="eventList"</span><span style="color: rgb(255,0,0)"> Background</span><span style="color: rgb(0,0,255)">="Black"</span><span style="color: rgb(255,0,0)"> ItemsSource</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=Events</span>/<span style="color: rgb(0,0,255)">Events}"</span><span style="color: rgb(255,0,0)"> <br>                 IsSynchronizedWithCurrentItem</span><span style="color: rgb(0,0,255)">="True"</span><span style="color: rgb(255,0,0)"> Grid.Row</span><span style="color: rgb(0,0,255)">="1"</span><span style="color: rgb(255,0,0)"> ItemTemplate</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">StaticResource</span><span style="color: rgb(255,0,0)"> dayTemplate</span><span style="color: rgb(0,0,255)">}"
</span>                <span style="color: rgb(255,0,0)"> ScrollViewer.HorizontalScrollBarVisibility</span><span style="color: rgb(0,0,255)">="Disabled"</span><span style="color: rgb(255,0,0)"> BorderThickness</span><span style="color: rgb(0,0,255)">="0"&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ListBox.Resources</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">SolidColorBrush</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Key</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Static</span><span style="color: rgb(255,0,0)"> SystemColors</span><span style="color: rgb(0,0,255)">.</span><span style="color: rgb(255,0,0)">HighlightBrushKey</span><span style="color: rgb(0,0,255)">}"</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">="Transparent"/&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">SolidColorBrush</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Key</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Static</span><span style="color: rgb(255,0,0)"> SystemColors</span><span style="color: rgb(0,0,255)">.</span><span style="color: rgb(255,0,0)">ControlBrushKey</span><span style="color: rgb(0,0,255)">}"</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">="Transparent"/&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ListBox.Resources</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ListBox</span><span style="color: rgb(0,0,255)">&gt;</span></span>
<a href="http://11011.net/software/vspaste"></a>


For this to work:



1.  You have to know that the standard colours for highlighting in a ListBox are System.Highlight (fairly obvious) and System.Control (guesswork).
No other components of the controls standard rendering that you want to retain can use these colours.


There must be a better, simple way?


