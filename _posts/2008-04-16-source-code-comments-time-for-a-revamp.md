---
layout: post
title: "Source Code Comments - Time for a revamp"
date: 2008-04-16 15:00
author: spencen
comments: true
categories: [.NET, Development, General]
tags: []
---


<font color="#0000ff">*[If you're impatient then check out the image [here](/images/Visual%20Studio%20Help%20Editor%201_4.png) to jump straight to the reason behind this post.]*</font>
 

When Microsoft first declared there intent to include support for comments within the C# language I was overjoyed. I was so taken with the idea that I wrote a VB6 add-in that would parse VB6 source comments prefixed by '/ and convert them into a HTML page which documents the class. Importantly it also provided a real-time preview window for the current method/property.
 

As it turned out when .NET was finally released I landed a job coding in VB.NET rather than C#. There were a couple of freeware tools around that would allow the XML documentation files to be produced from VB.NET source code. They were OK but it really wasn't the same as having the compiler validate comment references. Also most of the tools we tried were a little flaky and prone to crashing every now and then.
 

Jumping ahead a few years and I'm using C# with Visual Studio 2005 on a large scale project. On this project I was responsible for writing a lot of architecture and doing my best to make sure it was documented. We used [NDoc](http://ndoc.sourceforge.net/) to produce MSDN style Help via MSBuild and our continuous integration engine ([CruiseControl.NET](http://sourceforge.net/projects/ccnet/)). We also had to produce a script that would correctly register the generated v2 Help file to have it integrate seamlessly with the Visual Studio documentation. Setting this up was tricky. It required us to debug and fix the latest NDoc build to properly support generics, use another third party tool to generate the required Help registration goo, write custom NDoc tags for inserting images (like class diagrams) and creating FAQ indexes etc. It would also take about an hour to run (over our 40+ project solution).
 

#### But NDoc is dead!

 

Yes, but as everyone knows [Sandcastle](http://www.codeplex.com/Sandcastle) lives! The first few drops of Sandcastle were really quite scary. Config files here there and everywhere, write your own scripts to produce the end result. But whilst Microsoft were concentrating hard on getting the fundamental engine right the community came to the rescue&nbsp; with Sandcastle Help File Builder ([SHFB](http://www.codeplex.com/SHFB)). A full featured, easy to use GUI to produce Help v1, v2 and HTML documentation using the Sandcastle engine. The recent builds of Sandcastle coupled with SHFB and we're finally back (in fact slightly ahead) of where NDoc left off. The SHFB has even been built to look and feel just like the NDoc GUI - nice!
 <p align="center"><a href="/images/Sandcastle%20-%20UriToThumbnailConverter.png" target="_blank">![Sandcastle - UriToThumbnailConverter](/images/Sandcastle%20-%20UriToThumbnailConverter.png)</a>&nbsp;
 <p align="center">*Some HTML v1 output from Sandcastle using SHFB.*
 

#### Source Comments are ugly

 

However, having had a chance to write a fair amount of source comments on this particular VS2005 project, one thing became obvious. Improving the quality of the externally viewed documentation (e.g. in dexplore, or HTLP Help Viewer) had a directly negative effect on the readability of the same documentation within the source files (e.g. Visual Studio).
 

In other words - adding more markup to the source comments made them harder to read in Visual Studio. Yet I want the both of best worlds. I want to be able to produce useful documentation integrated with MSDN but at the same time I want those comments to be easily read by the developer when they are right there in the source.
 <div style="border-right: gray 1px solid; padding-right: 4px; border-top: gray 1px solid; padding-left: 4px; font-size: 8pt; padding-bottom: 4px; margin: 20px 0px 10px; overflow: auto; border-left: gray 1px solid; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; padding-top: 4px; border-bottom: gray 1px solid; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4">

<span style="color: #008000">/// &lt;summary&gt;</span>
<span style="color: #008000">/// This &lt;see cref="IValueConverter"/&gt; allows image paths in XAML to be efficiently converted into</span>
<span style="color: #008000">/// thumbnail images which could, for example, be used to populate an &lt;see cref="ItemsControl"/&gt;.</span>
<span style="color: #008000">/// &lt;/summary&gt;</span>
<span style="color: #008000">/// &lt;remarks&gt;</span>
<span style="color: #008000">/// &lt;para&gt;</span>
<span style="color: #008000">/// This converter relies on the image file referenced by the string representation of the value</span>
<span style="color: #008000">/// having a thumbnail image stored as part of its metadata. This will be likely for any image that</span>
<span style="color: #008000">/// has been taken direct from a modern digital camera. Digital cameras  record a thumbnail image</span>
<span style="color: #008000">/// for their own internal processing. This thumbnail "lives" with the metadata, e.g. EXIF tags that</span>
<span style="color: #008000">/// record other information such as when the image was taken, the various camera settings etc.</span>
<span style="color: #008000">/// &lt;/para&gt;</span>
<span style="color: #008000">/// &lt;para&gt;</span>
<span style="color: #008000">/// If the string representation of the value does not represent a valid file, or the file is not accessible</span>
<span style="color: #008000">/// for any reason then a default image will be returned.</span>
<span style="color: #008000">/// &lt;/para&gt;</span>
<span style="color: #008000">/// &lt;note&gt;If this converter is used with a non-string value it will perform a &lt;c&gt;ToString()&lt;/c&gt; on</span>
<span style="color: #008000">/// the method in order to generate a string which will then be translated to a URI used to</span>
<span style="color: #008000">/// load the image.&lt;/note&gt;</span>
<span style="color: #008000">/// &lt;/remarks&gt;</span>
<span style="color: #008000">/// &lt;example&gt;</span>
<span style="color: #008000">/// The following example shows how the converter can be used in XAML.</span>
<span style="color: #008000">/// &lt;code lang="XML" title="Using the UriToThumbnailConverter in XAML" numberLines="true"&gt;</span>
<span style="color: #008000">/// &amp;lt;converters:UriToThumbnailConverter x:Key="uriToThumbnailConverter"/&amp;gt;</span>
<span style="color: #008000">/// ...</span>
<span style="color: #008000">/// &amp;lt;Image Source="{Binding Path=FilePath, Converter={StaticResource uriToThumbnailConverter}}" Height="90" /&amp;gt;</span>
<span style="color: #008000">/// &lt;/code&gt;</span>
<span style="color: #008000">/// &lt;/example&gt;</span>
<span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> UriToThumbnailConverter : IValueConverter
{
<span style="color: #008000">/// &lt;summary&gt;</span>
<span style="color: #008000">/// Converts a string containing an image path (URI) into the thumbnail &lt;see cref="BitmapSource"&gt;bitmap&lt;/see&gt;</span>
<span style="color: #008000">/// image contained within the image.</span>
<span style="color: #008000">/// &lt;/summary&gt;</span>
<span style="color: #008000">/// &lt;param name="value"&gt;The value produced by the binding source.&lt;/param&gt;</span>
<span style="color: #008000">/// &lt;param name="targetType"&gt;The type of the binding target property.&lt;/param&gt;</span>
<span style="color: #008000">/// &lt;param name="parameter"&gt;The converter parameter to use.&lt;/param&gt;</span>
<span style="color: #008000">/// &lt;param name="culture"&gt;The culture to use in the converter.&lt;/param&gt;</span>
<span style="color: #008000">/// &lt;returns&gt;</span>
<span style="color: #008000">/// A converted value. If the method returns &lt;see langword="null"/&gt;, the valid &lt;see langword="null"/&gt; value is used.</span>
<span style="color: #008000">/// &lt;/returns&gt;</span>
<span style="color: #0000ff">public</span> <span style="color: #0000ff">object</span> Convert(<span style="color: #0000ff">object</span> <span style="color: #0000ff">value</span>, Type targetType, <span style="color: #0000ff">object</span> parameter, System.Globalization.CultureInfo culture)
{
<span style="color: #0000ff">try</span>
{
BitmapFrame bitmap = BitmapFrame.Create(<span style="color: #0000ff">new</span> Uri(<span style="color: #0000ff">value</span>.ToString()), BitmapCreateOptions.DelayCreation, BitmapCacheOption.OnDemand);
<span style="color: #0000ff">return</span> bitmap.Thumbnail;
}
<span style="color: #0000ff">catch</span>
{
<span style="color: #0000ff">return</span> System.Drawing.SystemIcons.Exclamation.ToBitmap();
}
}
</div>


#### So what's the solution?




Well - back in those Visual Studio 2005 days I used to use a little tool called [CR_Documentor](http://www.paraesthesia.com/archive/2004/11/15/cr_documentor---the-documentor-plug-in-for-dxcore.aspx) which was written as a [DXCore](http://www.devexpress.com/Products/NET/IDETools/DXCore/) add-in. This provided a real-time preview window of what your source comments would look like once marked-up. This was really useful as there was nothing more annoying that doing a whole bunch of source comments only to find the next day (after the nightly documentation was produced) I had made some silly error with my markup causing corrupt, incomplete or missing documentation. Admittedly the build I was using used to struggle keeping up with some of the really large namespace documentation (e.g. 500+ lines), but it did a good job for the small stuff.

<p align="center">![](http://www.paraesthesia.com/images/pMachine/CR_Documentor_options_sm.gif) 

<p align="center">*Configuration settings for CR_Documentor.*



#### But please Microsoft, I want more!




What I really want is source comment markup integrated in the Visual Studio editor. When I'm browsing source code I don't want to see angle brackets and tags all over the place. I want to see clearly readable documentation. When I want to edit the source comments then you can show me the angle brackets. Or better yet - given only a limited set of documentation and standard HTML tags are supported - why should I ever need to see the tags? Give me a limited document editor with a set of extendable "regions" and some basic bold/italic etc. formatting. (Click the image below to see in its full glory what could be a preview of Dev10).

<p align="center"><a href="/images/Visual%20Studio%20Help%20Editor%201.png" target="_blank">![Visual Studio Help Editor](/images/Visual%20Studio%20Help%20Editor%201.png)</a> 

<p align="center">*An impression of what source code comment viewing could be like in Visual Studio.*



So everyone knows that [fixed-width fonts are out](http://blog.spencen.com/2007/11/09/fixed-width-fonts-are-so-80s.aspx), and more readable proportional fonts are in. So why can't we also get some [WYSIWYG](http://en.wikipedia.org/wiki/WYSIWYG) capabilities in the editor?



Some features of the new inline WYSIWYG source comment editor:



1.  Comments can be collapsed completely, or to a single line (as per existing region style collapse) using the expand collapse arrow to the left of the class/member. (Not sure how the arrow does a three state collapse - but hey why not?)
Comment sections can each be collapsed to the heading as per MSDN comments.
Default collapse settings for comments are configurable via Visual Studio settings.
It should look nicer than my hacked "Frankenstein" screenshot which is more or less just overlaying the Sandcastle output over a Visual Studio editing window. In particular the comments should be de-emphasized.
Hmm... what else?


#### Resources:




[Sandcastle](http://www.codeplex.com/Sandcastle) - Microsoft's source code documentation engine



[Sandcastle Help File Builder](http://www.codeplex.com/SHFB) - NDoc-like GUI for Sandcastle



[GhostDoc](http://www.roland-weigelt.de/ghostdoc/) - does a "spooky" job of commenting your code for you.



[CR_Documentor](http://www.paraesthesia.com/archive/2004/11/15/cr_documentor---the-documentor-plug-in-for-dxcore.aspx) - real-time preview window of source comments marked up



[XML Documentation Comments Guide](http://www.dynicity.com/products/XmlDocComments.aspx) - explains the various comment tags for VS 2003, 2005, NDoc and Sandcastle


