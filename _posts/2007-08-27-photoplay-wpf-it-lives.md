---
title: "PhotoPlay - WPF - It Lives!"
date: 2007-08-27 02:59
author: spencen
comments: true
categories: [.NET, Development]
tags: []
---


Ok - so tonight I finally had the courage to install Visual Studio 2008 Beta 2 on my relatively new and until now pristinely clean development machine. The install process was the standard tried and true Visual Studio install engine and completed slowly but without any problems (only one reboot required).
 

So I immediately fire up Visual Studio 2008 - choose the C# preferences which gives me all the wrong keyboard shortcuts (I can never remember which is which). Create a new WPF application with solution folder. The add a new WPF user control library to the project. So far so good.
 

Renaming UserControl1 to PhotoPage and changing the namespaces turns out to be as bad as renaming in Visual Studio 2005. Have to rename the file, class, namespace and XAML all independently.
 

First impressions:
 

*   XAML editor hasn't immediately crashed - way more stable than previous builds!  XAML editor is almost real-time and is only a little slow to load - way faster than previous builds!  WPF has changed everything - even finding equivalent of *BackColor* to create a coloured background takes time. At least *Fill* is spelt correctly :-p  XAML Intellisense is great for auto-complete - but where are the help tooltips?  Hmm... appears Shapes have a *Fill* and controls have a *Background*.  Don't click on the helpful sounding message telling you the assembly has changed and XAML needs to be refreshed. Seems to crash Visual Studio every time ![](http://blog.spencen.com/emoticons/sad.png) 

My first attempt at a XAML only approach:
 

PhotoClass : UserControl


<font size="2"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">UserControl</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Class</span></font><font size="2"><span style="color: rgb(0,0,255)">="Spencen.Windows.Controls.Photo"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span></font><font size="2"><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">x</span></font><font size="2"><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml"
</span>   <span style="color: rgb(255,0,0)"> Width</span></font><font size="2"><span style="color: rgb(0,0,255)">="300"&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">StackPanel</span></font><font size="2"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Border</span><span style="color: rgb(255,0,0)"> Name</span><span style="color: rgb(0,0,255)">="OuterFrame"</span><span style="color: rgb(255,0,0)"> CornerRadius</span></font><font size="2"><span style="color: rgb(0,0,255)">="5"&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Border.Background</span></font><font size="2"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">LinearGradientBrush</span><span style="color: rgb(255,0,0)"> Opacity</span></font><font size="2"><span style="color: rgb(0,0,255)">="0.8"&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">="Azure"</span><span style="color: rgb(255,0,0)"> Offset</span></font><font size="2"><span style="color: rgb(0,0,255)">="0.0"/&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">GradientStop</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">="CadetBlue"</span><span style="color: rgb(255,0,0)"> Offset</span></font><font size="2"><span style="color: rgb(0,0,255)">="1.0"/&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">LinearGradientBrush</span></font><font size="2"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Border.Background</span></font><font size="2"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Border</span><span style="color: rgb(255,0,0)"> Name</span><span style="color: rgb(0,0,255)">="InnerFrame"</span><span style="color: rgb(255,0,0)"> Background</span><span style="color: rgb(0,0,255)"> ="White"</span><span style="color: rgb(255,0,0)"> BorderBrush</span><span style="color: rgb(0,0,255)">="Black"</span><span style="color: rgb(255,0,0)"> <br>                Margin</span></font><font size="2"><span style="color: rgb(0,0,255)">="6,6,6,6"&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Image</span><span style="color: rgb(255,0,0)"> Name</span><span style="color: rgb(0,0,255)">="image2"</span><span style="color: rgb(255,0,0)"> Margin</span></font><font size="2"><span style="color: rgb(0,0,255)">="1, 1, 1, 1"&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Image.Source</span></font><font size="2"><span style="color: rgb(0,0,255)">&gt;<br></span><span style="color: rgb(0,0,255)">                        </span></font><font size="2"><span style="color: rgb(163,21,21)">F:\Pictures\Camera Photos\2007\Mount Hotham\DSC06994.JPG<br></span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Image.Source</span></font><font size="2"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Image</span></font><font size="2"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Border</span></font><font size="2"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Border</span></font><font size="2"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">StackPanel</span></font><font size="2"><span style="color: rgb(0,0,255)">&gt;
&lt;/</span><span style="color: rgb(163,21,21)">UserControl</span><span style="color: rgb(0,0,255)">&gt;</span></font></pre><a href="http://11011.net/software/vspaste"></a>

    
    PhotoPlayWindow : Window
    <pre class="code"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Window</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Class</span><span style="color: rgb(0,0,255)">="Spencen.Windows.PhotoPlayWindow"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">x</span><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">p</span><span style="color: rgb(0,0,255)">="clr-namespace:Spencen.Windows.Controls;assembly=Spencen.Windows.Controls.Photo"
</span>   <span style="color: rgb(255,0,0)"> Title</span><span style="color: rgb(0,0,255)">="PhotoPlayWPF"</span><span style="color: rgb(255,0,0)"> Height</span><span style="color: rgb(0,0,255)">="428"</span><span style="color: rgb(255,0,0)"> Width</span><span style="color: rgb(0,0,255)">="619"&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">StackPanel</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Slider</span><span style="color: rgb(255,0,0)"> Name</span><span style="color: rgb(0,0,255)">="scaleSlider"</span><span style="color: rgb(255,0,0)"> Height</span><span style="color: rgb(0,0,255)">="21"</span><span style="color: rgb(255,0,0)"> Width</span><span style="color: rgb(0,0,255)">="100"</span><span style="color: rgb(255,0,0)"> <br>             </span><span style="color: rgb(255,0,0)">Minimum</span><span style="color: rgb(0,0,255)">="0.1"</span><span style="color: rgb(255,0,0)"> Maximum</span><span style="color: rgb(0,0,255)">="5.0"</span><span style="color: rgb(255,0,0)"> Value</span><span style="color: rgb(0,0,255)">="1.0"/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Slider</span><span style="color: rgb(255,0,0)"> Name</span><span style="color: rgb(0,0,255)">="angleSlider"</span><span style="color: rgb(255,0,0)"> Height</span><span style="color: rgb(0,0,255)"> ="21"</span><span style="color: rgb(255,0,0)"> Width</span><span style="color: rgb(0,0,255)">="100"</span><span style="color: rgb(255,0,0)"> <br>             Minimum</span><span style="color: rgb(0,0,255)">="-180.0"</span><span style="color: rgb(255,0,0)"> Maximum</span><span style="color: rgb(0,0,255)">="180.0"</span><span style="color: rgb(255,0,0)"> Value</span><span style="color: rgb(0,0,255)">="0.0"/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">p</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Photo</span><span style="color: rgb(255,0,0)"> Height</span><span style="color: rgb(0,0,255)">="230"</span><span style="color: rgb(255,0,0)"> Width</span><span style="color: rgb(0,0,255)">="300"</span><span style="color: rgb(255,0,0)"> Name</span><span style="color: rgb(0,0,255)">="testPhoto"&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">p</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Photo.LayoutTransform</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TransformGroup</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ScaleTransform</span><span style="color: rgb(255,0,0)"> ScaleX</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> ElementName</span><span style="color: rgb(0,0,255)">=scaleSlider,</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=Value}"</span><span style="color: rgb(255,0,0)"> <br>                        ScaleY</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> ElementName</span><span style="color: rgb(0,0,255)">=scaleSlider,</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=Value}"/&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">RotateTransform</span><span style="color: rgb(255,0,0)"> Angle</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> ElementName</span><span style="color: rgb(0,0,255)">=angleSlider,</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=Value}"</span><span style="color: rgb(255,0,0)"> <br>                        CenterX</span><span style="color: rgb(0,0,255)">="150"</span><span style="color: rgb(255,0,0)"> CenterY</span><span style="color: rgb(0,0,255)">="150"/&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">TransformGroup</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">p</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Photo.LayoutTransform</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">p</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Photo</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">StackPanel</span><span style="color: rgb(0,0,255)">&gt;
&lt;/</span><span style="color: rgb(163,21,21)">Window</span><span style="color: rgb(0,0,255)">&gt;
</span>



![image](/images/image_2.png)


