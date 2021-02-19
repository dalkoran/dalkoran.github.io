---
layout: post
title: "Extension Methods"
date: 2007-09-11 12:22
author: spencen
comments: true
categories: [.NET, Development]
tags: []
---


This is one feature in C# 3.0 that I've been really looking forward to for a long time.
 

It gives us the ability to extend classes that we didn't author and can't inherit. One of the key benefits I see in this is the ability to extend primitive .NET classes such as String, Int32 etc. These classes can't be inherited so there hasn't previously been a neat way of extending them.
 

The following example adds a rather simplistic method to the System.Int32 type - IsPrime which simply returns true or false depending upon whether the integer is a prime number. [Ok - I know the method is half done and performs poorly - its meant to explain extension methods not prime number evaluation algorithms].


<font face="Verdana" size="2">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(0,0,255)">class</span> </font><font face="Verdana" size="2"><span style="color: rgb(43,145,175)">ExtensionTest
</span>    {
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(0,0,255)">bool</span> IsPrime(<span style="color: rgb(0,0,255)">this</span> <span style="color: rgb(0,0,255)">int</span> value)
{
<span style="color: rgb(0,0,255)">for</span> (<span style="color: rgb(0,0,255)">int</span> denominator = 2; denominator &lt;= value / 2; denominator++)
{
<span style="color: rgb(0,0,255)">if</span> (value % denominator == 0)
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(0,0,255)">false</span>;
}
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(0,0,255)">true</span>;
}
}</font></font></pre><pre class="code"><font face="Verdana" size="2">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">class</span> </font><font size="2"><font face="Verdana"><span style="color: rgb(43,145,175)">Test
</span>    {
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">void</span> TestMethod()
{
<span style="color: rgb(0,0,255)">for</span> (<span style="color: rgb(0,0,255)">int</span> i = 0; i &lt; 100; i++)
{
<span style="color: rgb(0,0,255)">if</span> (i.IsPrime())
System.Diagnostics.<span style="color: rgb(43,145,175)">Debug</span>.Write(i + <span style="color: rgb(163,21,21)">", "</span>);
}
}
}</font></font>



What I really love about the .NET implementation is the way in which extension methods are understood by the IDE intellisense and how you bring the extension methods into scope by importing (using) the namespace. This means the you only see the relevant methods based on the current namespace scope.



The intellisense displays a little arrow against the standard method icon to indicate that the method in question has been extended by a static extension method. Very cool.



&nbsp;![Intellisense drop down showing IsPrime extension method](/images/Intellisense%20drop%20down%20showing%20IsPrime%20extension%20method_1.png)


