---
title: "Why DataTemplates Rock!"
date: 2008-08-02 15:45
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---

In the recent Screencasts I did on creating an Occasionally Connected app my user interface consisted of a ListBox and a few buttons. The ListBox was used to display the list of *Holidays* read from a locally cached database. The application has now evolved where that early prototype has become the initial &#8220;selection screen&#8221; for the HolidayPlanner application.
  

Here is the screen using a ResourceDictionary (Resources\Simple.xaml) with the bare minimum required to be usable:
  

![HolidaySelectionSimple](/images/HolidaySelectionSimple_2.jpg) 
  

And here&#8217;s the XAML behind it:
  

<font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">Window </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Class</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;ADOSyncSampleSP1.MainWindow&quot;
</span><span style="color: red">xmlns</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;
</span><span style="color: red">xmlns</span><span style="color: blue">:</span><span style="color: red">x</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;
</span><span style="color: red">xmlns</span><span style="color: blue">:</span><span style="color: red">dm</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;clr-namespace:HolidayPlanner.DataModel&quot;
</span><span style="color: red">xmlns</span><span style="color: blue">:</span><span style="color: red">cm</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;clr-namespace:Spencen.PresentationLayer.Commands&quot;
</span><span style="color: red">Title</span><span style="color: blue">=&quot;Occasionally Connected Application Demo&quot; </span><span style="color: red">Height</span><span style="color: blue">=&quot;581.783&quot; </span><span style="color: red">Width</span><span style="color: blue">=&quot;551.777&quot; </span><span style="color: red">Language</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;en-AU&quot;&gt;
</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">    &lt;</span><span style="color: #a31515">DockPanel </span><span style="color: red">Name</span><span style="color: blue">=&quot;dockPanel1&quot; </span><span style="color: red">VerticalAlignment</span><span style="color: blue">=&quot;Stretch&quot; </span><span style="color: red">LastChildFill</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;True&quot;&gt;
&lt;</span><span style="color: #a31515">TabControl </span><span style="color: red">DockPanel.Dock</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Top&quot;&gt;
&lt;</span><span style="color: #a31515">TabItem </span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Plan&quot;&gt;
&lt;</span><span style="color: #a31515">ToolBarTray</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ToolBar </span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Select Holiday&quot;&gt;
&lt;</span><span style="color: #a31515">Button </span><span style="color: red">cm</span><span style="color: blue">:</span><span style="color: red">CommandBinder.Command</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">NewCommand</span><span style="color: blue">}&quot;/</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">Button </span><span style="color: red">cm</span><span style="color: blue">:</span><span style="color: red">CommandBinder.Command</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">PropertiesCommand</span><span style="color: blue">}&quot;/</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ToolBar</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ToolBar </span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Configuration&quot;&gt;
&lt;</span><span style="color: #a31515">Button </span><span style="color: red">cm</span><span style="color: blue">:</span><span style="color: red">CommandBinder.Command</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">SecurityCommand</span><span style="color: blue">}&quot;/</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">Button </span><span style="color: red">cm</span><span style="color: blue">:</span><span style="color: red">CommandBinder.Command</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">NetworkSettingsCommand</span><span style="color: blue">}&quot;/</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">Button </span><span style="color: red">cm</span><span style="color: blue">:</span><span style="color: red">CommandBinder.Command</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">SettingsCommand</span><span style="color: blue">}&quot;/</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ToolBar</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ToolBarTray</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">TabItem</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TabItem </span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Experience&quot;&gt;
&lt;</span><span style="color: #a31515">ToolBarTray</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ToolBar </span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Select Holiday&quot; &gt;
&lt;</span><span style="color: #a31515">Button </span><span style="color: red">cm</span><span style="color: blue">:</span><span style="color: red">CommandBinder.Command</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">OpenCommand</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;/&gt;
&lt;/</span><span style="color: #a31515">ToolBar</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ToolBarTray</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">TabItem</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">TabItem </span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Review&quot;&gt;
&lt;</span><span style="color: #a31515">ToolBarTray</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ToolBar</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">Button </span><span style="color: red">cm</span><span style="color: blue">:</span><span style="color: red">CommandBinder.Command</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">SynchronizeCommand</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;/&gt;
&lt;/</span><span style="color: #a31515">ToolBar</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ToolBarTray</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">TabItem</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">TabControl</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ListBox </span><span style="color: red">Name</span><span style="color: blue">=&quot;listBox1&quot; </span><span style="color: red">ItemsSource</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">Holidays</span><span style="color: blue">}&quot; </span><span style="color: red">HorizontalAlignment</span><span style="color: blue">=&quot;Stretch&quot;   
                         </span><span style="color: red">ScrollViewer.CanContentScroll</span><span style="color: blue">=&quot;True&quot; </span><span style="color: red">ScrollViewer.VerticalScrollBarVisibility</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Visible&quot;&gt;
