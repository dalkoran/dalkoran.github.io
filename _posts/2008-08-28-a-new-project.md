---
title: "A New Project"
date: 2008-08-28 14:26
author: spencen
comments: true
categories: [.NET, Development, General]
tags: []
---

About five weeks ago I changed jobs – well I guess more accurately I took on another job but that’s a long story.
  

One of the key highlights of the new challenge was the technology stack:
  

*   Visual Studio 2008 SP1 
*   WPF 
*   NHibernate 
*   SQL 2008 
*   Sync Services for ADO.NET 
*   Rhino Mocks 
*   Resharper 
*   CruiseControl 
*   NCover 
*   Castle   

A lot of these tools I hadn’t had a chance to use in a commercial environment before. We’re at the point now where we’re handing over the first release of the software to system testing – in a couple of weeks it will be rolled out to a selection of&#160; end users. I thought this would be a good opportunity for me to jot down my current feelings towards these tools – something to reflect on at a later date.
  

**Visual Studio 2008 SP1 ![4Star](/images/4Star_12.png "4Star") **
  

Finally I get to use C# 3.0 at work – yay! Lambdas, LINQ, var, object initializers – aahh such sweet stuff. Unfortunately, though SP1 bought a lot of goodness and fixed a few issues with XAML editing its also got one or two nasty glitches. Every now and then Visual Studio just “disappears” when opening a XAML file. If you reload the solution – poof – it closes down again. The trick is to make sure the Toolbox is un-pinned when opening the solution again. Sometimes even that doesn’t do the trick though and you have to delete the solutions SUO file before re-opening.
  

**WPF ![5Star](/images/5Star_6.png "5Star") **
  

Awesome (yeah I’m biased ![](http://blog.spencen.com/emoticons/smile.png)). Still finding it hard to do certain things that should really be easy – but the pain is so much less than when I first started last year. Of course there are a bunch of improvements I’d like to see – particularly around the visual designer. Makes me long for one of those jobs on the Cider team that [Karl posted about](http://karlshifflett.wordpress.com/2008/08/27/first-week-at-microsoft/) now he’s been *absorbed* by Microsoft. “Build a Resource Manager”… “How about a Data Binding Explorer”…
  

**SQL 2008 ![4Star](/images/4Star_11.png "4Star") **
  

Haven’t really got into this as much as I’d like – there’s no real killer feature – but there are plenty of smaller features that really make it worthwhile. Change Tracking, Intellisense (when it decides to work), Resource Manager, Tools for Express editions, decent date types better matching the .NET types etc.
  

**Sync Services for ADO.NET ![5Star](/images/5Star_5.png "5Star") **
  

Love this stuff – has a few quirks but generally provides you with lots of depth. The wizard gets you started but you can drill into it as deep as you want – down to handwriting all the SQL yourself.
  

**Rhino Mocks ![3Star](/images/3Star_3.png "3Star") **
  

Mocking frameworks were new to me. I understand the principles – but I’d never bothered with one before – preferring instead to create and maintain my own concrete mock objects where possible. I’m finding Rhino Mocks to be quite a struggle – at times its bliss – at others its just plain frustrating. Need to keep working on this one.
  

**NCover **
  

Well if nothing else its convinced me that 100% code coverage means relatively little. In fact I think chasing code coverage percentages is generally detrimental to the quality of the overall tests. Not really fair to rate this one – it does what its supposed to do very well – I’m just not convinced that what its supposed to do is worth doing.
  

**Resharper ![2Star](/images/2Star_3.png "2Star") **
  

Some great features in this package. I love the highlighting of redundant code blocks (casts, variables, pointless condition tests etc). Also has some nice navigation hot keys. However, its slows my (brand new 4Gb RAM) machine to a crawl, the Intellisense and formatting is just plain intrusive (so much so I switched most of it off) and it crashes frequently. Often when it crashes it throws up a stupid “would you like to submit this crash” dialog that just takes me that much closer to uninstalling the package – particularly when I’ve told it countless times not to show me that dialog!
  

To sum this one up its got some great features – but they come at a significant cost. This is only still installed on my machine because I believe its the type of product you really need to commit some effort to before you can break through the pain barrier.
  

**CruiseControl ![4Star](/images/4Star_10.png "4Star") **
  

I have recently been using TFS which I’m a big fan of but I’ve used CruiseControl before and it certainly does a great job.
  

**NHibernate** ![1Star](/images/1Star_3.png "1Star") 
  

Probably the reason the project wasn’t finished a week earlier. I wasn’t really expecting too much from NHibernate – after all its [just an ORM](http://www.paulstovell.com/blog/orm-its-time-to-do-some-real-work) right? We’ve got some clever people working on our team [though admittedly I don’t think any would consider themselves an NHibernate expert] but every minor thing has been a struggle. Some examples: 
  

*   Using our own collections for one to many relationships – why is this so hard!? 
*   Creating a simple two level inheritance hierarchy – how can this not be trivial? 
*   Having to have nullable parent foreign keys in some scenarios because NHibernate insists on performing an insert prior to updating the parent foreign key. Why can’t it determine the dependency order and do the inserts in the right sequence? 
*   Eager loading generates idiotic queries. Reference data e.g. code lookups are fetched using individual SELECTs rather than by joining on the main table – WTF? So what could be done in one SELECT is instead done in several *thousand* (if you have several thousand rows).   

As things stand I would *never* recommend this tool. If you’ve got a simple database your time is better spent writing your own data access layer rather than struggling with NHibernate’s peculiarities. If you’ve got a complex database then spend the time to investigate other alternatives. I cringe to think that NHibernate could really be the ORM of cho
ice for an enterprise app.
  

**Castle** ![4Star](/images/4Star_9.png "4Star") 
  

We’ve been using Castle Windsor as our IoC. Previously I’d used Unity and, in the simple method that we’re using an IoC, there really isn’t any difference between them. Windsor does the job very nicely and I’m really enjoying working on an application that is so loosely coupled.
  

For performing validation in the business layer we’re also using Castle’s Validation components. These are really lightweight and provide some very simple property level validation through the use of attributes. Simple but nicely encapsulated. We’ve expanded this to work with our own entity level validation and then hooked that into the WPF Validation pipeline when those business objects are attached to a UI.
  

Castle documentation (what there is of it) sucks.
  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  


  

__________________________
  

Well – like I said – this is really *my* initial reaction to these tools. As the project progresses I’m sure some of these will change (for better or worse). Feel free to flame me if you think I've made some poor judgement calls.


