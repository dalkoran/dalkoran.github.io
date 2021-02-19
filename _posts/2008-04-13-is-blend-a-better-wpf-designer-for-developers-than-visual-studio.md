---
layout: post
title: "Is Blend a better WPF designer for developers than Visual Studio?"
date: 2008-04-13 13:43
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


For ages now I've had some XAML that consistently generates error messages in the Visual Studio designer. I've found this most frustrating because I was never sure quite what was wrong with the XAML. Compiling and running the application and it worked exactly how it was intended. But the designer simply refused to display it - instead giving this standard message.
 

![Cider Invalid XAML Message](/images/Cider%20Invalid%20XAML%20Message_1.png)&nbsp;
 

In the Error List I find:
 

![Cider Invalid XAML Error](/images/Cider%20Invalid%20XAML%20Error_1.png) 
 

Huh? Sure indexes "Must be non-negative and less than the size of the collection." that makes sense. But which index? Line '1' Position '616' simply refers to the DataTemplate definition - but the whole template is considered "in error".
 

The application runs and renders perfectly. It only happens with DataTemplates and I get squiggly lines on the entire DataTemplate so tracking down the exact cause is difficult. Something to do with data-binding is about as close as I got before deciding that I could live without the preview (since I don't actually use the designer for editing).
 

So last night - inspired by some of the [latest Mix08 videos](http://windowsclient.net/learn/presentations.aspx#MIX+2008+Presentations) up on [WindowsClient.NET](http://www.windowsclient.net/) I decided to download and try the latest [Expression Blend 2.5 Preview](http://www.microsoft.com/expression/products/download.aspx?key=blend2dot5). This last time I used Blend was a pre release 1.0 Beta - and I have to say this program has come a long way since then. My initial impression on the v2.5 Preview are very positive. It's got a whole heap of features that I wish the Visual Studio designer included:
 

1.  Resource Viewer!  Binding Editor  Better XAML validation 

Better XAML Validation? Yep - take for instance problem I was having with my DataTemplates. Loading this UserControl into Blend I was disappointed to find that it also didn't render. But lo-and-behold it included errors messages indicating its dislike of my method of binding. Must nicer than the "Index was out of range" error reported by Visual Studio for the same control XAML. Instead for each Binding in the DataTemplate I got the following error:
 

"The property "Path" is set multiple times."
 

Ok - so still not great - but at least it had identified each line within the template that it didn't like and let me know it was something to do with the data-binding. Now as it turns out I had coded the Bindings as:
 <div>

<span style="color: #0000ff">&lt;</span><span style="color: #800000">TextBlock</span> <span style="color: #ff0000">Text</span><span style="color: #0000ff">="{Binding TemplateBinding, Path=Name}"</span> ...<span style="color: #0000ff">/&gt;</span></pre></div>

    
    Why am I using TemplateBinding within a DataTemplate? I dunno - seemed to make sense when I did it and given that it ran without complaint the first time I never blinked an eye - just kept on doing it. So taking out the TemplateBinding my Binding is just...
    
<div><pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: consolas, 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; border-bottom-style: none"><span style="color: #0000ff">&lt;</span><span style="color: #800000">TextBlock</span> <span style="color: #ff0000">Text</span><span style="color: #0000ff">="{Binding Path=Name}"</span> ...<span style="color: #0000ff">/&gt;</span>
</div>


and hey-presto Blend renders my UserControl without a glitch. Beautiful!



Thinking that at last I had solved the mystery of my missing XAML previews in Cider I flicked back to Visual Studio sharing the same project and re-opened the UserControl. Nope - same error. Recompiled the project - nope same error. Bummer ![](http://blog.spencen.com/emoticons/sad.png). So its something that doesn't generate errors at run-time, is not a problem with Blend, but which causes the Visual Studio designer to barf?



So ignoring issues around availability/pricing should developers be using Blend in preference to Visual Studio for authoring XAML? Or is [Dev10](http://wpf.netfx3.com/blogs/cider_bloggers/archive/2008/02/27/cider-wpf-and-silverlight-designer-and-tools-team-is-hiring.aspx) going to address the shortfall of Cider vs. Blend. I can understand that Blend is geared for UX designers and as such the two products have different end-users in mind but surely Resources and Binding is something a developer needs to be able to manage effectively?


