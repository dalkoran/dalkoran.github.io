---
title: "XAML Ribbon Bar - Part 2"
date: 2008-03-08 15:20
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


When designing the XAML Ribbon Bar I really got stuck figuring out how to get the curved effect on the Tab Item. At one stage I used a Path instead of the Border in the markup, so instead of:


<span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Border</span><span style="color: rgb(255,0,0)"> CornerRadius</span><span style="color: rgb(0,0,255)">="5,5,0,0"</span><span style="color: rgb(255,0,0)"> <br>        BorderBrush</span><span style="color: rgb(0,0,255)">="Red"</span><span style="color: rgb(255,0,0)"> BorderThickness</span><span style="color: rgb(0,0,255)">="1,1,1,0"</span><span style="color: rgb(255,0,0)"> Background</span><span style="color: rgb(0,0,255)">="Yellow"</span><span style="color: rgb(255,0,0)"> <br>        HorizontalAlignment</span><span style="color: rgb(0,0,255)">="Left"</span><span style="color: rgb(255,0,0)"> Padding</span><span style="color: rgb(0,0,255)">="4,2"&gt;
</span><span style="color: rgb(0,0,255)">    &lt;</span><span style="color: rgb(163,21,21)">TextBlock</span><span style="color: rgb(0,0,255)">&gt;</span><span style="color: rgb(163,21,21)">Header Text</span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">TextBlock</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Border</span><span style="color: rgb(0,0,255)">&gt;</span></span></pre><a href="http://11011.net/software/vspaste"></a>

    
    ![XAML Tab Header using Border](/images/XAML%20Tab%20Header%20using%20Border_1.png) 
    

    
    I used:
    <pre class="code"><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Canvas</span><span style="color: rgb(255,0,0)"> Height</span><span style="color: rgb(0,0,255)">="25"&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Path</span><span style="color: rgb(255,0,0)"> Stroke</span><span style="color: rgb(0,0,255)">="Red"</span><span style="color: rgb(255,0,0)"> StrokeThickness</span><span style="color: rgb(0,0,255)">="1"</span><span style="color: rgb(255,0,0)"> <span style="color: rgb(255,0,0)">Fill</span><span style="color: rgb(0,0,255)">="Yellow"</span><br>          Data</span><span style="color: rgb(0,0,255)">="M 5,22 Q 10,22 10,17 V 5 Q 10,0 15,0 H 75 Q 80,0 80,5 V 20 Q 80,22 85,22"</span><span style="color: rgb(0,0,255)">/&gt;
</span><span style="color: rgb(0,0,255)">    &lt;</span><span style="color: rgb(163,21,21)">TextBlock</span><span style="color: rgb(255,0,0)"> Margin</span><span style="color: rgb(0,0,255)">="12,4"&gt;</span><span style="color: rgb(163,21,21)">Header Text</span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">TextBlock</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Canvas</span><span style="color: rgb(0,0,255)">&gt;</span></span></pre><pre class="code"><span style="color: rgb(0,0,255)">![XAML Tab Header using Path](/images/XAML%20Tab%20Header%20using%20Path.png) </span></pre><a href="http://11011.net/software/vspaste"><a href="http://11011.net/software/vspaste"></a>

    
    The problem with this was I just couldn't figure out how to get it to scale nicely to the tab header content (names on the tabs). You can't bind to the X and Y properties on a point, and as it turns out there is a bug that means you can't bind to the StartPoint either.
    

    
    After spending too long trying to figure this out I resigned myself to the fact that I was going to have to write some C# code to get the job done. This turned out to be trivial...
    <pre class="code"><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">TabHeaderBorder</span> : <span style="color: rgb(43,145,175)">Border
