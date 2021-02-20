---title: "VS2010 Intellisense and refactoring"
date: 2009-06-18 01:55
author: spencen
comments: true
categories: [.NET, Development]
tags: []
---
I’ve been spending a little more time recently with Visual Studio 2010 Beta 1. In particular building a solution from the ground up (including TFS integration) and doing a fair bit of prototyping. Some of the new editor improvements in VS2010 are really pretty neat.
  

One of the first changes that you notice in the editor of course is the changes to intellisense. It now includes auto-filtering, but not just by a “starts-with” search but using more of a “contains” approach. This includes the ability to search for multi-word names using abbreviations, e.g. “AQN” to match “AssemblyQualifiedName.“ Personally I think the filtering works particularly well. Having used Resharper’s intellisense filtering I wasn’t to keen on the idea, I find that implementation to be slow and it somehow feels *invasive*. The VS2010 implementation IMHO is *much* better.
  

[here](http://blogs.msdn.com/somasegar/archive/2008/12/19/code-focused-development-in-vs-2010.aspx) for more VS2010 *code-focused* changes).
  

{sigh} It’s getting harder and harder to go back to Visual Studio 2008.


