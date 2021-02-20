---
title: "Developing for Windows Mobile – Guru Meditation"
date: 2009-10-08 14:51
author: spencen
comments: true
categories: [.NET, Development, General, Windows Phone]
tags: []
---

<P>I’ve been working away for a few nights now trying to put together a simple framework for building user interfaces on a Windows Mobile device. It’s only taken a few nights to build something that I’m pretty happy with… and I was thinking of maybe even posting some source code.</P>
<P>It was at this moment of course that I had a visit from my Inner Guru. You know the one… that little voice in your head that sniggers impolitely whenever you struggle to convert a loop into a LINQ expression. Or when you’re hacking out some prototype code and you need to new up an instance. You know that really its going to need an IOC container, you look around, no-ones watching… in goes the explicit “new Foo()”. Immediately your Inner Guru can be heard muttering to themselves, “tut… tut.. will he <EM>never</EM> learn…”.</P>
<P>Anyhow, my Inner Guru popped in for a visit just the other day and we had a quick chat about the work I’d been doing on my mobile framework. The conversation went something like this:</P>
<BLOCKQUOTE>
<P><STRONG>Me</STRONG>: Hey – check out this framework I’ve been building for Windows Mobile – pretty cool huh?</P>
<P><STRONG>Guru</STRONG>: Hmm… yes it appears you’ve made quite some progress.</P>
<P><STRONG>Me</STRONG>: Yeah – look it can do animations.</P>
<P><STRONG>Guru</STRONG>: Very impressive. Did you have trouble using the DirectX libraries?</P>
<P><STRONG>Me</STRONG>: Ah well, err no it’s just using GDI.</P>
<P><STRONG>Guru</STRONG>: Ah, I see. Does that give you full hardware acceleration on the device?</P>
<P><STRONG>Me</STRONG>: Um… no I don’t think so.</P>
<P><STRONG>Guru</STRONG>: Ah, I see. Commendable that you have support for an opacity on each element.</P>
<P><STRONG>Me</STRONG>: Yeah, well I mean no. It seemed like a good idea at the time but I haven’t done that yet.</P>
<P><STRONG>Guru</STRONG>: Ah, I see. It appears all objects support using a RotateTransform?</P>
<P><STRONG>Me</STRONG>: Yeah, well I mean no. I couldn’t figure out how to rotate images an arbitrary amount with decent performance.</P>
<P><STRONG>Guru</STRONG>: Ah, I see. I notice that your transforms all inherit from a base class which of course must perform the Matrix calculations.</P>
<P><STRONG>Me</STRONG>: Yeah, well actually no. I mean there is a base class but I haven’t bothered implementing the transforms using Matrix math – they’re just, you-know, hard coded.</P>
<P><STRONG>Guru</STRONG>: Ah, I see. It seems you’ve done some work building the framework for a layout engine that can be plugged into any container control. Seems a bit WinForms’ish to me, not very WPF like.</P>
<P><STRONG>Me</STRONG>: Well I didn’t think it was so bad, although I haven’t actually written that bit yet.</P>
<P><STRONG>Guru</STRONG>: Ah, I see. So no actual layout panels yet, not even the most simple stack panel?</P>
<P><STRONG>Me</STRONG>: Err, well no – but it wouldn’t be hard to add.</P>
<P><STRONG>Guru</STRONG>: But you haven’t done it – even though it wouldn’t be hard.</P>
<P><STRONG>Me</STRONG>: Well no.</P>
<P><STRONG>Guru</STRONG>: Hmm.</P>
<P><STRONG>Me</STRONG>: I was thinking of posting the source code.</P>
<P><STRONG>Guru</STRONG>: I see. So despite the obvious architectural flaws, the missing classes, the lacklustre performance, the questionable code formatting you feel it adequate for public viewing, or perhaps that may be ridicule? I see you’re still using foreach loops – can’t quite seem to wrap your head around LINQ can you dear boy?</P></BLOCKQUOTE>
<BLOCKQUOTE>
<P><STRONG>Me</STRONG>: Umm…</P></BLOCKQUOTE>
<P>Fortunately it was at this point that my Inner Guru hit an untrapped exception deep in his runtime. The following image began blinking away in my head. </P><IMG src="http://upload.wikimedia.org/wikipedia/commons/d/db/Guru_meditation.gif">
<P>I suppressed an evil chuckle and posted the code: <A href="http://mobileui.codeplex.com">http://mobileui.codeplex.com</A></P>

