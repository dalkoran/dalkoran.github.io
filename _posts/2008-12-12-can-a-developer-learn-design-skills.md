---
title: "Can a developer learn design skills?"
date: 2008-12-12 14:50
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


Over the years I have the opportunity to build a fair number of user interfaces for various software applications. Whilst some have been more effective than others I’ve always struggled to create what I’d consider to be a truly compelling experience.
  

I find it very difficult to “dream up” user interfaces that are both functional and unique. It’s the starting point – the *blank canvas* if you like that really stumps me. If I concentrate on small individual portions of the interface I’ve trained myself to be fairly good at decomposing it into graphic primitives in order to be able to duplicate the effect. I find that this is actually an exercise in consciously processing what I’m seeing – rather than relying on the assumptions being made unconsciously.
  

Some examples:
  

*   What makes something look three dimensional? 
*   How are surfaces made to have either a matt or shiny finish? 
*   What characteristics does a glass surface have? What about a mirrored surface? 
*   How can an indicator light appear to realistically switch on and off – taking into account glow and shadows?   

When boiled down to graphic primitives most of these become fairly simple – although not altogether obvious at first. I’ve found that when working for an extended period of time to achieve a particular effect its possible to un-train your unconscious so that you actually process the primitives rather than the desired effect. This is pretty frustrating because for the next few days when I look at, say the glowing indicator light, what I actually see are a bunch of linear and radial gradients. Still it wears off after a few days and then, if I’ve done it properly, they starting looking like indicator lights again.
  

