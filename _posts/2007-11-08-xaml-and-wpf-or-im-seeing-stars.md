---
title: "XAML and WPF - or &quot;I'm seeing stars&quot;"
date: 2007-11-08 14:54
author: spencen
comments: true
categories: [.NET, Development, General, WPF]
tags: []
---

<font color="#808080">**Warning** - This post just describes some fun I've been having learning about 2D graphics in WPF. There's certainly nothing very clever going on here, but its been pointed out to me by a prominent Australian blogger that its not the quality but the quantity of blog posts that's important :-p</font>
 

<font color="#808080">But more seriously if you **do** want to read something of substance check out <a href="http://www.paulstovell.net" target="_blank">Paul's</a> post regarding why <a href="http://www.paulstovell.net/blog/index.php/evolution-of-the-wpf-designer/" target="_blank">he doesn't believe the Visual Studio 2008 Form Designer (Cider) will help developer productivity</a>.</font>
 

So anyway - about those stars...
 

I'm having a hard time at the moment really diving into a serious bit of WPF programming. It seems I'm constantly being tripped by the question of "should I do this in XAML, or should I do this in C#?". Now traditionally (under WinForms) when writing a custom control I'd just start with a blank class file, determine the most appropriate Control/Component base class to derive from and start coding properties, events and any required rendering.
 

Now each time I've tried that approach with WPF - I end up realising pretty early on that its the wrong approach. Everything apart from the very specific properties and events are already there - or are provided by attached properties/events. The rendering is much easier to do in XAML too - in fact if done property the rendering is almost completely divorced from the control definition anyway and ends up in a theme based or Generic.xaml file.
 

Ok - so that's good right? Well it sounds right - but I think I'm just having trouble coming to grips with it. The stumbling block I'm having at the moment with custom controls is realising when to step out of the XAML and into fleshing out the real logic. Trouble is with custom controls most of the logic is related to the UI!
 

As an example - I recently wanted to create some simple graphics by having some spinning stars. So my first reaction was to just jump straight into XAML and code up a filled polygon path using the System.Windows.Shapes.Polygon. 
 <div>

<span style="color: #0000ff">&lt;</span><span style="color: #800000">Polygon</span> <span style="color: #ff0000">Points</span><span style="color: #0000ff">="30,0 35,20 55,25 35,30 30,50 25,30 5,25 25,20"</span> <span style="color: #ff0000">Fill</span><span style="color: #0000ff">="Gold"</span> <span style="color: #ff0000">Stroke</span><span style="color: #0000ff">="Black"<br></span><span style="color: #ff0000">              StrokeThickness</span><span style="color: #0000ff">="2"</span><span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Polygon</span> <span style="color: #ff0000">Points</span><span style="color: #0000ff">="30,0 35,10 45,15 35,20 30,30 25,20 15,15 25,10"</span> <span style="color: #ff0000">Fill</span><span style="color: #0000ff">="Gold"</span> <span style="color: #ff0000">Stroke</span><span style="color: #0000ff">="Black"</span> <br>              <span style="color: #ff0000">StrokeThickness</span><span style="color: #0000ff">="2"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Polygon.LayoutTransform</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">RotateTransform</span> <span style="color: #ff0000">Angle</span><span style="color: #0000ff">="45"</span><span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Polygon.LayoutTransform</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Polygon</span><span style="color: #0000ff">&gt;</span></pre></div>

    
    ![SimpleXAMLStar](/images/SimpleXAMLStar_3.png)&nbsp;
    

    
    Ok - but that's a pretty lame star... and I want lots of them right - so I should create a custom control inheriting from Shape? Whilst I'm at it add a property that lets me configure the number of points too.
    
