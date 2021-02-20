---

title: "Updated RibbonBar"
date: 2008-03-24 15:51
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


Update to RibbonBar - is now a separate Resource file. Simply merge into the resource dictionary and use the RibbonBarStyle on the TabControl you want to look like a Ribbon. The sample below can be copied direct into XAML viewer (e.g. Kaxaml). This is a XAML only version so doesn't contain any of the command binding stuff, curvy tab headers etc.
 <div>

<span style="color: #0000ff">&lt;</span><span style="color: #800000">Page</span>
<span style="color: #ff0000">xmlns</span><span style="color: #0000ff">="http://schemas.microsoft.com/winfx/2006/xaml/presentation"</span>
<span style="color: #ff0000">xmlns:x</span><span style="color: #0000ff">="http://schemas.microsoft.com/winfx/2006/xaml"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Page.Resources</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">ResourceDictionary</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">ResourceDictionary.MergedDictionaries</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">ResourceDictionary</span> <span style="color: #ff0000">Source</span><span style="color: #0000ff">="http://www.spencen.com/downloads/RibbonBarResource.xaml"</span><span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">ResourceDictionary.MergedDictionaries</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">ResourceDictionary</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Page.Resources</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">StackPanel</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">TabControl</span> <span style="color: #ff0000">Style</span><span style="color: #0000ff">="{StaticResource RibbonBarStyle}"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">TabItem</span> <span style="color: #ff0000">Header</span><span style="color: #0000ff">="Tab 1"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">ToolBarTray</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">ToolBar</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Button</span> <span style="color: #ff0000">Tag</span><span style="color: #0000ff">="http://www.spencen.com/downloads/Button1_48.png"</span><span style="color: #0000ff">&gt;</span>Button 1<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Button</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Button</span> <span style="color: #ff0000">Tag</span><span style="color: #0000ff">="http://www.spencen.com/downloads/Button1_24.png"</span><span style="color: #0000ff">&gt;</span>Button 2<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Button</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Button</span> <span style="color: #ff0000">Tag</span><span style="color: #0000ff">="http://www.spencen.com/downloads/Button1_24.png"</span><span style="color: #0000ff">&gt;</span>Button 2.1<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Button</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">ToolBar</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">ToolBarTray</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">TabItem</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">TabItem</span> <span style="color: #ff0000">Header</span><span style="color: #0000ff">="Tab 2"</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">ToolBarTray</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">ToolBar</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Button</span><span style="color: #0000ff">&gt;</span>Button 3<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Button</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">Button</span><span style="color: #0000ff">&gt;</span>Button 4<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Button</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">ComboBox</span> <span style="color: #ff0000">Text</span><span style="color: #0000ff">="ComboBox 1"</span> <span style="color: #ff0000">Width</span><span style="color: #0000ff">="80"</span><span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">ToolBar</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">ToolBarTray</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">TabItem</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">TabControl</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">StackPanel</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">Page</span><span style="color: #0000ff">&gt;</span>
</div>


Grab the RibonBarResource.xaml from [here](http://www.spencen.com/downloads/RibbonBarResource.xaml).


