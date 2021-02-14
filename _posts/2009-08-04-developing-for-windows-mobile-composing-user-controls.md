---
layout: post
title: "Developing for Windows Mobile – Composing User Controls"
date: 2009-08-04 16:37
author: spencen
comments: true
categories: [.NET, Development]
tags: []
---


Tonight I finished refactoring the mobile UI framework I’ve been putting together so that it can correctly handle composite elements. Previously I’d been able to handle a collection of polygons (Rectangles, Stars etc.). I’ve now introduced the concept of DrawingContainers that themselves have a collection of elements, any of which may be another DrawingContainer.
  

The idea is that I can now construct UI *controls* that themselves are composed of simple graphic primitives. Just like WPF right? Each graphic element can have its own animations and hit test capabilities and controls can be composed within each other to form richer* user controls*. All this is rendered to the hosting Windows.Forms.Control in a single double-buffered render.
  

The class diagram below gives an indication of where I’m currently at. Everything in the diagram has been implemented.
  

&#160;
  

<a href="http://blog.spencen.com/images/83489-72989/Spencen.Mobile.UI_2.png" target="_blank">![Spencen.Mobile.UI](http://blog.spencen.com/images/83489-72989/Spencen.Mobile.UI_thumb.png "Spencen.Mobile.UI")</a> 
  

As one example of compositing controls I used a multi-part windmill.
  

<a href="http://www.spencen.com/Downloads/winmo_animation_spinning.wmv">![Windows Mobile Spinning Wheel Thing_Thumb](http://blog.spencen.com/images/83489-72989/Windows%20Mobile%20Spinning%20Wheel%20Thing_Thumb_3.jpg "Windows Mobile Spinning Wheel Thing_Thumb")</a> 
  

Defined in code as:
  

<font size="1"><font face="Verdana"><span style="color: blue">var </span>center = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( hostCanvas.Width / 2, hostCanvas.Height / 2 );
<span style="color: blue">var </span>yellowBrush = <span style="color: blue">new </span><span style="color: #2b91af">SolidBrush</span>( <span style="color: #2b91af">Color</span>.Yellow );
<span style="color: blue">var </span>panel = <span style="color: blue">new </span><span style="color: #2b91af">Panel</span>( hostCanvas ) { Size = hostCanvas.Size, Center = center };
<span style="color: blue">var </span>bar1 = <span style="color: blue">new </span><span style="color: #2b91af">Panel</span>( hostCanvas ) { Size = <span style="color: blue">new </span><span style="color: #2b91af">Size</span>( 200, 20 ), Center = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( center.X, center.Y - 190 ) };
<span style="color: blue">var </span>bar2 = <span style="color: blue">new </span><span style="color: #2b91af">Panel</span>( hostCanvas ) { Size = <span style="color: blue">new </span><span style="color: #2b91af">Size</span>( 200, 20 ), Center = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( center.X, center.Y + 190 ) };
panel.Children.Add( <span style="color: blue">new </span><span style="color: #2b91af">Rectangle</span>() { Size = <span style="color: blue">new </span><span style="color: #2b91af">Size</span>( 50, 400 ), Center = center } );
bar1.Children.Add( <span style="color: blue">new </span><span style="color: #2b91af">Rectangle</span>() { Size = bar1.Size, Position = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( 0, 0 ) } );
bar2.Children.Add( <span style="color: blue">new </span><span style="color: #2b91af">Rectangle</span>() { Size = bar1.Size, Position = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( 0, 0 ) } );
bar1.Children.Add( <span style="color: blue">new </span><span style="color: #2b91af">Star</span>() { Size = <span style="color: blue">new </span><span style="color: #2b91af">Size</span>( 75, 75 ),   
                                                 Center = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( bar1.Size.Width / 2 - 90, bar1.Size.Height / 2 ),   
                                                 Background = yellowBrush } );
bar1.Children.Add( <span style="color: blue">new </span><span style="color: #2b91af">Star</span>() { Size = <span style="color: blue">new </span><span style="color: #2b91af">Size</span>( 75, 75 ),   
                                                 Center = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( bar1.Size.Width / 2 + 90, bar1.Size.Height / 2 ),   
                                                 Background = yellowBrush } );
bar2.Children.Add( <span style="color: blue">new </span><span style="color: #2b91af">Star</span>() { Size = <span style="color: blue">new </span><span style="color: #2b91af">Size</span>( 75, 75 ),   
                                                 Center = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( bar2.Size.Width / 2 - 90, bar2.Size.Height / 2 ),   
                                                 Background = yellowBrush } );
bar2.Children.Add( <span style="color: blue">new </span><span style="color: #2b91af">Star</span>() { Size = <span style="color: blue">new </span><span style="color: #2b91af">Size</span>( 75, 75 ),   
                                                 Center = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( bar2.Size.Width / 2 + 90, bar2.Size.Height / 2 ),   
                                                 Background = yellowBrush } );
panel.Children.Add( bar1 );
panel.Children.Add( bar2 );
panel.Transforms.Add( _rotateTransform );
_rotateTransform.RenderCenter = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( hostCanvas.Width / 2, hostCanvas.Height / 2 );
panel.Host.AnimationManager.AddAnimation(   
                      <span style="color: blue">new </span><span style="color: #2b91af">FloatAnimation</span>( _rotateTransform, <span style="color: #a31515">&quot;Angle&quot;</span>, <span style="color: blue">new </span><span style="color: #2b91af">TimeSpan</span>( 0, 0, 50 ) ) { FinalValue = 3600 } );
<span style="color: blue">var </span>barRotation = <span style="color: blue">new </span><span style="color: #2b91af">RotateTransform</span>();
bar1.Transforms.Add( barRotation );
bar2.Transforms.Add( barRotation );
panel.Host.AnimationManager.AddAnimation(   
                     <span style="color: blue">new </span><span style="color: #2b91af">FloatAnimation</span>( barRotation, <span style="color: #a31515">&quot;Angle&quot;</span>, <span style="color: blue">new </span><span style="color: #2b91af">TimeSpan</span>( 0, 0, 50 ) ) { FinalValue = -7200 } );
<span style="color: blue">var </span>starRotation = <span style="color: blue">new </span><span style="color: #2b91af">RotateTransform</span>();
bar1.Children[ 1 ].Transforms.Add( starRotation );
bar1.Children[ 2 ].Transforms.Add( starRotation );
bar2.Children[ 1 ].Transforms.Add( starRotation );
bar2.Children[ 2 ].Transforms.Add( starRotation );
panel.Host.AnimationManager.AddAnimation(   
                    <span style="color: blue">new </span><span style="color: #2b91af">FloatAnimation</span>( starRotation, <span style="color: #a31515">&quot;Angle&quot;</span>, <span style="color: blue">new </span><span style="color: #2b91af">TimeSpan</span>( 0, 0, 50 ) ) { FinalValue = 18000 } );
hostCanvas.View.Children.Add( panel );</font></font>

<a href="http://11011.net/software/vspaste"></a>


Other features so that I’ve got so far:



*   Support for Behaviours. So far the only implementation is a DragBehavior that allows dragging any attached element along one or both axis. Even supports a “flick” by determining the angle and velocity on mouse up and applying an animation with a cubic easing function to simulate velocity decay.
*   Scrolling between views/pages using an easing function (and BitBlt).
*   Simple image support.


Other ideas:



*   Button and ItemsList controls.
*   Stack and custom (e.g. radial) layout panels.
*   Integration with platform APIs to allow drawing of rounded rectangles, gradient fills and to support alphas.
*   A transform for flipping vertically or horizontally.
*   Behaviour to support an ICommand style interface to allowing hooking UI interactions with a view model.

