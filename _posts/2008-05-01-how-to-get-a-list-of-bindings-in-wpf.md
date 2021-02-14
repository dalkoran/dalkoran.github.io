---
layout: post
title: "How to get a list of Bindings in WPF?"
date: 2008-05-01 15:46
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


Tonight I was struggling with how to get a list of Bindings defined within a WPF form. Why do I want to do this? Well there are two reasons.
 

1.  I want to be able to evaluate each binding and use the meta-data defined on the business entity to "decorate" the bound control. For example, a description of the business entity property (defined using an attribute) could be applied as the default tooltip on the control to which it is bound. This can (and in the WinForms world I have) be taken much further to determine the data source for lookup fields, label text, maximum input lengths or masks etc.  If I validate a business entity and it returns a list of validation errors I want to be able to use the validation "context" (the instance type and property name) to be able to highlight the relevant bound control (if any) as being in error. 

In WinForms determining all Bindings for a given top level control is reasonably straight forward by iterating through the controls and examining the BindingContext's CurrencyManager's Bindings collection. Normally a WinForm will only have a small number (e.g. 1) BindingContext that is inherited by child container controls.
 

In WPF I couldn't find anything obvious. There is a likely sounding BindingOperations static class which provides several helper methods but none seem to do the trick.
 

The solution I ended up with before resorting to Google was to use reflection as follows...