Last night whilst experimenting with some glass effects I came across this website: [http://www.bestechvideos.com/tag/screencasters](http://www.bestechvideos.com/tag/screencasters). It has a number of video tutorials that explain how to achieve common visual effects such as <a href="http://www.bestechvideos.com/2008/07/05/screencasters-episode-017-glass-button-effect-redux" target="_blank">shiny buttons</a>, <a href="http://www.bestechvideos.com/2008/10/01/screencasters-episode-072-glass-panels" target="_blank">glass panels</a> and the like. The tool they use is InkScape which appears to be a vector drawing program running on Linux. However, I found its pretty easy to translate the effects into XAML, especially with the help of Blend.
  

<font face="Verdana" size="1">  </font><font size="1"><font face="Verdana"><span style="color: rgb(139,0,139)">&lt;Grid&gt;
&lt;Image </span><span style="color: rgb(255,0,0)">Source</span><span style="color: rgb(0,0,255)">=&quot;F:\Pictures\Textures\Image_After_Stock_Images\b1crystal001.jpg&quot; </span><span style="color: rgb(255,0,0)">Stretch</span><span style="color: rgb(0,0,255)">=&quot;None&quot;</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(139,0,139)">/&gt;
&lt;Border </span><span style="color: rgb(255,0,0)">BorderBrush</span><span style="color: rgb(0,0,255)">=&quot;#FF040404&quot; </span><span style="color: rgb(255,0,0)">BorderThickness</span><span style="color: rgb(0,0,255)">=&quot;1&quot; </span><span style="color: rgb(255,0,0)">CornerRadius</span><span style="color: rgb(0,0,255)">=&quot;8&quot; </span><span style="color: rgb(255,0,0)">Width</span><span style="color: rgb(0,0,255)">=&quot;135&quot; </span><span style="color: rgb(255,0,0)">Height</span><span style="color: rgb(0,0,255)">=&quot;160&quot; </span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(139,0,139)">&gt;
&lt;Border </span><span style="color: rgb(255,0,0)">Width</span><span style="color: rgb(0,0,255)">=&quot;Auto&quot; </span><span style="color: rgb(255,0,0)">Height</span><span style="color: rgb(0,0,255)">=&quot;Auto&quot; </span><span style="color: rgb(255,0,0)">Background</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;#60A0A0F0&quot;   
                  </span><span style="color: rgb(255,0,0)">BorderBrush</span><span style="color: rgb(0,0,255)">=&quot;#A3808080&quot; </span><span style="color: rgb(255,0,0)">BorderThickness</span><span style="color: rgb(0,0,255)">=&quot;5&quot; </span><span style="color: rgb(255,0,0)">CornerRadius</span><span style="color: rgb(0,0,255)">=&quot;4&quot;</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(139,0,139)">&gt;
&lt;Border.Effect&gt;
&lt;DropShadowEffect </span><span style="color: rgb(255,0,0)">BlurRadius</span><span style="color: rgb(0,0,255)">=&quot;20&quot; </span><span style="color: rgb(255,0,0)">ShadowDepth</span><span style="color: rgb(0,0,255)">=&quot;5&quot; </span><span style="color: rgb(255,0,0)">Opacity</span><span style="color: rgb(0,0,255)">=&quot;0.4&quot;</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(139,0,139)">/&gt;
&lt;/Border.Effect&gt;
&lt;Path </span><span style="color: rgb(255,0,0)">Stretch</span><span style="color: rgb(0,0,255)">=&quot;Fill&quot; </span><span style="color: rgb(255,0,0)">Margin</span><span style="color: rgb(0,0,255)">=&quot;0,0,0,70&quot; </span><span style="color: rgb(255,0,0)">VerticalAlignment</span><span style="color: rgb(0,0,255)">=&quot;Top&quot; </span><span style="color: rgb(255,0,0)">Height</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;Auto&quot;
</span><span style="color: rgb(255,0,0)">Data</span></font></font><span style="color: rgb(0,0,255)"><font face="Verdana" size="1">=&quot;M256,0
</font></span><span style="color: rgb(139,0,139)"><font face="Verdana" color="#0000ff" size="1">C256,0 260,0, 260,4
L260,80
C230.46646,92.718311 185.26848,100.83087 129.63841,101.82405
95.868918,102.42695 58.948563,96.301248 27.904232,88.28635
20.143162,86.282627 12.749348,84.160824 5.863842,81.996578
L0,80 0,4
C0,4 0,0 4,0 z</font></span><span style="color: rgb(0,0,255)"><font face="Verdana" size="1">&quot;&gt;
</font></span><font size="1"><font face="Verdana"><span style="color: rgb(139,0,139)">&lt;Path.Fill&gt;
&lt;LinearGradientBrush </span><span style="color: rgb(255,0,0)">EndPoint</span><span style="color: rgb(0,0,255)">=&quot;0.5,1&quot; </span><span style="color: rgb(255,0,0)">StartPoint</span><span style="color: rgb(0,0,255)">=&quot;0.5,0&quot;</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(139,0,139)">&gt;
&lt;GradientStop </span><span style="color: rgb(255,0,0)">Color</span><span style="color: rgb(0,0,255)">=&quot;#CFFFFFFF&quot; </span><span style="color: rgb(255,0,0)">Offset</span><span style="color: rgb(0,0,255)">=&quot;0&quot;</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(139,0,139)">/&gt;
&lt;GradientStop </span><span style="color: rgb(255,0,0)">Color</span><span style="color: rgb(0,0,255)">=&quot;#00FFFFFF&quot; </span><span style="color: rgb(255,0,0)">Offset</span><span style="color: rgb(0,0,255)">=&quot;1&quot;</span></font></font><span style="color: rgb(139,0,139)"><font face="Verdana" size="1">/&gt;
&lt;/LinearGradientBrush&gt;
&lt;/Path.Fill&gt;
&lt;/Path&gt;
&lt;/Border&gt;
&lt;/Border&gt;
&lt;/Grid&gt;</font></span></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    This is a really simple example and you can tell I did the Path using Blend because of the ridiculous numbers (though I did clean up the edges). This XAML generates the following, with and without the background Image:
    

    
    ![Glass Panel Effect](/images/Glass%20Panel%20Effect_3.png "Glass Panel Effect")&#160;&#160;&#160; ![Glass Panel No Background](/images/Glass%20Panel%20No%20Background_3.png "Glass Panel No Background")
    

    
    Here’s another glassy effect that I was using for an update to <a href="http://blog.spencen.com/2008/11/30/a-simple-word-puzzle.aspx" target="_blank">my word puzzle</a> UI. This one just uses a linear gradient brush on a diagonal.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">LinearGradientBrush</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Key</span><span style="color: rgb(0,0,255)">=&quot;ShimmerBrush&quot;</span><span style="color: rgb(255,0,0)"> StartPoint</span><span style="color: rgb(0,0,255)">=&quot;0,0&quot;</span><span style="color: rgb(255,0,0)"> EndPoint</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;1,1&quot;&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">=&quot;#40FFFFFF&quot;</span><span style="color: rgb(255,0,0)"> Offset</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;0&quot;/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">=&quot;#40FFFFFF&quot;</span><span style="color: rgb(255,0,0)"> Offset</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;0.25&quot;/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">=&quot;#80FFFFFF&quot;</span><span style="color: rgb(255,0,0)"> Offset</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;0.3&quot;/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">=&quot;#80FFFFFF&quot;</span><span style="color: rgb(255,0,0)"> Offset</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;0.5&quot;/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">=&quot;#40FFFFFF&quot;</span><span style="color: rgb(255,0,0)"> Offset</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;0.55&quot;/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">=&quot;#40FFFFFF&quot;</span><span style="color: rgb(255,0,0)"> Offset</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;1&quot;/&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">LinearGradientBrush</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">LinearGradientBrush</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Key</span><span style="color: rgb(0,0,255)">=&quot;GlassEdge&quot;</span><span style="color: rgb(255,0,0)"> StartPoint</span><span style="color: rgb(0,0,255)">=&quot;0,0&quot;</span><span style="color: rgb(255,0,0)"> EndPoint</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;1,1&quot;&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Offset</span><span style="color: rgb(0,0,255)">=&quot;0&quot;</span><span style="color: rgb(255,0,0)"> Color</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;#80808080&quot;/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Offset</span><span style="color: rgb(0,0,255)">=&quot;0.3&quot;</span><span style="color: rgb(255,0,0)"> Color</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;#80FFFFFF&quot;/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Offset</span><span style="color: rgb(0,0,255)">=&quot;0.5&quot;</span><span style="color: rgb(255,0,0)"> Color</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;#80FFFFFF&quot;/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Offset</span><span style="color: rgb(0,0,255)">=&quot;1&quot;</span><span style="color: rgb(255,0,0)"> Color</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;#80606060&quot;/&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">LinearGradientBrush</span><span style="color: rgb(0,0,255)">&gt;</span></font></font>

<a href="http://11011.net/software/vspaste"></a>


&#160;![Word Puzzle on Wood](/images/Word%20Puzzle%20on%20Wood_5.png "Word Puzzle on Wood")