<div><pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; color: black; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: verdana, consolas, 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; border-bottom-style: none"><span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> Star : Shape
{
<span style="color: #008000">// Using a DependencyProperty as the backing store for NumberOfPoints.  <br></span>    <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">readonly</span> DependencyProperty NumberOfPointsProperty =
DependencyProperty.Register(<span style="color: #006080">"NumberOfPoints"</span>, <span style="color: #0000ff">typeof</span>(<span style="color: #0000ff">int</span>), <span style="color: #0000ff">typeof</span>(Shape), <span style="color: #0000ff">new</span> UIPropertyMetadata(5));
<span style="color: #0000ff">public</span> <span style="color: #0000ff">int</span> NumberOfPoints
{
get { <span style="color: #0000ff">return</span> (<span style="color: #0000ff">int</span>)GetValue(NumberOfPointsProperty); }
set { SetValue(NumberOfPointsProperty, <span style="color: #0000ff">value</span>); }
}
<span style="color: #0000ff">protected</span> <span style="color: #0000ff">override</span> Geometry DefiningGeometry
{
get { <span style="color: #0000ff">return</span> VisualContainer.CreateStarGeometry(NumberOfPoints); }
}
}</pre></div><a href="http://11011.net/software/vspaste"></a>

    
    Creating the geometry for an n-pointed star could be done a heap of ways. Mine was the easiest to visualize but certainly not very elegant. Just create a triangle for each prong and keep rotating for as many as required. Then use the GetOutlinedPathGeometry to get the enclosing path.
    
<div><pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; color: black; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: verdana, consolas, 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; border-bottom-style: none"><span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> Geometry CreateStarGeometry(<span style="color: #0000ff">int</span> numberOfPoints)
{
GeometryGroup group = <span style="color: #0000ff">new</span> GeometryGroup();
group.FillRule = FillRule.Nonzero;
Geometry triangle = PathGeometry.Parse(<span style="color: #006080">"M 0,-30 L 10,10 -10,10 0,-30"</span>);
group.Children.Add(triangle);
<span style="color: #0000ff">double</span> deltaAngle = 360 / numberOfPoints;
<span style="color: #0000ff">double</span> currentAngle = 0;
<span style="color: #0000ff">for</span> (<span style="color: #0000ff">int</span> index = 1; index &lt; numberOfPoints; index++)
{
currentAngle += deltaAngle;
triangle = triangle.CloneCurrentValue();
triangle.Transform = <span style="color: #0000ff">new</span> RotateTransform(currentAngle, 0, 0);
group.Children.Add(triangle);
}
Geometry outlinePath = group.GetOutlinedPathGeometry();
<span style="color: #0000ff">return</span> outlinePath;
}</pre></div>

    
    Now I've got a Shape they're much easier to re-use - so much so we may as well even add some animation.
    