</span>{
<span style="color: rgb(0,0,255)">protected</span> <span style="color: rgb(0,0,255)">override</span> <span style="color: rgb(0,0,255)">void</span> OnRender(System.Windows.Media.<span style="color: rgb(43,145,175)">DrawingContext</span> dc)
{
<span style="color: rgb(43,145,175)">PathSegmentCollection</span> segments = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">PathSegmentCollection</span>();
segments.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">QuadraticBezierSegment</span>(<br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(CornerRadius.BottomLeft, ActualHeight), <br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(CornerRadius.BottomLeft, ActualHeight - CornerRadius.BottomLeft), <span style="color: rgb(0,0,255)">true</span>));
segments.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">LineSegment</span>(<br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(CornerRadius.BottomLeft, CornerRadius.TopLeft), <span style="color: rgb(0,0,255)">true</span>));
segments.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">QuadraticBezierSegment</span>(<br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(CornerRadius.BottomLeft, 0), <br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(CornerRadius.BottomLeft + CornerRadius.TopLeft, 0), <span style="color: rgb(0,0,255)">true</span>));
segments.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">LineSegment</span>(<br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(ActualWidth - CornerRadius.TopRight - CornerRadius.BottomRight, 0), <span style="color: rgb(0,0,255)">true</span>));
segments.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">QuadraticBezierSegment</span>(<br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(ActualWidth - CornerRadius.BottomRight, 0), <br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(ActualWidth - CornerRadius.BottomRight, CornerRadius.TopRight), <span style="color: rgb(0,0,255)">true</span>));
segments.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">LineSegment</span>(<br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(ActualWidth - CornerRadius.BottomRight, ActualHeight - CornerRadius.BottomRight), <span style="color: rgb(0,0,255)">true</span>));
segments.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">QuadraticBezierSegment</span>(<br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(ActualWidth - CornerRadius.BottomRight, ActualHeight), <br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(ActualWidth, ActualHeight), <span style="color: rgb(0,0,255)">true</span>));
segments.Add(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">LineSegment</span>(<br>           <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(0, ActualHeight), <span style="color: rgb(0,0,255)">false</span>));
<span style="color: rgb(43,145,175)">PathFigure</span> borderPath = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">PathFigure</span>(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(0, ActualHeight), segments, <span style="color: rgb(0,0,255)">true</span>);
<span style="color: rgb(43,145,175)">PathFigureCollection</span> figures = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">PathFigureCollection</span>();
figures.Add(borderPath);
<span style="color: rgb(43,145,175)">PathGeometry</span> borderGeometry = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">PathGeometry</span>(figures);
dc.DrawGeometry(Background, <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Pen</span>(BorderBrush, BorderThickness.Left), borderGeometry);
}
}</span></pre><a href="http://11011.net/software/vspaste"></a>

    
    Which gave the visual effect I was after and the same content capabilties as the Border. This meant that by default it would automatically size to its content. I did cheat a little bit in that the BorderThickness is only treated as a single value - but I did allow separate corner radius values.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">controls</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">TabHeaderBorder</span><span style="color: rgb(255,0,0)"> CornerRadius</span><span style="color: rgb(0,0,255)">="5, 5, 10, 10"</span><span style="color: rgb(255,0,0)"> <br>                    BorderBrush</span><span style="color: rgb(0,0,255)">="Red"</span><span style="color: rgb(255,0,0)"> BorderThickness</span><span style="color: rgb(0,0,255)">="1"</span><span style="color: rgb(255,0,0)"> Background</span><span style="color: rgb(0,0,255)">="Yellow"</span><span style="color: rgb(255,0,0)"> <br>                    HorizontalAlignment</span><span style="color: rgb(0,0,255)">="Left"</span><span style="color: rgb(255,0,0)"> Padding</span><span style="color: rgb(0,0,255)">="12,4"&gt;
</span><span style="color: rgb(0,0,255)">    &lt;</span><span style="color: rgb(163,21,21)">TextBlock</span><span style="color: rgb(0,0,255)">&gt;</span><span style="color: rgb(163,21,21)">Header Text</span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">TextBlock</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">controls</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">TabHeaderBorder</span><span style="color: rgb(0,0,255)">&gt;</span></span></pre><a href="http://11011.net/software/vspaste"></a>

    
    ![XAML Tab Header 1](/images/XAML%20Tab%20Header%201.png)
    

    
    ![XAML Tab Header With Line](/images/XAML%20Tab%20Header%20With%20Line.png)Now - how do I get rid of the horizontal line between the selected Tab Header and the Tab Content? Of course the easiest solutions was to simply not paint the content Border's top edge but I decided that I cared enough.
    

    
    ![XAML Tab Header Without Line](/images/XAML%20Tab%20Header%20Without%20Line.png)Again - I couldn't figure out how to approach this in XAML. So given how easy it was to create the TabHeaderBorder class I created a TabControlBorder. This control gets "linked" to the actual TabControl to check for and subscribes to its SelectedIndex changed event. The event handler then invalidates the controls visuals so that the OnRender method can draw the border and then "paste" a line directly underneath where the TabItem's would be drawn. It certainly isn't pretty but its the most adaptable solution I've come up with so far. The results look Ok - unless you really zoom in using a tool like Windows Magnifier which allows the WPF vectors so scale correctly. Once zoomed you can see that the lines are slightly disjointed - not surprisingly because they are drawn on two different UI elements.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">TabContentBorder</span> : <span style="color: rgb(43,145,175)">Border
</span>    {
<span style="color: rgb(0,128,0)">... Dependency property stuff<br></span>
<span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">void</span> TabControl_SelectionChanged(<span style="color: rgb(0,0,255)">object</span> sender, <span style="color: rgb(43,145,175)">SelectionChangedEventArgs</span> e)
{
InvalidateVisual();
}
<span style="color: rgb(0,0,255)">protected</span> <span style="color: rgb(0,0,255)">override</span> <span style="color: rgb(0,0,255)">void</span> OnRender(System.Windows.Media.<span style="color: rgb(43,145,175)">DrawingContext</span> dc)
{
<span style="color: rgb(0,0,255)">if</span> (TabControl != <span style="color: rgb(0,0,255)">null</span> &amp;&amp; TabControl.SelectedIndex &gt;= 0)
{
<span style="color: rgb(43,145,175)">TabItem</span> selectedItem = (<span style="color: rgb(43,145,175)">TabItem</span>) TabControl.SelectedItem;
<span style="color: rgb(0,0,255)">double</span> x = selectedItem.TransformToAncestor(TabControl).Transform(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Point</span>(0,0)).X;
<span style="color: rgb(0,128,0)">// Use the base paint - we'll paint "over" it with the background colour
</span>                <span style="color: rgb(0,0,255)">base</span>.OnRender(dc);
<span style="color: rgb(0,128,0)">// Erase the border line segment - this is making a rash assumption that a linear gradient brush is used!
</span>                dc.DrawRectangle(<br>                    <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">SolidColorBrush</span>(((<span style="color: rgb(43,145,175)">LinearGradientBrush</span>) Background).GradientStops[0].Color), <span style="color: rgb(0,0,255)">null</span>, <br>                    <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Rect</span>(x + 1, -1, selectedItem.ActualWidth - 1, BorderThickness.Top + 2));
}
<span style="color: rgb(0,0,255)">else
</span>            {
<span style="color: rgb(0,0,255)">base</span>.OnRender(dc);
}<span>
}</span></span>
<a href="http://11011.net/software/vspaste"></a>


Yuerk - that isn't pretty - will have to do for now though ![](http://blog.spencen.com/emoticons/sad.png)



I've changed the ItemsPanelTemplate for the Toolbar to be a fixed height WrapPanel and its pretty much giving me what I want now. Although - I have a very limited set of requirements ![](http://blog.spencen.com/emoticons/smile.png)



&nbsp;![XAML Ribbon Bar - Part 2](/images/XAML%20Ribbon%20Bar%20-%20Part%202.png) 



And just for laughs here the same content but without Style="{StaticResource RibbonBarStyle}" applied. 



&nbsp;![XAML Ribbon Bar Default Style](/images/XAML%20Ribbon%20Bar%20Default%20Style.png) 



Interestingly what got me started down this whole Ribbon path was the fact that the only way I could find to switch off the display of the Overflow chrome on the ToolBar was to override its default template.


