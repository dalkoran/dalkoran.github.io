---title: "Word Puzzle to Silverlight Phone – Part 2"
date: 2010-04-26 04:37
author: spencen
comments: true
categories: [.NET, Development, Windows Phone]
tags: []
---
Finally got interaction and feedback happening on the [Silverlight port of Word Puzzle](http://blog.spencen.com/2010/04/08/porting-wpf-word-puzzle-to-windows-phone-silverlight-ndash-part-1.aspx). This was so much more difficult than I had imagined – feels like learning WPF from scratch. I am beginning to believe that it would be easier to approach Silverlight with no WPF knowledge whatsoever.
  

*   I had to cater for not having DataTriggers – and then not being able to get behaviours/triggers/states to work like I wanted. In the end I used a ValueConverter to hack the *selection* and *solved* colours – yuk!
*   Had some weird issues with the MouseMove event – had to use CaptureMouse to get position readings outside the original UI element – wasn’t a requirement for WPF.
*   Spent ages working through really minor bugs that just aren’t reported properly in Silverlight. Things as simple as referencing a resource that doesn’t exist (due to misspelling) generates a super generic error message.
*   Had to create a proper custom layout panel for the words to position and rotate the highlight boxes. This was actually an improvement on the original version.  

Anyhow – now have a playable version on the emulator. Slow progress, but progress nonetheless.
  

<a href="/images/WordPuzzle_Stage2.png">![WordPuzzle_Stage2](/images/WordPuzzle_Stage2.png "WordPuzzle_Stage2")</a>&#160; <a href="/images/WordPuzzle_Stage2_EndGame.png">![WordPuzzle_Stage2_EndGame](/images/WordPuzzle_Stage2_EndGame.png "WordPuzzle_Stage2_EndGame")</a>