<div><pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; color: black; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: verdana, consolas, 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; border-bottom-style: none">
    
    <span style="color: #0000ff">&lt;</span><span style="color: #800000">control:Star</span> <span style="color: #ff0000">NumberOfPoints</span><span style="color: #0000ff">="5"</span> <span style="color: #ff0000">Width</span><span style="color: #0000ff">="60"</span> <span style="color: #ff0000">Height</span><span style="color: #0000ff">="60"</span> <span style="color: #ff0000">Stroke</span><span style="color: #0000ff">="Black"</span> <span style="color: #ff0000">Fill</span><span style="color: #0000ff">="Gold"</span> <span style="color: #ff0000">StrokeThickness</span><span style="color: #0000ff">="2"</span> <br>                    <span style="color: #ff0000">Opacity</span><span style="color: #0000ff">="0.5"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">control:Star.Triggers</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">EventTrigger</span> <span style="color: #ff0000">RoutedEvent</span><span style="color: #0000ff">="control:Star.MouseEnter"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">EventTrigger.Actions</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">BeginStoryboard</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Storyboard</span> <span style="color: #ff0000">TargetProperty</span><span style="color: #0000ff">="Angle"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">DoubleAnimation</span> <span style="color: #ff0000">Storyboard</span>.<span style="color: #ff0000">TargetName</span><span style="color: #0000ff">="starRotation"</span> <span style="color: #ff0000">From</span><span style="color: #0000ff">="0"</span> <span style="color: #ff0000">To</span><span style="color: #0000ff">="72"</span> <br>                                                   <span style="color: #ff0000">Duration</span><span style="color: #0000ff">="0:0:1"</span> <span style="color: #ff0000">AccelerationRatio</span><span style="color: #0000ff">="0.3"</span> <span style="color: #ff0000">DecelerationRatio</span><span style="color: #0000ff">="0.3"</span><span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">DoubleAnimation</span> <span style="color: #ff0000">Storyboard</span>.<span style="color: #ff0000">TargetProperty</span><span style="color: #0000ff">="Opacity"</span> <span style="color: #ff0000">From</span><span style="color: #0000ff">="0.5"</span> <span style="color: #ff0000">To</span><span style="color: #0000ff">="1.0"</span> <br>                                                   <span style="color: #ff0000">Duration</span><span style="color: #0000ff">="0:0:0.5"</span> <span style="color: #ff0000">AutoReverse</span><span style="color: #0000ff">="True"</span><span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Storyboard</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">BeginStoryboard</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">EventTrigger.Actions</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">EventTrigger</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">control:Star.Triggers</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">control:Star.RenderTransform</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">RotateTransform</span> <span style="color: #ff0000">x:Name</span><span style="color: #0000ff">="starRotation"</span> <span style="color: #ff0000">Angle</span> <span style="color: #0000ff">="0"</span><span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">control:Star.RenderTransform</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">control:Star</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">control:Star</span> <span style="color: #ff0000">NumberOfPoints</span><span style="color: #0000ff">="6"</span> <span style="color: #ff0000">Width</span><span style="color: #0000ff">="60"</span> <span style="color: #ff0000">Height</span><span style="color: #0000ff">="60"</span> <span style="color: #ff0000">Stroke</span><span style="color: #0000ff">="Black"</span> <span style="color: #ff0000">Fill</span><span style="color: #0000ff">="Orange"</span> <br>                    <span style="color: #ff0000">StrokeThickness</span><span style="color: #0000ff">="2"</span><span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">control:Star</span> <span style="color: #ff0000">NumberOfPoints</span><span style="color: #0000ff">="7"</span> <span style="color: #ff0000">Width</span><span style="color: #0000ff">="60"</span> <span style="color: #ff0000">Height</span><span style="color: #0000ff">="60"</span> <span style="color: #ff0000">Stroke</span><span style="color: #0000ff">="Black"</span> <span style="color: #ff0000">Fill</span><span style="color: #0000ff">="Red"</span> <br>                    <span style="color: #ff0000">StrokeThickness</span><span style="color: #0000ff">="2"</span><span style="color: #0000ff">/&gt;</span>
    </pre></div>

    
    ![ShapeStars](/images/ShapeStars_3.png) 
    

    
    So I think I'm getting the hang of it. But hold on, according to <a href="http://blog.spencen.com/2007/08/28/wpf-book.aspx" target="_blank">the book</a> if I'm going to have heaps of these things then I shouldn't be using Shapes - I should be using DrawingVisuals within a container.
    
