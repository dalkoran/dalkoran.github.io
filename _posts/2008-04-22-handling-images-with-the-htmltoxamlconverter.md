---
layout: post
title: "Handling Images with the HtmlToXamlConverter"
date: 2008-04-22 16:05
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


I was asked via a [comment](http://blog.spencen.com/2008/01/19/html-to-flowdocument-converter.aspx#comment-989719) "why won't HtmlToXamlConverter display images, for example those embedded in an RSS feed's contents?".
 

The answer is probably indicated by the comment in the [HtmlToXamlConverter](http://blogs.msdn.com/wpfsdk/archive/2006/05/25/606317.aspx) classes AddBlock() method ![](http://blog.spencen.com/emoticons/smile.png):


<span style="font-size: 8pt; font-family: verdana">        <span style="color: rgb(0,0,255)">case</span> <span style="color: rgb(163,21,21)">"img"</span>:
<span style="color: rgb(0,128,0)">// TODO: Add image processing
</span>            AddImage(xamlParentElement, htmlElement, inheritedProperties, stylesheet, sourceContext);
<span style="color: rgb(0,0,255)">break</span>;</span></pre><a href="http://11011.net/software/vspaste"></a>

    
    And the equally terse AddImage() method itself:
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">        <span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(0,0,255)">void</span> AddImage(<span style="color: rgb(43,145,175)">XmlElement</span> xamlParentElement, <span style="color: rgb(43,145,175)">XmlElement</span> htmlElement, <br>                                                  <span style="color: rgb(43,145,175)">Hashtable</span> inheritedProperties, <span style="color: rgb(43,145,175)">CssStylesheet</span> stylesheet, <br>                                                  <span style="color: rgb(43,145,175)">List</span>&lt;<span style="color: rgb(43,145,175)">XmlElement</span>&gt; sourceContext)
{
<span style="color: rgb(0,128,0)">//  Implement images
</span>        }</span></pre><a href="http://11011.net/software/vspaste"></a>

    
    I'd tweaked the HtmlFromXamlConverter AddImage class as follows:
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">        <span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(0,0,255)">void</span> AddImage(<span style="color: rgb(43,145,175)">XmlElement</span> xamlParentElement, <br>                                                  <span style="color: rgb(43,145,175)">XmlElement</span> htmlElement, <br>                                                  <span style="color: rgb(43,145,175)">Hashtable</span> inheritedProperties, <br>                                                  <span style="color: rgb(43,145,175)">CssStylesheet</span> stylesheet, <br>                                                  <span style="color: rgb(43,145,175)">List</span>&lt;<span style="color: rgb(43,145,175)">XmlElement</span>&gt; sourceContext)
{
<span style="color: rgb(0,128,0)">//  Implement images (HACK by Nigel Spencer)
</span>            <span style="color: rgb(0,0,255)">bool</span> inLine = (xamlParentElement.Name == <span style="color: rgb(43,145,175)">HtmlToXamlConverter</span>.Xaml_Paragraph);
<span style="color: rgb(43,145,175)">XmlElement</span> xamlUIContainerElement = <span style="color: rgb(0,0,255)">null</span>;
<span style="color: rgb(0,0,255)">if</span> (inLine)
xamlUIContainerElement = xamlParentElement.OwnerDocument.CreateElement(<br>                                                            <span style="color: rgb(0,0,255)">null</span>, <span style="color: rgb(163,21,21)">"InlineUIContainer"</span>, _xamlNamespace);
<span style="color: rgb(0,0,255)">else
</span>                xamlUIContainerElement = xamlParentElement.OwnerDocument.CreateElement(<br>                                                            <span style="color: rgb(0,0,255)">null</span>, <span style="color: rgb(163,21,21)">"BlockUIContainer"</span>, _xamlNamespace);
<span style="color: rgb(43,145,175)">XmlElement</span> xamlImageElement = xamlParentElement.OwnerDocument.CreateElement(<br>                                                                  <span style="color: rgb(0,0,255)">null</span>, <span style="color: rgb(163,21,21)">"Image"</span>, _xamlNamespace);
xamlImageElement.SetAttribute(<span style="color: rgb(163,21,21)">"Source"</span>, htmlElement.GetAttribute(<span style="color: rgb(163,21,21)">"src"</span>));
xamlImageElement.SetAttribute(<span style="color: rgb(163,21,21)">"Stretch"</span>, <span style="color: rgb(163,21,21)">"None"</span>);
xamlUIContainerElement.AppendChild(xamlImageElement);
xamlParentElement.AppendChild(xamlUIContainerElement);
}</span></pre>

    
    Don't expect too much from this though. If you really need to display rich HTML in a WPF app then use a Frame control. However, if like me, you've got some predominantly textual data stored in HTML and you want to make use of the great FlowDocument reader controls then the HtmlFromXamlConverter is a great little utility.
    

    
    The source code for my test WPF harness was as&nbsp; follows:
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">partial</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">Window1</span> : <span style="color: rgb(43,145,175)">Window
</span>    {
<span style="color: rgb(0,0,255)">public</span> Window1()
{
InitializeComponent();
}
<span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">void</span> button1_Click(<span style="color: rgb(0,0,255)">object</span> sender, <span style="color: rgb(43,145,175)">RoutedEventArgs</span> e)
{
<span style="color: rgb(43,145,175)">FlowDocument</span> flowDocument = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">FlowDocument</span>();
<span style="color: rgb(0,0,255)">string</span> xaml = <span style="color: rgb(43,145,175)">HtmlToXamlConverter</span>.ConvertHtmlToXaml(textBox1.Text, <span style="color: rgb(0,0,255)">false</span>);
<span style="color: rgb(0,0,255)">using</span> (<span style="color: rgb(43,145,175)">MemoryStream</span> stream = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">MemoryStream</span>((<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ASCIIEncoding</span>()).GetBytes(xaml)))
{
<span style="color: rgb(43,145,175)">TextRange</span> text = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">TextRange</span>(flowDocument.ContentStart, flowDocument.ContentEnd);
text.Load(stream, <span style="color: rgb(43,145,175)">DataFormats</span>.Xaml);
}
flowDocumentReader1.Document = flowDocument;
}<br>    }</span></pre><a href="http://11011.net/software/vspaste"></a>

    
    Some very simple HTML to test. I also tested with HTML source from a&nbsp; blog post of two and it seemed OK.
    
<div><pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: consolas, 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; border-bottom-style: none"><span style="color: #0000ff">&lt;</span><span style="color: #800000">HTML</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">BODY</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>The <span style="color: #0000ff">&lt;</span><span style="color: #800000">b</span><span style="color: #0000ff">&gt;</span>Markup<span style="color: #0000ff">&lt;/</span><span style="color: #800000">b</span><span style="color: #0000ff">&gt;</span> that is to be converted.<span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>An inline image [<span style="color: #0000ff">&lt;</span><span style="color: #800000">img</span> <span style="color: #ff0000">src</span><span style="color: #0000ff">="[http://www.microsoft.com/australia/remix08/images/bling/iamgoing_08.jpg](http://www.microsoft.com/australia/remix08/images/bling/iamgoing_08.jpg)"</span><span style="color: #0000ff">/&gt;</span>]<span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>Ooh! <span style="color: #0000ff">&lt;</span><span style="color: #800000">i</span><span style="color: #0000ff">&gt;</span>Beautiful.<span style="color: #0000ff">&lt;/</span><span style="color: #800000">i</span><span style="color: #0000ff">&gt;&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">img</span> <span style="color: #ff0000">src</span><span style="color: #0000ff">="http://images.quickblogcast.com/public/visitorImages/black_createdby.gif"</span><span style="color: #0000ff">/&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>Hmm...<span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">BODY</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">HTML</span><span style="color: #0000ff">&gt;</span>
</div>

