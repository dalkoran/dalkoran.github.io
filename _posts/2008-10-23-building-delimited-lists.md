---
title: "Building delimited lists"
date: 2008-10-23 12:50
author: spencen
comments: true
categories: [.NET, Development]
tags: []
---


As developers we often find ourselves writing the same simple bits of logic over and over again. Today at work I noticed one of my co-workers (let’s call him [Benjy](http://www.citv.co.uk/static/engie/index.html)) writing a very simple routine to format some output for diagnostics. It went something like this:
  

<font face="Verdana" size="1">        <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(0,0,255)">string</span> FormatAsCommaDelimitedList(<span style="color: rgb(0,0,255)">object</span>[] args)
{
<span style="color: rgb(0,0,255)">var</span> builder = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">StringBuilder</span>();
<span style="color: rgb(0,0,255)">foreach</span> (<span style="color: rgb(0,0,255)">object</span> arg <span style="color: rgb(0,0,255)">in</span> args)
{
builder.Append( (builder.Length &gt; 0 ? <span style="color: rgb(163,21,21)">&quot;, &quot;</span> : <span style="color: rgb(163,21,21)">&quot;&quot;</span>) + arg.ToString());
}
<span style="color: rgb(0,0,255)">return</span> builder.ToString();
}</font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    I commented that whilst his implementation was fine (and I’m sure we’ve all written something similar in the past) it annoyed me that the condition within the loop made it look so ungainly. It was then that a few things clicked. I mentioned that if it were a simple array of strings we could concatenate using String.Join(). Benjy then applied his philosophy of “thou should never write in a loop what can better be expressed as a query”. And thus was born the following:
    
<pre class="code"><font face="Verdana" size="1">        <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(0,0,255)">string</span> AsCommaDelimitedList&lt;T&gt;(<span style="color: rgb(43,145,175)">IEnumerable</span>&lt;T&gt; items)
{
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(43,145,175)">String</span>.Join( <span style="color: rgb(163,21,21)">&quot;, &quot;</span>, items.Select&lt;T, <span style="color: rgb(0,0,255)">string</span>&gt;( item =&gt; item.ToString() ).ToArray() );
}</font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    Not satisfied with that, Benjy went on to create a neat little CSV routine expressed as an extension method so as to be available to any enumerable collection of objects.
    
<pre class="code"><font face="Verdana" size="1">       <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(0,0,255)">string</span> ToCSV&lt;T&gt;(<span style="color: rgb(0,0,255)">this</span> <span style="color: rgb(43,145,175)">IEnumerable</span>&lt;T&gt; items, <span style="color: rgb(0,0,255)">string</span> delimiter, <span style="color: rgb(0,0,255)">bool</span> quoteAll)
{
<span style="color: rgb(0,0,255)">string</span> quoteChar = <span style="color: rgb(163,21,21)">&quot;\&quot;&quot;</span>;
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(43,145,175)">String</span>.Join(
delimiter,
items.Select&lt;T, <span style="color: rgb(0,0,255)">string</span>&gt;(
item =&gt; item.ToString().Contains(quoteChar) || quoteAll
? <span style="color: rgb(43,145,175)">String</span>.Format(<span style="color: rgb(163,21,21)">&quot;{0}{1}{0}&quot;</span>, quoteChar, item)
: item.ToString()
).ToArray()
);
}</font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    If you ignore his ghastly formatting (and the bug which I’m sure is just there to test me) then that’s pretty cute.
    

    
    I figured at this point I ought to at least pretend to contribute something – so here’s a version that allows the object to be formatted. Works with IFormattable types like Int32, Decimal, Float, DateTime etc.
    
<pre class="code"><font face="Verdana" size="1">        <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(0,0,255)">string</span> ToFormattedList&lt;T&gt;(<span style="color: rgb(0,0,255)">this</span> <span style="color: rgb(43,145,175)">IEnumerable</span>&lt;T&gt; items, <span style="color: rgb(0,0,255)">string</span> delimiter, <span style="color: rgb(0,0,255)">string</span> format)
<span style="color: rgb(0,0,255)">where</span> T : </font><font size="1"><font face="Verdana"><span style="color: rgb(43,145,175)">IFormattable
</span>        {
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(43,145,175)">String</span>.Join(
delimiter,
items.Select&lt;T, <span style="color: rgb(0,0,255)">string</span>&gt;(
item =&gt; item.ToString(format, System.Globalization.<span style="color: rgb(43,145,175)">CultureInfo</span>.CurrentCulture)
).ToArray());
}</font></font></pre>

    
    In deference to Benjy (a strong believer in console applications and mono-spaced fonts) here’s the simple Console Main() I used to test these:
    
<pre class="code"><font face="Verdana" size="1">        <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(0,0,255)">void</span> Main(<span style="color: rgb(0,0,255)">string</span>[] args)
{
<span style="color: rgb(43,145,175)">Console</span>.WriteLine(args.ToCSV(<span style="color: rgb(163,21,21)">&quot;, &quot;</span>, <span style="color: rgb(0,0,255)">true</span>));
<span style="color: rgb(43,145,175)">Console</span>.WriteLine(args.AsCommaDelimitedList());
<span style="color: rgb(0,0,255)">var</span> amounts = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(0,0,255)">decimal</span>[] { 2.1m, 4, 4.5m, 9, 3.333m, 4.14m };
<span style="color: rgb(43,145,175)">Console</span>.WriteLine(amounts.ToCSV(<span style="color: rgb(163,21,21)">&quot;, &quot;</span>, <span style="color: rgb(0,0,255)">true</span>));
<span style="color: rgb(43,145,175)">Console</span>.WriteLine(amounts.AsCommaDelimitedList());
<span style="color: rgb(43,145,175)">Console</span>.WriteLine(amounts.ToFormattedList(<span style="color: rgb(163,21,21)">&quot;  |  &quot;</span>, <span style="color: rgb(163,21,21)">&quot;$#,##0.00&quot;</span>));
<span style="color: rgb(43,145,175)">Console</span>.ReadLine();
}</font>

<a href="http://11011.net/software/vspaste"></a>


And the corresponding output in all its dumb terminal glory.



>


<font face="Courier New" size="2">&quot;The&quot;, &quot;quick&quot;, &quot;brown&quot;, &quot;fox&quot;, &quot;jumped&quot;, &quot;over&quot;, &quot;the&quot;, &quot;lazy&quot;, &quot;dog.&quot;
  
The, quick, brown, fox, jumped, over, the, lazy, dog.
  
&quot;2.1&quot;, &quot;4&quot;, &quot;4.5&quot;, &quot;9&quot;, &quot;3.333&quot;, &quot;4.14&quot;
  
2.1, 4, 4.5, 9, 3.333, 4.14
  
$2.10&#160; |&#160; $4.00&#160; |&#160; $4.50&#160; |&#160; $9.00&#160; |&#160; $3.33&#160; |&#160; $4.14</font>






On the subject of formatting I enjoyed this blog post on [Visual Studio Debugging Tips](http://blogesh.wordpress.com/2008/09/09/visual-studio-debugging-tips-and-tricks/). Particularly the explanation of how to use conditional formatting with the DebuggerDisplay attribute. Worth a look!


