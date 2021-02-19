---
layout: post
title: "Word Puzzle to Sliverlight Phone–Part 3"
date: 2010-09-05 02:56
author: spencen
comments: true
categories: [.NET, Development, Windows Phone]
tags: []
---


Last night I dusted off [Word Puzzle](http://blog.spencen.com/2010/04/26/word-puzzle-to-silverlight-phone-ndash-part-2.aspx) and decided to try out tombstoning in Window Phone 7 – just to see how much of a pain this is really going to be. The first hurdle I had was to convert the existing solution from the Windows Phone CTP to the Beta release. This turned out to be quite a bit harder than I had expected. On the upside I got a pretty good idea of some of the changes that were made – ditching the resource files, using the manifest to nominate the launch window, assembly consolidation etc.
  

Once I had eventually gotten it to work with the Beta I decided to create a Settings page so that I could:      
1) test the navigation and       
2) have a simplified state object to persist to the application cache whilst de-activating.
  

The settings page looked like this:
  

<a href="/images/WordPuzzle_Stage3_Settings.png">![WordPuzzle_Stage3_Settings](/images/WordPuzzle_Stage3_Settings.png "WordPuzzle_Stage3_Settings")</a>
  

It was bound to some of the properties on my pre-existing LetterBoardSetup class, e.g. AllowBackwards, AllowDiagonal, Width, Height.
  

I added the following code to my App.xaml.cs file and everything worked just fine.
  <div id="codeSnippetWrapper">   

<span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> Application_Activated(<span style="color: #0000ff">object</span> sender, ActivatedEventArgs e)  
    {  
        var myState = PhoneApplicationService.Current.State;  
        settings = myState[<span style="color: #006080">&quot;settings&quot;</span>] <span style="color: #0000ff">as</span> LetterBoardSetup;  
    }  
    <span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> Application_Deactivated(<span style="color: #0000ff">object</span> sender, DeactivatedEventArgs e)  
    {  
        var myState = PhoneApplicationService.Current.State;  
        myState[<span style="color: #006080">&quot;settings&quot;</span>] = settings;  
    }

  
</div>


When the application de-activates the state is saved, then restored correctly on re-activation. I decided to move on and save off the actual game state. The easiest (laziest) way to save the game state seemed to be to just save off the entire object graph (after all we are talking about a *very* trivial game here). Several of my classes had private setters for public properties. No problem I figured – I’ll just use DataContract from System.Runtime.Serialization namespace/assembly. This does all sorts of wonderful things – like allowing private fields to be serialized, creating instances without invoking any constructors and the like. At least I thought that’s how it worked – and on the full .NET framework I would have been right. Through trial and error I determined that the [Silverlight version](http://msdn.microsoft.com/en-us/library/system.runtime.serialization.datacontractserializer(v=VS.95).aspx#1) doesn’t have these capabilities – classes are required to have public setters and getters for properties (yuerk!).



So – in the mindset of just “getting it done” I went through and opened up my object model – changing private setters to public, making getters do “on-demand” construction for things like collections etc. to make it more serialization friendly.



Now I have a version of WordPuzzle that runs in the emulator and survives tombstoning with absolutely no data loss. Should I ever want to I also now have a version that could easily allow games to be saved. In fact all that’s really left to do is find a couple of images for the application bar buttons – oh – and actually make the game play itself something slightly more err… exciting.


