---title: "HTML to FlowDocument Converter"
date: 2008-01-19 13:52
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---
I have an existing application that I wrote that stores "notes" in HTML format. I actually used the embedded HTML editor in IE to allow the user to create text - similar to what a RichText control would be used for but outputting to HTML rather than (the even more horrid) RTF.
 

I'm now in the process of upgrading that application to WPF, so its only natural that I want to display these notes using one of the WPF FlowDocument viewer controls. The problem I encountered was how to convert my HTML to something that could be nicely displayed in the FlowDocument?
 

**Step 1 - Converting HTML to XAML**
 

The solution was to download Microsoft's sample [HtmlToXaml Converter](http://blogs.msdn.com/wpfsdk/archive/2006/05/25/606317.aspx) (which actually allows conversion in both directions). Its apparently not foolproof but its certainly more than enough to convert my very simple HTML to the corresponding FlowDocument. 
 

Using the HtmlToXamlConverter classes ConvertHtmlToXaml we can take a HTML string and convert to a XAML string, e.g. from:
<span style="font-size: 8pt; font-family: verdana"> 

&nbsp;&nbsp;&nbsp; &lt;p&gt;The &lt;b&gt;Markup&lt;/b&gt; that is to be converted.&lt;/p&gt;
</span> 

to:


<span style="font-size: 8pt; font-family: verdana">    &lt;FlowDocument&gt;<br>        &lt;Paragraph&gt;The &lt;Run FontWeight="bold"&gt;Markup&lt;/Run&gt; that is to be converted.&lt;/Paragraph&gt;<br>    &lt;/FlowDocument&gt;</span></pre>

    
    **Step 2 - Converting XAML markup into a FlowDocument instance**
    

    
    This was certainly a great start but it converts HTML text to XAML text - not actual objects. So the next hurdle was how to convert the XAML document markup at runtime and insert within a FlowDocument?
    

    
    The solution was fairly easy to find thanks to Google and [Ronald Clifford](http://blog.roncli.com/2007/12/databinding-to-xaml-flowdocument.html) - although I can't say I think its obvious.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">    <span style="color: rgb(43,145,175)">FlowDocument</span> flowDocument = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">FlowDocument</span>();
<span style="color: rgb(0,0,255)">string</span> xaml = "&lt;p&gt;The &lt;b&gt;Markup&lt;/b&gt; that is to be converted.&lt;/p&gt;";
<span style="color: rgb(0,0,255)">using</span> (<span style="color: rgb(43,145,175)">MemoryStream</span> msDocument = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">MemoryStream</span>((<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ASCIIEncoding</span>()).GetBytes(xaml)))
{
<span style="color: rgb(43,145,175)">TextRange</span> textRange = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">TextRange</span>(flowDocument.ContentStart, flowDocument.ContentEnd);
textRange.Load(msDocument, <span style="color: rgb(43,145,175)">DataFormats</span>.Xaml);
}</span></pre><a href="http://11011.net/software/vspaste"></a>

    
    **Step 3 - Using DataBinding**
    

    
    So things were almost coming together. The last step was to perform this conversion as easily as possible - which meant using DataBinding. I had a list of items displayed, each with a field containing a string field that contained the HTML markup (as populated from a database using LINQ). So I wanted to be able to bind the FlowDocumentScrollViewer to the field containing the HTML and for things to work themselves out automatically. This required yet another IValueConverter class to use on the data binding.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">HtmlToFlowDocumentConverter</span> : <span style="color: rgb(43,145,175)">IValueConverter
</span>    {
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">object</span> Convert(<span style="color: rgb(0,0,255)">object</span> value, <span style="color: rgb(43,145,175)">Type</span> targetType, <span style="color: rgb(0,0,255)">object</span> parameter, <br>                              System.Globalization.<span style="color: rgb(43,145,175)">CultureInfo</span> culture)
{
<span style="color: rgb(0,0,255)">if</span> (value != <span style="color: rgb(0,0,255)">null</span>)
{
<span style="color: rgb(43,145,175)">FlowDocument</span> flowDocument = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">FlowDocument</span>();
<span style="color: rgb(0,0,255)">string</span> xaml = <span style="color: rgb(43,145,175)">HtmlToXamlConverter</span>.ConvertHtmlToXaml(value.ToString(), <span style="color: rgb(0,0,255)">false</span>);
<span style="color: rgb(0,0,255)">using</span> (<span style="color: rgb(43,145,175)">MemoryStream</span> stream = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">MemoryStream</span>((<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ASCIIEncoding</span>()).GetBytes(xaml)))
{
<span style="color: rgb(43,145,175)">TextRange</span> text = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">TextRange</span>(flowDocument.ContentStart, flowDocument.ContentEnd);
text.Load(stream, <span style="color: rgb(43,145,175)">DataFormats</span>.Xaml);
}
<span style="color: rgb(0,0,255)">return</span> flowDocument;
}
<span style="color: rgb(0,0,255)">return</span> value;
}
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">object</span> ConvertBack(<span style="color: rgb(0,0,255)">object</span> value, <span style="color: rgb(43,145,175)">Type</span> targetType, <span style="color: rgb(0,0,255)">object</span> parameter, <br>                                  System.Globalization.<span style="color: rgb(43,145,175)">CultureInfo</span> culture)
{
<span style="color: rgb(0,0,255)">throw</span> <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">NotImplementedException</span>();
}
}</span>
<a href="http://11011.net/software/vspaste"></a>


**Note**: The HtmlToXmlConverter class above is simply the [Microsoft sample](http://blogs.msdn.com/wpfsdk/archive/2006/05/25/606317.aspx).



**The Result**



This means I can now use this converter anywhere in my XAML that I wish to display HTML content within a FlowDocument control.



<span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Page.Resources</span><span style="color: rgb(0,0,255)">&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">conv</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">HtmlToFlowDocumentConverter</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Key</span><span style="color: rgb(0,0,255)">="htmlToXamlConverter"/&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Page.Resources</span><span style="color: rgb(0,0,255)">&gt;<br>&nbsp;&nbsp;&nbsp; ...<br></span><span style="color: rgb(0,0,255)">&nbsp;&nbsp;&nbsp; &lt;</span><span style="color: rgb(163,21,21)">FlowDocumentScrollViewer</span><span style="color: rgb(255,0,0)"> Document</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=Events</span>/<span style="color: rgb(0,0,255)">Diaries</span>/<span style="color: rgb(0,0,255)">Comment,</span><span style="color: rgb(255,0,0)"> <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Converter</span><span style="color: rgb(0,0,255)">={</span><span style="color: rgb(163,21,21)">StaticResource</span><span style="color: rgb(255,0,0)"> htmlToXamlConverter</span><span style="color: rgb(0,0,255)">}}"/&gt;</span></span>