<span style="font-size: 7pt; font-family: verdana"><span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(43,145,175)">ValidationBindingContext</span> GetBindingForProperty(<span style="color: rgb(43,145,175)">Type</span> boundType, <span style="color: rgb(0,0,255)">string</span> boundPropertyName, <br>                                                                                                          <span style="color: rgb(43,145,175)">FrameworkElement</span> root)
{
<span style="color: rgb(0,0,255)">foreach</span> (<span style="color: rgb(43,145,175)">FrameworkElement</span> element <span style="color: rgb(0,0,255)">in</span> <span style="color: rgb(43,145,175)">LogicalTreeHelper</span>.GetChildren(root).OfType&lt;<span style="color: rgb(43,145,175)">FrameworkElement</span>&gt;())
{
<span style="color: rgb(43,145,175)">FieldInfo</span>[] properties = element.GetType().GetFields(<span style="color: rgb(43,145,175)">BindingFlags</span>.Public | <span style="color: rgb(43,145,175)">BindingFlags</span>.GetProperty | <br>                                                                                                  <span style="color: rgb(43,145,175)">BindingFlags</span>.Static | <span style="color: rgb(43,145,175)">BindingFlags</span>.FlattenHierarchy);
<span style="color: rgb(0,0,255)">foreach</span> (<span style="color: rgb(43,145,175)">FieldInfo</span> field <span style="color: rgb(0,0,255)">in</span> properties)
{
<span style="color: rgb(0,0,255)">if</span> (field.FieldType == <span style="color: rgb(0,0,255)">typeof</span>(<span style="color: rgb(43,145,175)">DependencyProperty</span>))
{
<span style="color: rgb(43,145,175)">DependencyProperty</span> dp = (<span style="color: rgb(43,145,175)">DependencyProperty</span>)field.GetValue(<span style="color: rgb(0,0,255)">null</span>);
<span style="color: rgb(0,0,255)">if</span> (<span style="color: rgb(43,145,175)">BindingOperations</span>.IsDataBound(element, dp))
{
<span style="color: rgb(43,145,175)">BindingExpression</span> bindingExpression = <span style="color: rgb(43,145,175)">BindingOperations</span>.GetBindingExpression(element, dp);
<span style="color: rgb(0,0,255)">if</span> (boundType == bindingExpression.DataItem.GetType())
{
<span style="color: rgb(0,0,255)">if</span> (boundPropertyName == bindingExpression.ParentBinding.Path.Path)
{
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ValidationBindingContext</span>(bindingExpression, element <span style="color: rgb(0,0,255)">as</span> <span style="color: rgb(43,145,175)">UIElement</span>);
}
}
}
}
}
<span style="color: rgb(0,128,0)">// Not found so check all child elements
</span>        <span style="color: rgb(43,145,175)">ValidationBindingContext</span> bindingContext = GetBindingForProperty(boundType, boundPropertyName, element);
<span style="color: rgb(0,0,255)">if</span> (bindingContext != <span style="color: rgb(0,0,255)">null</span>) <span style="color: rgb(0,0,255)">return</span> bindingContext;
}
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(0,0,255)">null</span>;
}</span></pre><a href="http://11011.net/software/vspaste"></a>

    
    I thought maybe it would look less horrendous if I used LINQ...
    <pre class="code"><span style="font-size: 7pt; font-family: verdana"><span style="color: rgb(0,0,255)">private static</span> <span style="color: rgb(43,145,175)">ValidationBindingContext</span> GetBindingForPropertyLINQ(<span style="color: rgb(43,145,175)">Type</span> boundType, <span style="color: rgb(0,0,255)">string</span> boundPropertyName, <br>                                                                                                                   <span style="color: rgb(43,145,175)">FrameworkElement</span> root)
{
<span style="color: rgb(0,0,255)">foreach</span> (<span style="color: rgb(43,145,175)">FrameworkElement</span> element <span style="color: rgb(0,0,255)">in</span> <span style="color: rgb(43,145,175)">LogicalTreeHelper</span>.GetChildren(root).OfType&lt;<span style="color: rgb(43,145,175)">FrameworkElement</span>&gt;())
{
<span style="color: rgb(0,0,255)">var</span> result =
<span style="color: rgb(0,0,255)">from</span> <span style="color: rgb(43,145,175)">FieldInfo</span> field <span style="color: rgb(0,0,255)">in</span> element.GetType().GetFields(<span style="color: rgb(43,145,175)">BindingFlags</span>.Public | <span style="color: rgb(43,145,175)">BindingFlags</span>.GetProperty | <br>                                                                                                           <span style="color: rgb(43,145,175)">BindingFlags</span>.Static | <span style="color: rgb(43,145,175)">BindingFlags</span>.FlattenHierarchy)
<span style="color: rgb(0,0,255)">where</span> field.FieldType == <span style="color: rgb(0,0,255)">typeof</span>(<span style="color: rgb(43,145,175)">DependencyProperty</span>)
<span style="color: rgb(0,0,255)">let</span> dp = (<span style="color: rgb(43,145,175)">DependencyProperty</span>)field.GetValue(<span style="color: rgb(0,0,255)">null</span>)
<span style="color: rgb(0,0,255)">where</span> <span style="color: rgb(43,145,175)">BindingOperations</span>.IsDataBound(element, dp)
<span style="color: rgb(0,0,255)">let</span> bindingExpression = <span style="color: rgb(43,145,175)">BindingOperations</span>.GetBindingExpression(element, dp)
<span style="color: rgb(0,0,255)">where</span> boundType == bindingExpression.DataItem.GetType()
<span style="color: rgb(0,0,255)">where</span> boundPropertyName == bindingExpression.ParentBinding.Path.Path
<span style="color: rgb(0,0,255)">select</span> <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ValidationBindingContext</span>(bindingExpression, element <span style="color: rgb(0,0,255)">as</span> <span style="color: rgb(43,145,175)">UIElement</span>);
<span style="color: rgb(0,0,255)">if</span> (result.FirstOrDefault() != <span style="color: rgb(0,0,255)">null</span>)
<span style="color: rgb(0,0,255)">return</span> result.FirstOrDefault();
<span style="color: rgb(0,0,255)">else
</span>        {
<span style="color: rgb(0,128,0)">// Not found so check all child elements
</span>            <span style="color: rgb(43,145,175)">ValidationBindingContext</span> bindingContext = GetBindingForProperty(boundType, boundPropertyName, element);
<span style="color: rgb(0,0,255)">if</span> (bindingContext != <span style="color: rgb(0,0,255)">null</span>) <span style="color: rgb(0,0,255)">return</span> bindingContext;
}
}
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(0,0,255)">null</span>;
}</span>
<a href="http://11011.net/software/vspaste"><a href="http://11011.net/software/vspaste"></a>


But no - it was still going to suck, the performance would be horrible and the whole thing just looks so... "kludgy". So, resigned I look on the net - horrified to find that the reflection method was one of two ways developers had commonly resorted to. The other method is to make use of the MarkupWriter class - which to me looked even worse ![](http://blog.spencen.com/emoticons/sad.png).



It works...



![Discovering WPF Bindings](http://blog.spencen.com/images/83489-72989/Discovering%20WPF%20Bindings_3.png) 



...but does someone know a better way?


