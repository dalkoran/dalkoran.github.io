---
layout: post
title: "Developing for Windows Mobile – Marketplace Sign-up"
date: 2009-09-24 15:05
author: spencen
comments: true
categories: [Development, Windows Phone]
tags: []
---


[Windows Phone Marketplace](http://developer.windowsmobile.com/Marketplace.aspx). Obviously all the major phone/mobile OS vendors have already beaten Microsoft to market – so you would expect them to have taken some of the learnings from these ventures onboard. In other words – I expect them to do a good job of this – both from a customer and developer perspective.
  

The following is a summary of my experiences in getting an application into the Marketplace thus far… 
  

#### Sign-up Process

  

Microsoft have been very crafty with their Marketplace registration process. Its obviously designed to weed out those individuals who are anything less that desperate to get their product into the app store. If you can answer “no” to two or more of the questions below then I suggest thinking twice before you consider signing-up.
  

*   Do you enjoy trawling [IRS websites](http://www.irs.gov/individuals/article/0,,id=96287,00.html) to learn about the various US tax forms? 
*   Do you enjoy reading about country tax laws and specific treaty clauses? 
*   Do you want to know the difference between [SSNs and ITINs](http://www.irs.gov/individuals/article/0,,id=96287,00.html#how)? 
*   Do you have easy access to a non-expired passport else birth certificate plus other government photo id? 
*   Do you enjoy [filling out paperwork](http://www.irs.gov/pub/irs-pdf/fw7.pdf) and posting (yes snail-mail) your documents overseas? 
*   Do you mind taking a PDF signed by a Microsoft rep, on Microsoft letterhead, modifying it yourself and then sending it off to the IRS as an official sanction for an ITIN? [What is the point of this step?] 
*   Are you really expecting to make a killer mobile app that will make this all worthwhile?   

[Note: If you are a US taxpayer already then most of this will be irrelevant and the process will be a breeze.]
  

Of course I answered “no” to all of these but as I stated before I’m writing the whole thing off as an “educational experience”. Nothing worthwhile is meant to be easy… right?
  

[Note: There is actually a pretty good walkthrough/slideshow of the registration process [here](http://www.slideshare.net/mymobilehome/windows-marketplacefor-mobile-developer-registration-walk-through-081209-pr).]
  

#### Artwork

  

If you got through the sign-up process don’t think you’ve beaten them. You now have to create your application icon/logo in a wide variety of resolutions (dpi) and sizes. Then you have to tweak the install process to pick out the right imagery for the particular device its being installed on. When I say tweak – I mean write some C code and inject that into the installer. OK – maybe that’s a little unfair – but go check out some of the following blog posts and their related comments.
  

*   [http://windowsteamblog.com/blogs/windowsphone/archive/2009/07/24/creating-custom-icons-for-windows-mobile-6-5.aspx](http://windowsteamblog.com/blogs/windowsphone/archive/2009/07/24/creating-custom-icons-for-windows-mobile-6-5.aspx) 
*   [http://windowsteamblog.com/blogs/windowsphone/archive/2009/08/11/using-custom-icons-in-windows-mobile-6-5.aspx](http://windowsteamblog.com/blogs/windowsphone/archive/2009/08/11/using-custom-icons-in-windows-mobile-6-5.aspx) 
*   [http://windowsteamblog.com/blogs/windowsphone/pages/start-screen-png-icon-faq.aspx](http://windowsteamblog.com/blogs/windowsphone/pages/start-screen-png-icon-faq.aspx)   

As one tongue-in-cheek commenter put it:
  

>   

“Was there some sort of requirement to do this in the most developer-hostile way possible, or was that just a happy accident?”
 

  

#### Getting Verified

  

During the sign-up process an email verification is sent out to you by the third-party identity verification company ([GeoTrust](http://www.geotrust.com)) that are issuing the code-signing certificates. Make sure you respond to this email immediately because it takes a week or two after that for their poor over-worked web server to send out the next email which actually asks you to provide some credentials.
  

The good news is that you can supply these credentials back to them via email (unlike the IRS).
  

The bad news is that their systems are a little flaky and four days later they send your exactly the same email by mistake. Luckily their online chat staff seem to be a little more competent than their computer systems and will tell you to ignore the second email whilst they manually forward your details on to the next verification step.
  

I’m still waiting on a final outcome…
  

#### 

  

#### Submitting Your Application

  

This is the part of the process that I expected to be more challenging but unfortunately I haven’t gotten this far yet. Submitting your application to the Marketplace and have it pass all the internal testing. There are a number of tools that Microsoft have published which they use internally during the testing process. One such is [Hopper](http://msdn.microsoft.com/en-us/library/bb158517.aspx) – which jumps between your application and others – presumably to detect your applications ability to quickly switch (i.e. for incoming call), to use minimal resources particularly whilst switched out and to be stable over a long (2 hour) period.
  

One major gotcha with some of the test tools and those supplied with the Windows Mobile SDK is that they only work on 32 bit machines. This is annoying to say the least. They only 32 bit machine I have left is the new HP Netbook which my wife had grown rather fond of.
  

Well – assuming I every get past the verification process I may post some further thoughts on submission and publication.