&lt;</span><span style="color: #a31515">ListBox.InputBindings</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">KeyBinding </span><span style="color: red">Command</span><span style="color: blue">=&quot;ApplicationCommands.Open&quot; </span><span style="color: red">Key</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Enter&quot;/&gt;</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">
&lt;/</span><span style="color: #a31515">ListBox.InputBindings</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ListBox.GroupStyle</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">GroupStyle </span><span style="color: red">HeaderTemplate</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">HolidayGroupHeader</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;/&gt;
&lt;/</span><span style="color: #a31515">ListBox.GroupStyle</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ListBox.ItemsPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">ItemsPanelTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">VirtualizingStackPanel </span><span style="color: red">Orientation</span><span style="color: blue">=&quot;Vertical&quot; </span><span style="color: red">VirtualizingStackPanel.VirtualizationMode</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Recycling&quot;/&gt;
&lt;/</span><span style="color: #a31515">ItemsPanelTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ListBox.ItemsPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ListBox</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">DockPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">Window</span></font></font><span style="color: blue"><font face="Verdana" size="1">&gt;</font>
</span>

<a href="http://11011.net/software/vspaste"></a><a href="http://11011.net/software/vspaste"></a>


What I love about WPF&#8217;s templating and styling system is that with a single line change to the App.xaml I can swap out the vanilla DataTemplates with something more appealing. So changing this:



<font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">Application </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Class</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;ADOSyncSampleSP1.App&quot;
  
&#160;&#160;&#160; </span><span style="color: red">xmlns</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=[http://schemas.microsoft.com/winfx/2006/xaml/presentation](http://schemas.microsoft.com/winfx/2006/xaml/presentation)
  
&#160;&#160;&#160; </span><span style="color: red">xmlns</span><span style="color: blue">:</span><span style="color: red">x</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=[http://schemas.microsoft.com/winfx/2006/xaml](http://schemas.microsoft.com/winfx/2006/xaml)
  
</span><span style="color: red">&#160;&#160;&#160; StartupUri</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Views/MainWindow.xaml&quot;&gt;
  
&#160;&#160;&#160; &lt;</span><span style="color: #a31515">Application.Resources</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">ResourceDictionary</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">ResourceDictionary.MergedDictionaries</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; **&lt;**</span>**<span style="color: #a31515">ResourceDictionary </span><span style="color: red">Source</span>**</font></font><span style="color: blue"><font face="Verdana" size="1">**=&quot;Resources/Simple.xaml&quot;/&gt;
  
**&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font></span><font size="1"><font face="Verdana"><span style="color: green"></span><span style="color: blue">&lt;/</span><span style="color: #a31515">ResourceDictionary.MergedDictionaries</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&#160;   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;/</span><span style="color: #a31515">ResourceDictionary</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
  
&#160;&#160;&#160; &lt;/</span><span style="color: #a31515">Application.Resources</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
  
&lt;/</span><span style="color: #a31515">Application</span><span style="color: blue">&gt;</span></font></font>



To this:

<font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">Application </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Class</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;ADOSyncSampleSP1.App&quot;
  
&#160;&#160;&#160; </span><span style="color: red">xmlns</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=[http://schemas.microsoft.com/winfx/2006/xaml/presentation](http://schemas.microsoft.com/winfx/2006/xaml/presentation)
  
