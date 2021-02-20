---
title: "&quot;Star&quot;tlingly Bad Code"
date: 2008-06-03 14:19
author: spencen
comments: true
categories: [.NET, General, WPF]
tags: []
---

Scott Hanselman's got my attention this morning with his recent endeavour to launch a community driven reference application for WPF. If you haven't already read about BabySmash! go read about it [here](http://www.hanselman.com/blog/IntroducingBabySmashAWPFExperiment.aspx) and check it out on codeplex [here](http://www.codeplex.com/babysmash).
 

Scott mentions that the whole thing was put together whilst his wife was watching a rather unappealing movie. He then goes on to mention that the code is bad - really bad. What gets me though is that he has the audacity, nay the gall to suggest that just because he is capable of writing lame-ass code that by some bizarre form of association that his readers too sometimes write code like this. Now - I've downloaded the source code for BabySmash! - and let me tell you the code is pretty ugly. If Scott thinks that I'd ever let code like that be published under my name then... wait... ah... BUGGER! He's used some of my code! What's worse - he used probably the MOST lame-ass piece of code I've ever blogged (oh heck - yes OK I'm sure there are even worse examples of mine).
 

The code in question was a class I [blogged about in November last year](http://blog.spencen.com/2007/11/09/xaml-and-wpf--or-quotim-seeing-starsquot.aspx). I should point out that I made it very clear at the top of the post that this was more or less a rant - "dribble code" if you will. Its a very simply class that derives from Shape to draw a Star.
 

The method I used to draw the star was to take a triangle and simply "stamp" it out multiple times, rotating it around a central axis. The very clever WPF GetOutlinedPathGeometry is then used to clean up all the mess and consolidate into just the outline of the star.
 

Here's the original code that appears now in Scott's BabySmash:


<span style="font-size: 8pt; font-family: verdana"><span>public</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(43,145,175)">Geometry</span> CreateStarGeometry(<span style="color: rgb(0,0,255)">int</span> numberOfPoints)
{
<span style="color: rgb(43,145,175)">GeometryGroup</span> group = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">GeometryGroup</span>();
group.FillRule = <span style="color: rgb(43,145,175)">FillRule</span>.Nonzero;
<span style="color: rgb(43,145,175)">Geometry</span> triangle = <span style="color: rgb(43,145,175)">PathGeometry</span>.Parse(<span style="color: rgb(163,21,21)">"M 0,-30 L 10,10 -10,10 0,-30"</span>);
group.Children.Add(triangle);
<span style="color: rgb(0,0,255)">double</span> deltaAngle = 360 / numberOfPoints;
<span style="color: rgb(0,0,255)">double</span> currentAngle = 0;
<span style="color: rgb(0,0,255)">for</span> (<span style="color: rgb(0,0,255)">int</span> index = 1; index &lt; numberOfPoints; index++)
{
currentAngle += deltaAngle;
triangle = triangle.CloneCurrentValue();
triangle.Transform = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">RotateTransform</span>(currentAngle, 0, 0);
group.Children.Add(triangle);
}
<span style="color: rgb(43,145,175)">Geometry</span> outlinePath = group.GetOutlinedPathGeometry();
<span style="color: rgb(0,0,255)">return</span> outlinePath;
}![DrawingAStar](/images/DrawingAStar_3.png)</span></pre><a href="http://11011.net/software/vspaste"></a>

    
    The code that I replaced that with the first time I actually used the Star class in an application (for a rating indicator) is shown below. It takes what I think is a much neater approach and is a little easier to configure with an inner and outer radius.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(43,145,175)">Geometry</span> CreateStarGeometry2(<br>                             <span style="color: rgb(0,0,255)">int</span> numberOfPoints, <br>                             <span style="color: rgb(0,0,255)">int</span> outerRadius, <br>                             <span style="color: rgb(0,0,255)">int</span> innerRadius, <br>                             <span style="color: rgb(43,145,175)">Point</span> offset)
{
<span style="color: rgb(43,145,175)">List</span>&lt;<span style="color: rgb(43,145,175)">PathSegment</span>&gt; segments = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">List</span>&lt;<span style="color: rgb(43,145,175)">PathSegment</span>&gt;();
<span style="color: rgb(0,0,255)">double</span> angleOffset = <span style="color: rgb(43,145,175)">Math</span>.PI * 2 / numberOfPoints;
<span style="color: rgb(0,0,255)">for</span> (<span style="color: rgb(0,0,255)">double</span> angle = 0; angle &lt; <span style="color: rgb(43,145,175)">Math</span>.PI * 2; angle += angleOffset)
{
<span style="color: rgb(0,0,255)">double</span> innerAngle = angle + angleOffset / 2;
<span style="color: rgb(43,145,175)">Point</span> outerPoint = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(<br>                                   <span style="color: rgb(43,145,175)">Math</span>.Sin(angle) * outerRadius + offset.X, <br>                                   <span style="color: rgb(43,145,175)">Math</span>.Cos(angle) * -outerRadius + offset.Y);
<span style="color: rgb(43,145,175)">Point</span> innerPoint = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(<br>                                   <span style="color: rgb(43,145,175)">Math</span>.Sin(innerAngle) * innerRadius + offset.X, <br>                                   <span style="color: rgb(43,145,175)">Math</span>.Cos(innerAngle) * -innerRadius + offset.Y);
segments.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">LineSegment</span>(outerPoint, <span style="color: rgb(0,0,255)">true</span>));
segments.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">LineSegment</span>(innerPoint, <span style="color: rgb(0,0,255)">true</span>));
}
<span style="color: rgb(43,145,175)">List</span>&lt;<span style="color: rgb(43,145,175)">PathFigure</span>&gt; figures = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">List</span>&lt;<span style="color: rgb(43,145,175)">PathFigure</span>&gt;();
figures.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">PathFigure</span>(<br>                         <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(0 + offset.X, -outerRadius + offset.Y), <br>                         segments, <span style="color: rgb(0,0,255)">true</span>));
<span style="color: rgb(43,145,175)">Geometry</span> star = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">PathGeometry</span>(figures);
<span style="color: rgb(0,0,255)">return</span> star;
}</span>



<a href="http://11011.net/software/vspaste"></a>As embarrassed as I am that my Star class has meandered its way to such a large audience - I must admit I kinda got a kick out of seeing this in Scott's code ![](http://blog.spencen.com/emoticons/smile.png).



In terms of the BabySmash! application itself - I think this is a great idea. Firstly from the point of view that I also have two young kids who love spending time with me on the computers. The younger two year old is at the stage where she recognizes most of the alphabet and loves typing random letters and seeing them appear on the screen. I wrote my [WinForms animation sample](http://blog.spencen.com/2007/10/19/winforms-animation.aspx) primarily because my oldest(then 4) got a kick out of watching the bits dance around the screen (a later version included A-Z characters). [Alas he's now turned 5 and has since moved on to solving [Bloxorz](http://www.albinoblacksheep.com/games/bloxorz) levels - scary!]



Secondly the idea of a community driven WPF application just sounds like a great idea. I've been very slowly trying to build my own "hobby" application using WPF. Stumbling and being sidetracked at every turn - all very educational for me - but certainly delaying any semblance of a deliverable. Maybe Scott's approach is the way to go - hack it together ASAP - then let the community work together to discuss, refactor and enhance?


