---
layout: post
title: "Revisiting Commands"
date: 2008-03-24 14:11
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


Having put together the basics of the user interface for my current project I decided to do some more reading regarding the best practices around using a Command Pattern in WPF.
 

Specifically this means determining how to use the ICommand and RoutedCommand. [My previous attempt](http://blog.spencen.com/2007/09/24/i-command-thee--exception.aspx) (without reading any of the documentation) was something of a failure but I pretty much have the same goals in mind:
 

1.  UI elements that perform an action that is not purely UI related are required to be bound to a Command. This must be easily done in XAML.  The Window/Page itself should have minimal (i.e. as close to possible to zero) code in the code behind. This means no ugly event handlers.  The Command or more likely the Command Binding mechanism will be responsible for setting attributes on the associated UI element(s). So this includes things like Text, Image, Tooltip, Enabled, Visible etc. 

The closest thing I've found so far in terms of recommended approaches is the [collection of blog entries written by Dan Crevier back in 2006](http://blogs.msdn.com/dancre/archive/2006/10/11/datamodel-view-viewmodel-pattern-series.aspx). Dan does a great job of discussing a simplified version of the Data Model - View - View Model (DV-V-VM) that the team working on Microsoft Max used. Having said that there are some approaches that he takes that I either find too unwieldy (one command per class) or (just as likely) don't fully comprehend.
 

Reading straight out of the MSDN documentation I stared with:


<span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Page.CommandBindings</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">CommandBinding</span><span style="color: rgb(255,0,0)"> Command</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Static</span><span style="color: rgb(255,0,0)"> pages</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">RibbonTest</span><span style="color: rgb(0,0,255)">.WriteEntryCommand}"</span><span style="color: rgb(255,0,0)"> <br>                        Executed</span><span style="color: rgb(0,0,255)">="CommandBinding_Executed"</span><span style="color: rgb(255,0,0)"> <br>                        CanExecute</span><span style="color: rgb(0,0,255)">="CommandBinding_CanExecute"/&gt;<br>    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Page.CommandBindings</span><span style="color: rgb(0,0,255)">&gt;<br>    ...<br></span><span style="color: rgb(0,0,255)">    &lt;</span><span style="color: rgb(163,21,21)">Button</span><span style="color: rgb(255,0,0)"> Command</span><span style="color: rgb(0,0,255)">="pages:RibbonTest.WriteEntryCommand"/&gt;
</span></span></pre><a href="http://11011.net/software/vspaste"><a href="http://11011.net/software/vspaste"></a>

    
    This involved defining my own RoutedCommand on the page itself (pages:RibbonTest) and then binding the Executed and CanExecute events to event handlers on the page. Of course this doesn't meet any of my three requirements, - it's too much XAML, it requires code behind event handlers and with the exception of the Enabled property doesn't "reflect" command properties to the UI element.
    

    
    Needless to say I went through a whole raft of possible solutions from here (refer to references below), trying each in turn to see what worked for me and what didn't. 
    

    
    At the moment I have created an attached dependency property (as per Dan's post) so that my XAML is nice and concise.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TabControl</span><span style="color: rgb(255,0,0)"> Name</span><span style="color: rgb(0,0,255)">="tabControl2"</span><span style="color: rgb(255,0,0)"> Style</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">StaticResource</span><span style="color: rgb(255,0,0)"> RibbonBarStyle</span><span style="color: rgb(0,0,255)">}"</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TabItem</span><span style="color: rgb(255,0,0)"> Header</span><span style="color: rgb(0,0,255)">="Holidays"&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ToolBarTray</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ToolBar</span><span style="color: rgb(255,0,0)"> Header</span><span style="color: rgb(0,0,255)">="Select a Holiday"&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Button</span><span style="color: rgb(255,0,0)"> commands</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">CommandBinder.Command</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Controller</span><span style="color: rgb(0,0,255)">.</span><span style="color: rgb(255,0,0)">NewHoliday</span><span style="color: rgb(0,0,255)">}"</span><span style="color: rgb(0,0,255)">/</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Button</span><span style="color: rgb(255,0,0)"> commands</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">CommandBinder.Command</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Controller</span><span style="color: rgb(0,0,255)">.</span><span style="color: rgb(255,0,0)">OpenHoliday</span><span style="color: rgb(0,0,255)">}"</span><span style="color: rgb(0,0,255)">/</span><span style="color: rgb(0,0,255)">&gt;</span><span style="color: rgb(163,21,21)">
</span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ToolBar</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ToolBarTray</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">TabItem</span><span style="color: rgb(0,0,255)">&gt;<br>    ...<br><br><br></span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">TabControl</span><span style="color: rgb(0,0,255)">&gt;</span></span>
<a href="http://11011.net/software/vspaste"></a>


The Controller referenced above is instantiated by the form and defines all of the commands available for the form. It is also responsible for keeping the form state which is then reflected in the commands, and thus by the buttons. The commands are actually a custom class (UICommand) that is a "wrapper" around a RoutedCommand and contains other metadata that is associated with the command. This includes the caption text, full description, image, IsEnabled and IsAvailable (visible/security). The wrapped RoutedCommand also defines any keyboard gestures.



Taking this approach means that:



1.  Only the attached property needs to be set - the CommandBindings are not required - this is done automatically by the attached property.
The command execution logic (callback) lives either in the controller, or in a UICommand derivative (for common functions like navigation).
The only code required in the form code-behind is the instantiation of the controller. Of course this could easily be done in XAML too if preferred.
In addition to setting the Text (via binding), Enabled and Visibility properties I'm also using the CommandBinder to build a groovy Tooltip complete with input key shortcuts.


![XAML Ribbon with Tooltip](/images/XAML%20Ribbon%20with%20Tooltip_1.png) 



Of course its not all roses - there are more than a couple of "nasty" bits in my implementation at the moment. One surprising one was an oddity in the ICommandSource interface. Within the CommandBinder attached property I thought to use this interface to set the Command property on the source at the same time as I registered the RoutedCommand. However, it turns out that the ICommandSource.Command property is read-only - most frustrating.



More than anything though I'm curious as to how everyone else tackles implementing a command pattern (and MVC/MVP etc.) in WPF?



References:



[WPF Commands on Code Project](http://www.codeproject.com/KB/WPF/wpfcommands.aspx)



[Karl on WPF](http://karlshifflett.wordpress.com/category/commands/)



[Rob Relyea on ICommand, RoutedCommand and RoutedUICommand](http://rrelyea.spaces.live.com/Blog/cns!167AD7A5AB58D5FE!1968.entry)



[Josh Smith on RoutedCommand](http://joshsmithonwpf.wordpress.com/2008/03/18/understanding-routed-commands/)



[Josh Smith on MVC](http://www.codeproject.com/KB/WPF/MVCtoUnitTestinWPF.aspx)



[Commands in Prism](http://www.codeplex.com/prism/Thread/View.aspx?ThreadId=23196) (some great comments)


