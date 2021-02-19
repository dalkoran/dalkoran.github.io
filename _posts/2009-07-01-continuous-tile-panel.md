---
layout: post
title: "Continuous Tile Panel"
date: 2009-07-01 15:15
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---
<p dir="ltr">A couple of weeks ago someone suggested to me an idea for a particular type of layout panel. The idea was to be able to display a fixed set of tiles which are “wrapped” horizontally. So as you scroll horizontally the tiles will move off one edge of the panel and eventually re-appear on the other side. Think of how we commonly see the earth projected onto a rectangle – scrolling left or right to bring the particular geography into the centre of the screen. 
 <p style="margin-right: 0px" dir="ltr">I jumped into this without giving too much thought about how I would want to interact with the panel from an API perspective. For that reason its probably I’ve taken altogether the wrong approach but I thought I would post it up here anyway, ‘cause its unlikely I’ll take it any further than this proof of concept.
 

### Usage

 <p style="margin-right: 0px" dir="ltr">Define some XAML like so:
<a href="http://11011.net/software/vspaste"></a>

<font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">panels</span><span style="color: blue">:</span><span style="color: #a31515">TilePanel </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Name</span><span style="color: blue">="tilePanel" </span><span style="color: red">Height</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="170"
</span><span style="color: red">Rows</span><span style="color: blue">="3" </span><span style="color: red">Background</span><span style="color: blue">="LightYellow" </span><span style="color: red">ClipToBounds</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="True"
</span><span style="color: red">XOffset</span><span style="color: blue">="{</span><span style="color: #a31515">Binding </span><span style="color: red">ElementName</span><span style="color: blue">=xOffsetSlider,</span><span style="color: red">Path</span><span style="color: blue">=Value,</span><span style="color: red">Mode</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=OneWay}"
</span><span style="color: red">YOffset</span><span style="color: blue">="{</span><span style="color: #a31515">Binding </span><span style="color: red">ElementName</span><span style="color: blue">=yOffsetSlider,</span><span style="color: red">Path</span><span style="color: blue">=Value,</span><span style="color: red">Mode</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=OneWay}"
</span><span style="color: red">Scale</span><span style="color: blue">="{</span><span style="color: #a31515">Binding </span><span style="color: red">ElementName</span><span style="color: blue">=scaleSlider,</span><span style="color: red">Path</span><span style="color: blue">=Value,</span><span style="color: red">Mode</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=OneWay}"&gt;
&lt;</span><span style="color: #a31515">panels</span><span style="color: blue">:</span><span style="color: #a31515">TilePanel.Resources</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">Style </span><span style="color: red">TargetType</span><span style="color: blue">="{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}"&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">="FontSize" </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="32pt"/&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">="FontWeight" </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="Bold"/&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">="Padding" </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="3"/&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">="TextAlignment" </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="Center"/&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">="Background" </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="LightBlue"/&gt;
&lt;/</span><span style="color: #a31515">Style</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">panels</span><span style="color: blue">:</span><span style="color: #a31515">TilePanel.Resources</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">A</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">B</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">C</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">D</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">E</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">F</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">G</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">H</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">I</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">J</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">K</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">L</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">M</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">N</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">O</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">P</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">Q</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">R</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">S</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">T</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">U</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">V</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">W</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">X</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">Y</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">Z</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">1</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">2</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">3</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">4</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">5</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">6</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">7</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">8</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">9</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TextBlock</span><span style="color: blue">&gt;</span><span style="color: #a31515">0</span><span style="color: blue">&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">panels</span><span style="color: blue">:</span><span style="color: #a31515">TilePanel</span><span style="color: blue">&gt;</span></font></font></pre><a href="http://11011.net/software/vspaste"></a>

    
    This generates the following layout where each of the items is laid out in three rows.
    

    
    <a href="/images/Continuous%20Tile%20Panel%20-%20Letters%20at%20Start.png">![Continuous Tile Panel - Letters at Start](/images/Continuous%20Tile%20Panel%20-%20Letters%20at%20Start.png "Continuous Tile Panel - Letters at Start")</a> 
    

    
    Items that are beyond the right edge are actually wrapped back to the left hand side (by subtracting the total width of all columns). So that when we supply a horizontal offset we get the desired effect.
    

    
    <a href="/images/Continuous%20Tile%20Panel%20-%20Letters%20Shifted.png">![Continuous Tile Panel - Letters Shifted](/images/Continuous%20Tile%20Panel%20-%20Letters%20Shifted.png "Continuous Tile Panel - Letters Shifted")</a> 
    

    
    
    

    
    Note that in the example each column width is automatically calculated based on the widest element. Likewise row heights are calculated based on the tallest elements. Although this allows for different widths/heights per column/row its works well when all items are the same width and height.
    

    
    One example usage of this control would be as an ItemsPanel for an ItemsControl that displays tiled images that form a single scene. It so happens that [ICE](http://blog.spencen.com/2009/03/22/panoramas-using-microsoft-image-composite-editor-ice-and-hd-view.aspx) generates these types of tiles (at various zoom levels) for publishing its panoramas.
    <pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">ItemsControl </span><span style="color: red">ItemsSource</span><span style="color: blue">="{</span><span style="color: #a31515">Binding </span><span style="color: red">TileSet</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}"&gt;
&lt;</span><span style="color: #a31515">ItemsControl.ItemTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">DataTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">Border </span><span style="color: red">BorderThickness</span><span style="color: blue">="1" </span><span style="color: red">BorderBrush</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="LightGray"&gt;
&lt;</span><span style="color: #a31515">Grid</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">Image </span><span style="color: red">Source</span><span style="color: blue">="{</span><span style="color: #a31515">Binding </span><span style="color: red">ImagePath</span><span style="color: blue">}" </span><span style="color: red">Stretch</span><span style="color: blue">="Uniform" </span><span style="color: red">Height</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="Auto"/&gt;
</span><span style="color: green">                    </span><span style="color: blue">&lt;</span><span style="color: #a31515">TextBlock </span><span style="color: red">Margin</span><span style="color: blue">="4" </span><span style="color: red">TextAlignment</span><span style="color: blue">="Center" </span><span style="color: red">FontSize</span><span style="color: blue">="6pt" </span><span style="color: red">Foreground</span><span style="color: blue">="White" </span><span style="color: red">Opacity</span><span style="color: blue">="0.5" <br>                                     </span><span style="color: red">VerticalAlignment</span><span style="color: blue">="Center" </span><span style="color: red">HorizontalAlignment</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="Center"&gt;
&lt;</span><span style="color: #a31515">TextBlock.Text</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">MultiBinding </span><span style="color: red">StringFormat</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="{}{0},{1}" &gt;
&lt;</span><span style="color: #a31515">Binding </span><span style="color: red">Path</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="ColumnIndex"/&gt;
&lt;</span><span style="color: #a31515">Binding </span><span style="color: red">Path</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="RowIndex"/&gt;
&lt;/</span><span style="color: #a31515">MultiBinding</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">TextBlock.Text</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">TextBlock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">Grid</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">Border</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">DataTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ItemsControl.ItemTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ItemsControl.ItemsPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ItemsPanelTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">panels</span><span style="color: blue">:</span><span style="color: #a31515">TilePanel </span><span style="color: red">Margin</span><span style="color: blue">="10" </span><span style="color: red">Background</span><span style="color: blue">="LightYellow" </span><span style="color: red">ClipToBounds</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">="True"
</span><span style="color: red">Rows</span><span style="color: blue">="{</span><span style="color: #a31515">Binding </span><span style="color: red">TileSet</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">.RowCount}"
</span><span style="color: red">XOffset</span><span style="color: blue">="{</span><span style="color: #a31515">Binding </span><span style="color: red">ElementName</span><span style="color: blue">=xOffsetSlider,</span><span style="color: red">Path</span><span style="color: blue">=Value,</span><span style="color: red">Mode</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=OneWay}"
</span><span style="color: red">YOffset</span><span style="color: blue">="{</span><span style="color: #a31515">Binding </span><span style="color: red">ElementName</span><span style="color: blue">=yOffsetSlider,</span><span style="color: red">Path</span><span style="color: blue">=Value,</span><span style="color: red">Mode</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=OneWay}"
</span><span style="color: red">Scale</span><span style="color: blue">="{</span><span style="color: #a31515">Binding </span><span style="color: red">ElementName</span><span style="color: blue">=scaleSlider,</span><span style="color: red">Path</span><span style="color: blue">=Value,</span><span style="color: red">Mode</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=OneWay}"/&gt;
&lt;/</span><span style="color: #a31515">ItemsPanelTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ItemsControl.ItemsPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ItemsControl</span><span style="color: blue">&gt;</span></font></font></pre>

    
    <a href="/images/Continuous%20Tile%20Panel%20-%20IC.jpg">![Continuous Tile Panel - ICE](/images/Continuous%20Tile%20Panel%20-%20ICE.jpg "Continuous Tile Panel - ICE")</a> 
    

    
    &nbsp;
    

    
    ### Code
    
    

    
    To load the ICE images into a collection of “Tiles” I wrote the following.
    <pre class="code"> <font size="1"><font face="Verdana">   <span style="color: blue">public static class </span></font></font><font size="1"><font face="Verdana"><span style="color: #2b91af">TileLoader
</span>{
<span style="color: blue">public static </span><span style="color: #2b91af">TileSet </span>LoadFromFolder( <span style="color: blue">string </span>path, <span style="color: blue">int </span>zoomLevel )
{
<span style="color: blue">var </span>result = <span style="color: blue">new </span><span style="color: #2b91af">TileSet</span>(path, zoomLevel);
<span style="color: blue">if </span>( !<span style="color: #2b91af">Directory</span>.Exists( path ) )
<span style="color: blue">throw new </span><span style="color: #2b91af">ArgumentException</span>( <span style="color: #a31515">"The specified tile folder does not exist."</span>, <span style="color: #a31515">"path" </span>);
<span style="color: blue">if </span>( zoomLevel &lt; 0 )
<span style="color: blue">throw new </span><span style="color: #2b91af">ArgumentException</span>( <span style="color: #a31515">"The zoom level must be &gt;= 0."</span>, <span style="color: #a31515">"zoomLevel" </span>);
<span style="color: #2b91af">DirectoryInfo </span>tileLevelFolder;
</font></font><font size="1"><font face="Verdana"><span style="color: blue">try
</span>{
<span style="color: blue">var </span>rootFolder = <span style="color: blue">new </span><span style="color: #2b91af">DirectoryInfo</span>( path );
<span style="color: blue">var </span>tileFolder = <span style="color: blue">new </span><span style="color: #2b91af">DirectoryInfo</span>( <span style="color: #2b91af">Path</span>.Combine( path, <span style="color: #a31515">"tiles" </span>) );
tileLevelFolder = <span style="color: blue">new </span><span style="color: #2b91af">DirectoryInfo</span>( <span style="color: #2b91af">Path</span>.Combine( tileFolder.FullName, <span style="color: #a31515">"l_" </span>+ zoomLevel ) );
}
<span style="color: blue">catch </span>( System.IO.<span style="color: #2b91af">IOException </span>ex )
{
<span style="color: blue">throw new </span><span style="color: #2b91af">ArgumentException</span>( <br>                    <span style="color: blue">string</span>.Format( <span style="color: #a31515">"The tile set specified by the path {0} is corrupt."</span>, path ), <span style="color: #a31515">"path"</span>, ex );
}
<span style="color: blue">var </span>column = 0;
</font></font><font size="1"><font face="Verdana"><span style="background: #fbfffb; color: green">// Order the directories by numerical ascending, e.g. c_3 before c_21
</span>            <span style="color: blue">foreach </span>( <span style="color: blue">var </span>columnFolder <span style="color: blue">in </span>tileLevelFolder.GetDirectories( <span style="color: #a31515">"c_*" </span>)<br>                                                          .OrderBy( d =&gt; <span style="color: blue">int</span>.Parse( d.Name.Substring( 2 ) ) ) )
{
<span style="color: blue">var </span>row = 0;
<span style="color: blue">foreach </span>( <span style="color: blue">var </span>imageFile <span style="color: blue">in </span>columnFolder.GetFiles( <span style="color: #a31515">"tile_*.wdp" </span>)<br>                                                          .OrderBy( i =&gt; <span style="color: blue">int</span>.Parse( i.Name.Substring( 5, i.Name.IndexOf( <span style="color: #a31515">'.' </span>) - 5 ) ) ) )
{
<span style="color: blue">var </span>tile = <span style="color: blue">new </span><span style="color: #2b91af">Tile</span>() { ColumnIndex = column, RowIndex = row, ImagePath = imageFile.FullName };
result.Add( tile );
row++;
<span style="color: blue">if </span>( result.RowCount &lt; row )
result.RowCount = row;
}
column++;
}
<span style="color: blue">return </span>result;
}
}
[<span style="color: #2b91af">DebuggerDisplay</span>(<span style="color: #a31515">"Column: {ColumnIndex}, Row: {RowIndex}, {ImagePath}"</span>)]
<span style="color: blue">public class </span></font></font><font size="1"><font face="Verdana"><span style="color: #2b91af">Tile
</span>{
<span style="color: blue">public string </span>ImagePath { <span style="color: blue">get</span>; <span style="color: blue">set</span>; }
<span style="color: blue">public int </span>ColumnIndex { <span style="color: blue">get</span>; <span style="color: blue">set</span>; }
<span style="color: blue">public int </span>RowIndex { <span style="color: blue">get</span>; <span style="color: blue">set</span>; }
}
<span style="color: blue">public class </span><span style="color: #2b91af">TileSet </span>: <span style="color: #2b91af">List</span>&lt;<span style="color: #2b91af">Tile</span>&gt;
{
<span style="color: blue">public </span>TileSet( <span style="color: blue">string </span>path, <span style="color: blue">int </span>zoomLevel )
{
Path = path;
ZoomLevel = zoomLevel;
}
<span style="color: blue">public string </span>Path { <span style="color: blue">get</span>; <span style="color: blue">protected set</span>; }
<span style="color: blue">public int </span>ZoomLevel { <span style="color: blue">get</span>; <span style="color: blue">protected set</span>; }
<span style="color: blue">public int </span>RowCount { <span style="color: blue">get</span>; <span style="color: blue">set</span>; }
}</font></font></pre><a href="http://11011.net/software/vspaste"></a>

    
    The panel’s arrange override code is as follows:
    <pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">protected override </span><span style="color: #2b91af">Size </span>ArrangeOverride(<span style="color: #2b91af">Size </span>finalSize)
{
<span style="color: blue">int </span>row = 0;
<span style="color: blue">int </span>column = 0;
<span style="color: blue">double </span>fullWidth = _columnWidths.Sum();
<span style="color: blue">double </span>fullHeight = _rowHeights.Sum();
<span style="color: blue">int </span>maxColumns = (<span style="color: blue">int</span>) <span style="color: #2b91af">Math</span>.Ceiling( (<span style="color: blue">double</span>) finalSize.Width / (<span style="color: blue">double</span>) _maxChildWidth);
<span style="color: blue">double </span>x = -( XOffset * _maxChildWidth );
<span style="color: blue">double </span>y = - YOffset;
<span style="color: blue">foreach </span>(<span style="color: #2b91af">UIElement </span>child <span style="color: blue">in </span>Children)
{
x = x % fullWidth;
<span style="color: blue">if </span>(x &gt;= <span style="color: #2b91af">Math</span>.Min(_maxChildWidth * maxColumns, fullWidth - _maxChildWidth + 1))
x -= fullWidth;
<span style="color: blue">else if </span>(x &lt; - _maxChildWidth)
x += fullWidth;
<span style="color: blue">if </span>( x &gt; -_maxChildWidth &amp;&amp; x &lt;= _maxChildWidth * maxColumns &amp;&amp;
y &gt; -_maxChildHeight &amp;&amp; y &lt;= fullHeight )
{
child.Arrange( <br>                 <span style="color: blue">new </span><span style="color: #2b91af">Rect</span>( <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( x * Scale, y * Scale ), <br>                 <span style="color: blue">new </span><span style="color: #2b91af">Size</span>( child.DesiredSize.Width * Scale, child.DesiredSize.Height * Scale ) ) );
child.Visibility = <span style="color: #2b91af">Visibility</span>.Visible;
}
</font></font><font size="1"><font face="Verdana"><span style="color: blue">else
</span>{
child.Visibility = <span style="color: #2b91af">Visibility</span>.Hidden;
child.Arrange( <span style="color: blue">new </span><span style="color: #2b91af">Rect</span>( <span style="color: blue">new </span><span style="color: #2b91af">Point</span>( 0, 0 ), <span style="color: blue">new </span><span style="color: #2b91af">Size</span>( child.DesiredSize.Width * Scale, child.DesiredSize.Height * Scale ) ) );
}
y += _rowHeights[ row ];
row = ( row + 1 ) % Rows;
<span style="color: blue">if </span>( row == 0 )
{
x += _columnWidths[ column ]; </font></font><font size="1"><font face="Verdana"><span style="background: #fbfffb; color: green">//column * _childWidth ;
</span>            y = -YOffset ;
column++;
}
}
</font></font><font size="1"><font face="Verdana"><span style="background: #fbfffb; color: green">//RenderTransform = new ScaleTransform(Scale, Scale, 0.0, YOffset);
</span>    <span style="color: blue">return </span>finalSize;
}</font></font></pre><a href="http://11011.net/software/vspaste"></a>

    
    This is far from complete – sample project can be found [here](http://www.spencen.com/Downloads/TilePanelTest.zip). [It’s a VS2010 project but should be easy enough to re-assemble for VS2008.]
    <pre class="code">&nbsp;


