---title: "A New Project (part 2)"
date: 2008-08-30 15:05
author: spencen
comments: true
categories: [.NET, Development, General]
tags: []
---
[David](http://davidgardiner.blogspot.com/) left a comment to my [previous post](http://blog.spencen.com/2008/08/28/a-new-project.aspx) rightly pointing out that I’d omitted MbUnit from the list of tools. Considering that omission I also realised that I’d missed a few other tools. For completeness sake I’ve decided to include them in this follow-up post.
  

**Tortoise and Subversion ![3Star](/images/3Star_3.png "3Star") **
  

I wasn’t new to Tortoise or Subversion but my previous encounters with them had left something of a bitter taste. In particular the lack of Visual Studio integration, the difficulty involved in moving code around (renaming) and the fact that I was always discovering files from the solution that I’d forgotten to manually add to the repository.
  

For the last 8 months I’ve been using Team Foundation Server’s source control and its interesting to compare the two. It seems to me that each one’s major strength is also their weaknesses. For example, when working on a small team in which “everyone owns the code” it can sometimes be very useful to know what is currently being worked on – i.e. checked out by another user. TFS supports this by being aware of all check-outs which means you often know which files you need to be careful about when merging, or you may decide not to modify the file until the other developer has checked it in. Of course the downside of this is that TFS’s offline capabilities become a lot more complicated (bad). Subversion is the opposite – you’ve got no idea what’s being worked on ![](http://blog.spencen.com/emoticons/sad.png) - but it means working offline requires no smarts at all.
  

The big discriminator for me at the moment between TFS and SVN is the quality of the merging tools. TFS warns you about any conflicts, giving you the option to perform manual or automatic merging on each file. I haven’t figured out how to do this in SVN – it just does the merge for you and prompts when it can’t resolve a conflict. The problem is that this auto-merge is just *way* to optimistic about its own capabilities. Or to put it another way – it sucks! It gets it wrong frequently – i.e. once every couple of days. If you’re lucky you notice straight away (for example the app doesn’t compile). If you’re unlucky its rolled back some code, or duplicated a line which you’ll find out about down the track when odd bugs start (re-)appearing. [If anyone has any pointers on this I’m all ears – because in my experience its scarily bad – I’m starting to resort to manually checking every file prior to commit.]
  

**Visual SVN**&#160;![4Star](/images/4Star_3.png "4Star") 
  

We went for a couple of weeks without having any Visual Studio integration. Things worked OK – but it did seem kinda redundant having an Explorer window open to perform source control operations. Especially as Explorer isn’t to flash at refreshing itself, so icons are often lagging unless a forced refresh is done.
  

Visual SVN has been doing a great job and was cheap to boot. Renames are so much nicer!
  

**MbUnit ![4Star](/images/4Star_6.png "4Star") **
  

We’re using MbUnit as our testing framework. To date I’ve just treated it almost as I would have NUnit or the built in MS namespaces. However, just the addition of the Row attribute makes MbUnit worth having. This is a tool (along with Rhino Mocks) that I really need to dig around a bit more with as I’m sure there are plenty of hidden gems I’m not making good use of.
  

**TestDriven.NET&#160; **
  

Testing menus everywhere. Job done.


