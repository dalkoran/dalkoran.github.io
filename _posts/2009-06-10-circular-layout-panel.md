---title: "Circular Layout Panel"
date: 2009-06-10 14:40
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---
I’ve been given an opportunity to write a custom WPF layout panel. This is something that I’ve been wanting to try for ages but have never really had the need. Rather than jumping straight in to some potentially complex layout algorithms, I figured that I’d start with something trivial just to get the hang of things. Hence the CircularPanel was born.
  

The CircularPanel is a simple Panel derivative that lays out its children in a circular arrangement. It has some useful dependency properties to allow some customization.
  

*   **StartAngle** – Angle in degrees at which the first child element will be positioned. 
*   **EndAngle** – Angle in degrees at which the last child element will be positioned. If the EndAngle forms a complete circle, e.g. Math.Abs(StartAngle-EndAngle) &gt; 360 then rather than positioning the last element on top of the first the element an angle will be included between first and last elements. 
*   **Padding** – As per standard – reduces the space within the panel used to layout the child elements.   

When adding these properties I originally missed the AffectsArrange flag on the property registration’s FrameworkElementMetaData. Without this flag changing the property (for instance in the designer) would not cause the control to re-render.
  

I also added some attached dependency properties. These properties can be used by the child UI elements to dictate an override to the default behaviour.
  

*   **FixedAngle** – Overrides the default automatic assignment of angle based on child index. Instead the child element is placed at the fixed angle specified in degrees. 
*   **RadiusScaleX** – Allows the radius to be scaled in the X direction for this child element – defaults to 1. 
*   **RadiusScaleY** – Allows the radius to be scaled in the Y direction for this child element – defaults to 1.   

When adding these attached properties I made sure I included the AffectsArrange flag. Of course this wasn’t right – changing the attached property requires the parent to re-calculate the arrangement, it doesn’t affect the applied elements arrangement. I updated the flag to AffectsParentArrange and all was good.
  

Now I’m not sure how much real-world value this panel has – but I did get the combination of these properties to produce some interesting affects. For example:
  

Set StartAngle = 0, EndAngle = 1080 and then have each element decrease the RadiusScaleX/Y via binding. This produces a nice sprial.
  

![CircularPanel - Spiral](/images/CircularPanel%20-%20Spiral_1.png "CircularPanel - Spiral") 
  

Add 12 auto-placed elements, then three more using FixedAngle combined with an animation to produce a clock with hour, minute and second hands.
  

![CircularPanel - Clock](/images/CircularPanel%20-%20Clock_1.png "CircularPanel - Clock") 
  

Use an animation over StartAngle and EndAngle to produce an effect similar to opening a fan. 
  

![CircularPanel - Fan](/images/CircularPanel%20-%20Fan_1.png "CircularPanel - Fan")
  

Here’s the interesting code from the ArrangeOverride method on CircularPanel.
  

<font size="1"><font face="Verdana"><span style="color: blue">protected override </span><span style="color: #2b91af">Size </span>ArrangeOverride(<span style="color: #2b91af">Size </span>finalSize)
{
<span style="color: blue">int </span>numberOfVisibleChildren = InternalChildren  
                                                          .OfType&lt;<span style="color: #2b91af">UIElement</span>&gt;()  
                                                          .Count(u =&gt; u.Visibility != <span style="color: #2b91af">Visibility</span>.Collapsed   
                                                                            &amp;&amp; !GetFixedAngle(u).HasValue);
<span style="color: blue">if </span>(numberOfVisibleChildren == 0) <span style="color: blue">return </span>finalSize; </font></font><font size="1"><font face="Verdana"><span style="color: green">// Short circuit if there are no children
</span><span style="color: blue">int </span>currentChildPosition = 0;
<span style="color: blue">double </span>startArcAngle = StartAngle / 180 * <span style="color: #2b91af">Math</span>.PI;
<span style="color: blue">double </span>endArcAngle = EndAngle / 180 * <span style="color: #2b91af">Math</span>.PI;
<span style="color: blue">double </span>arcDelta;
<span style="color: blue">if </span>( <span style="color: #2b91af">Math</span>.Abs(startArcAngle-endArcAngle) &gt;= <span style="color: #2b91af">Math</span>.PI * 2 )
</font></font><font size="1"><font face="Verdana"><span style="color: green">// If we have a full circle then don't end the last element on the   
            // EndAngle because that would overlay the StartAngle.
</span>arcDelta = (endArcAngle - startArcAngle ) /  (<span style="color: blue">double</span>) numberOfVisibleChildren;
</font></font><span style="color: blue"><font size="1" face="Verdana">else
</font></span><font size="1"><font face="Verdana"><span style="color: green">// If we have less than a full circle then make sure we spread the   
            // elements with first and last on the start and end angles.
</span>arcDelta = (endArcAngle - startArcAngle) / ((<span style="color: blue">double</span>)numberOfVisibleChildren - 1);
<span style="color: blue">double </span>maxChildWidth = InternalChildren  
                                                .OfType&lt;<span style="color: #2b91af">UIElement</span>&gt;()  
                                                .Max( u =&gt; u.DesiredSize.Width );
<span style="color: blue">double </span>maxChildHeight = InternalChildren  
                                                 .OfType&lt;<span style="color: #2b91af">UIElement</span>&gt;()  
                                                .Max( u =&gt; u.DesiredSize.Height );
<span style="color: blue">double </span>radiusX = ( finalSize.Width - Padding.Left - Padding.Right - maxChildWidth ) / 2;
<span style="color: blue">double </span>radiusY = ( finalSize.Height - Padding.Top - Padding.Bottom - maxChildHeight ) / 2;
<span style="color: #2b91af">Point </span>midPoint = <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( radiusX + Padding.Left + maxChildWidth / 2,   
                                                radiusY + Padding.Top + maxChildHeight / 2);
<span style="color: blue">foreach </span>(<span style="color: #2b91af">UIElement </span>child <span style="color: blue">in </span>InternalChildren)
{
<span style="color: blue">var </span>childAngle = startArcAngle + arcDelta * currentChildPosition;
<span style="color: blue">double</span>? fixedAngle = GetFixedAngle(child);
<span style="color: blue">if </span>(fixedAngle.HasValue)
childAngle = fixedAngle.Value / 180 * <span style="color: #2b91af">Math</span>.PI;
<span style="color: blue">double </span>x = <span style="color: #2b91af">Math</span>.Cos( childAngle ) * radiusX * GetRadiusScaleX(child) +   
                            midPoint.X - child.DesiredSize.Width / 2;
<span style="color: blue">double </span>y = <span style="color: #2b91af">Math</span>.Sin( childAngle ) * radiusY * GetRadiusScaleY(child) +   
                            midPoint.Y - child.DesiredSize.Height / 2;
child.Arrange(<span style="color: blue">new </span><span style="color: #2b91af">Rect</span>(<span style="color: blue">new </span><span style="color: #2b91af">Point</span>(x, y), child.DesiredSize));
</font></font><font size="1"><font face="Verdana"><span style="color: green">// Ignore collapsed children and FixedAngle children.
</span><span style="color: blue">if </span>( child.Visibility != <span style="color: #2b91af">Visibility</span>.Collapsed &amp;&amp; !fixedAngle.HasValue )
currentChildPosition++;
}
<span style="color: blue">return </span>finalSize;
}</font></font>

<a href="http://11011.net/software/vspaste"></a>


Source code with simple sample application [here](http://www.spencen.com/Downloads/CircularPanel.zip). It’s a VS2010 solution/project but should be easy to converted to VS2008 – just remove the EasingFunctions in the sample app XAML.