<div><pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; color: black; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: verdana, consolas, 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; border-bottom-style: none"><span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> VisualContainer : Canvas
{
<span style="color: #0000ff">private</span> List&lt;Visual&gt; visuals = <span style="color: #0000ff">new</span> List&lt;Visual&gt;();
<span style="color: #0000ff">public</span> VisualContainer()
{
DrawingVisual star = CreateStar(5);
visuals.Add(star);
<span style="color: #0000ff">foreach</span> (Visual visual <span style="color: #0000ff">in</span> visuals)
{
AddVisualChild(visual);
AddLogicalChild(visual);
}
}
<span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> DrawingVisual CreateStar(<span style="color: #0000ff">int</span> numberOfPoints)
{
DrawingVisual star = <span style="color: #0000ff">new</span> DrawingVisual();
<span style="color: #0000ff">using</span> (DrawingContext drawingContext = star.RenderOpen())
{
Geometry outlinePath = CreateStarGeometry(numberOfPoints);
drawingContext.DrawGeometry(Brushes.Gold, <span style="color: #0000ff">new</span> Pen(Brushes.Black, 2), outlinePath);
}
<span style="color: #0000ff">return</span> star;
}
<span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> Geometry CreateStarGeometry(<span style="color: #0000ff">int</span> numberOfPoints)
{&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ...&nbsp; as above ...
}
<span style="color: #0000ff">protected</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">int</span> VisualChildrenCount
{
get { <span style="color: #0000ff">return</span> visuals.Count; }
}
<span style="color: #0000ff">protected</span> <span style="color: #0000ff">override</span> Visual GetVisualChild(<span style="color: #0000ff">int</span> index)
{
<span style="color: #0000ff">if</span> (index &lt; 0 || index &gt;= visuals.Count)
<span style="color: #0000ff">throw</span> <span style="color: #0000ff">new</span> ArgumentOutOfRangeException(<span style="color: #006080">"index"</span>);
<span style="color: #0000ff">return</span> visuals[index];
}
<span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> AddStar()
{
Visual visual = CreateStar(5);
visuals.Add(visual);
PositionVisuals();
AddVisualChild(visual);
AddLogicalChild(visual);
}
<span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> PositionVisuals()
{
<span style="color: #0000ff">if</span> (visuals.Count == 1)
((DrawingVisual)visuals[0]).Offset = <span style="color: #0000ff">new</span> Vector(Width / 2, Height / 2);
<span style="color: #0000ff">else</span>
{
<span style="color: #0000ff">double</span> angle = 0;
<span style="color: #0000ff">double</span> deltaAngle = Math.PI * 2 / visuals.Count;
<span style="color: #0000ff">double</span> radius = Width / 2;
<span style="color: #0000ff">foreach</span> (DrawingVisual visual <span style="color: #0000ff">in</span> visuals)
{
visual.Offset = <span style="color: #0000ff">new</span> Vector(Width / 2 + Math.Cos(angle) * radius, <br>                                                       Height / 2 + Math.Sin(angle) * radius);
angle += deltaAngle;
}
}
}
}</pre></div>

    
    So that gives me a container that I can create heaps of stars in and only have one UIElement - the stars themselves are the apparently much lighter weight DrawingVisual instances. The cool thing with these is that Visual Hit Testing actually allows me to respond to click events on individual stars. 
    
<div><pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; color: black; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: verdana, consolas, 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; border-bottom-style: none"><span style="color: #0000ff">&lt;</span><span style="color: #800000">control:VisualContainer</span> <span style="color: #ff0000">Width</span><span style="color: #0000ff">="150"</span> <span style="color: #ff0000">Height</span><span style="color: #0000ff">="150"</span> <span style="color: #ff0000">MouseLeftButtonUp</span><span style="color: #0000ff">="VisualContainer_MouseLeftButtonUp"</span> <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="color: #ff0000">RenderTransformOrigin</span><span style="color: #0000ff">="0.5,0.5"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">control:VisualContainer.RenderTransform</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">RotateTransform</span> <span style="color: #ff0000">x:Name</span><span style="color: #0000ff">="rotateTransform"</span> <span style="color: #ff0000">Angle</span><span style="color: #0000ff">="0"</span> <span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">control:VisualContainer.RenderTransform</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">control:VisualContainer</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Button</span> <span style="color: #ff0000">HorizontalAlignment</span><span style="color: #0000ff">="Center"</span><span style="color: #0000ff">&gt;</span>Rotate
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Button.Triggers</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">EventTrigger</span> <span style="color: #ff0000">RoutedEvent</span><span style="color: #0000ff">="Button.Click"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">EventTrigger.Actions</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">BeginStoryboard</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Storyboard</span> <span style="color: #ff0000">TargetProperty</span><span style="color: #0000ff">="Angle"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">DoubleAnimation</span> <span style="color: #ff0000">Storyboard</span>.<span style="color: #ff0000">TargetName</span><span style="color: #0000ff">="rotateTransform"</span> <span style="color: #ff0000">From</span><span style="color: #0000ff">="0"</span> <span style="color: #ff0000">To</span><span style="color: #0000ff">="360"</span> <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="color: #ff0000">Duration</span><span style="color: #0000ff">="0:0:5"</span><span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Storyboard</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">BeginStoryboard</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">EventTrigger.Actions</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">EventTrigger</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Button.Triggers</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Button</span><span style="color: #0000ff">&gt;</span>
</div>


Of course I couldn't resist rotating the whole thing too ![](http://blog.spencen.com/emoticons/smile.png)



![RotatingDrawingVisuals](/images/RotatingDrawingVisuals_3.png)


