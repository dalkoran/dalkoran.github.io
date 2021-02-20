---
title: "Fixed Width Fonts are so 80's"
date: 2007-11-08 15:21
author: spencen
comments: true
categories: [.NET, Development]
tags: []
---

Seriously - why is it that when I walk around the office I see all the developers staring at Visual Studio code windows displayed using "Courier New" font. Trust me - the "new" in "Courier New" ain't that new!
 

Can someone please explain to me how using a fixed width font so that your character positions line up is more important that readability gains of using proportional fonts such as Verdana. Ok - the guy using vi down the back of the class - you can put your hand down - I'm sure your hippy editor is just the bees knees but if it don't support proportional fonts I'm really not interested.
 

How is this:
 <div>

<span style="color: #008000">/// &lt;summary&gt;</span>
<span style="color: #008000">/// Provides a simplified overview of the validation failure in multi-line text format.</span>
<span style="color: #008000">/// &lt;/summary&gt;</span>
<span style="color: #008000">/// &lt;returns&gt;A multi-line description of the validation failure.&lt;/returns&gt;</span>
<span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">string</span> ToString()
{
<span style="color: #0000ff">return</span> <span style="color: #0000ff">string</span>.Format(<span style="color: #006080">"{0}: \t{1}\r\n\t\t{2}"</span>,
Severity,
<span style="color: #0000ff">string</span>.Format(Message, MessageParameters),
<span style="color: #0000ff">string</span>.Format(MessageDetail, MessageParameters));
}</pre></div>

    
    Better than this!?
    
<div><pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: verdana, arial, helvetica; border-right-style: none; border-left-style: none; background-color: #f4f4f4; border-bottom-style: none"><span style="color: #008000">/// &lt;summary&gt;</span>
<span style="color: #008000">/// Provides a simplified overview of the validation failure in multi-line text format.</span>
<span style="color: #008000">/// &lt;/summary&gt;</span>
<span style="color: #008000">/// &lt;returns&gt;A multi-line description of the validation failure.&lt;/returns&gt;</span>
<span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">string</span> ToString()
{
<span style="color: #0000ff">return</span> <span style="color: #0000ff">string</span>.Format(<span style="color: #006080">"{0}: \t{1}\r\n\t\t{2}"</span>,
Severity,
<span style="color: #0000ff">string</span>.Format(Message, MessageParameters),
<span style="color: #0000ff">string</span>.Format(MessageDetail, MessageParameters));
}
</div>


I'm mean it's not like we're all writing in some old version of COBOL or RPG where the column positions really matter. I know they'll be some old sticklers that really can't let go of their last hold of the 'good ol' days of 80s programming - but for the rest of us - let's move on.



I hope Visual Studio 2008 RTM ships with a proportional font as the default editor font. 