&#160;&#160;&#160; </span><span style="color: red">xmlns</span><span style="color: blue">:</span><span style="color: red">x</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=[http://schemas.microsoft.com/winfx/2006/xaml](http://schemas.microsoft.com/winfx/2006/xaml)
  
</span><span style="color: red">&#160;&#160;&#160; StartupUri</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Views/MainWindow.xaml&quot;&gt;
  
&#160;&#160;&#160; &lt;</span><span style="color: #a31515">Application.Resources</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">ResourceDictionary</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">ResourceDictionary.MergedDictionaries</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
  
&#160;&#160;&#160; **&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;**</span>**<span style="color: #a31515">ResourceDictionary </span><span style="color: red">Source</span>**</font></font><span style="color: blue"><font face="Verdana" size="1">**=&quot;Resources/Standard.xaml&quot;/&gt;
  
**&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font></span><font size="1"><font face="Verdana"><span style="color: green"></span><span style="color: blue">&lt;/</span><span style="color: #a31515">ResourceDictionary.MergedDictionaries</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&#160;   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;/</span><span style="color: #a31515">ResourceDictionary</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
  
&#160;&#160;&#160; &lt;/</span><span style="color: #a31515">Application.Resources</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
  
&lt;/</span><span style="color: #a31515">Application</span><span style="color: blue">&gt;</span></font></font>


Changes the front screen as shown below without changing any of the behaviours (other than adding UI enhancements such as hot tracking etc.)



![HolidaySelectionStandard](/images/HolidaySelectionStandard_2.jpg) 



At this point I must admit I was feeling kinda proud of myself. But then two things happened to change this. 



Firstly I watched [DNR TV episode 115](http://blog.spencen.com/2008/06/25/billy-hollis-on-dnrtv.aspx) where Billy Hollis shows how WPF allows developers to bring new user interface paradigms into their applications. Now my own user interface begins to look a little bit like &#8220;lipstick on a pig&#8221;. I can&#8217;t help looking through the veil and seeing that same old toolbar and listbox from the Windows 95 era.



Secondly, a few weeks ago I attended the first day of CodeCampSA. During this a guy by the name of Alan Boldock gave a very entertaining talk on his experiences creating appealing user interfaces using WPF. During this presentation he showed off some quick samples of the work his team had been doing. I was sitting in the audience putting together some final tweaks on my new *Resources\Standard.xaml* resource dictionary. Again, it was somewhat demoralizing, I couldn&#8217;t help but think that I just hadn&#8217;t embraced the potential that WPF could offer.



There was nothing for it &#8211; I had to come to grips with my &#8220;inner-designer&#8221;. I tried coaxing my alter-ego out by wearing the closest thing I could find to a turtleneck sweater. Bravely I cracked open Expression Design and painstakingly put together something that I could export out as the new DataTemplate for the *Holiday* class. So I add another line to the App.xaml to include the new DataTemplate, and switched the ListBox to be Horizontal rather than Vertical.

<font size="1"><font face="Verdana"><span style="color: blue">&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">ResourceDictionary.MergedDictionaries</span></font></font><font size="1"><font face="Verdana"><span>&gt;
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;<font color="#800000">ResourceDictionary</font> </span><span style="color: red">Source</span></font></font><span style="color: blue"><font face="Verdana" size="1">=&quot;Resources/Standard.xaml&quot;/&gt;
  
**<font size="1"><font face="Verdana"><span>&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;ResourceDictionary </span><span style="color: red">Source</span></font></font><span style="color: blue"><font face="Verdana" size="1">=&quot;Resources/PhotoAlbum.xaml&quot;/&gt;</font></span>
  
**&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font></span><font size="1"><font face="Verdana"><span style="color: green"></span><span style="color: blue">&lt;/</span><span style="color: #a31515">ResourceDictionary.MergedDictionaries</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&#160;   
</span></font></font>


Here&#8217;s the end result:



![HolidaySelectionAlbum](/images/HolidaySelectionAlbum_2.jpg) 



Well, granted its not particularly artistic, nor innovative &#8211; but I still got a warm, fuzzy feeling seeing my new selection screen. [Or maybe that was just from the itchy old sweater? ![](http://blog.spencen.com/emoticons/smile.png)]


