---
layout: post
title: "Problems binding to SelectedValue with Microsoft’s WPF DataGrid"
date: 2009-04-30 13:17
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


I had been seeing some odd exceptions being thrown by the WPF DataGrid code when interacting with the “new row” place holder. 
  

![GridEditing - SelectedItem FormatException](/images/GridEditing%20-%20SelectedItem%20FormatException_1.png "GridEditing - SelectedItem FormatException") 
  

I could identify that the error was occurring because I had data-bound to the SelectedItem property on the DataGrid like so:
  

<font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGrid </span><span style="color: red">ItemsSource</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">Persons</span><span style="color: blue">}&quot; </span><span style="color: red">AutoGenerateColumns</span><span style="color: blue">=&quot;False&quot;</span></font></font><span style="color: blue">
</span><font size="1"><font face="Verdana"><span style="color: blue">                  </span><span style="color: red">SelectedItem</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">SelectedPerson</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;
</span><span style="color: red">IsSynchronizedWithCurrentItem</span><span style="color: blue">=&quot;True&quot;&gt;  
        …</span></font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    Both the Persons collection and SelectedPerson are properties on my ViewModel (VM). Its possible to use CollectionViewSource.GetDefaultView(Persons).CurrentItem – but I find it useful to expose and bind a simple read/write property. I’ve used this previously for ListView and ListBox without a problem.
    

    
    I spent some time debugging this right down through BindingExpression and DependencyObject.SetValue. As far as I can tell the exception is thrown because a ConvertBack method (on the default converter) fails when dealing with the MS.Internal.NamedObject that represents the NewItemPlaceholder. This instance is used to represent the blank “new row” if CanUserAddRows is set to True (and the collection supports it). In fact it appears as if the FormatException is actually being thrown within an exception handler whilst attempting to Trace the binding failure. Whoops!
    

    
    Initially I tried simply putting an try/catch block around the DataGrid code shown above. However, the exception occurred under various conditions – focus on new row, begin edit on a new row and rollback on a new row. Not all of these could be easily caught because they would leave the grid in an invalid state. Eventually the answer (HACK) became obvious – to use a converter on the binding.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">using </span>System;
<span style="color: blue">using </span>System.Windows;
<span style="color: blue">using </span>System.Windows.Data;
<span style="color: blue">namespace </span>GridEditing.Converters
{
<span style="color: blue">public class </span><span style="color: #2b91af">IgnoreNewItemPlaceHolderConverter </span>: </font></font><font size="1"><font face="Verdana"><span style="color: #2b91af">IValueConverter
</span>{
<span style="color: blue">private const string </span>NewItemPlaceholderName = <span style="color: #a31515">&quot;{NewItemPlaceholder}&quot;</span>;
<span style="color: blue">public object </span>Convert( <span style="color: blue">object </span>value, <span style="color: #2b91af">Type </span>targetType, <span style="color: blue">object </span>parameter, System.Globalization.<span style="color: #2b91af">CultureInfo </span>culture )
{
<span style="color: blue">return </span>value;
}
<span style="color: blue">public object </span>ConvertBack( <span style="color: blue">object </span>value, <span style="color: #2b91af">Type </span>targetType, <span style="color: blue">object </span>parameter, System.Globalization.<span style="color: #2b91af">CultureInfo </span>culture )
{
<span style="color: blue">if </span>( value != <span style="color: blue">null </span>&amp;&amp; value.ToString() == NewItemPlaceholderName )
<span style="color: blue">return </span><span style="color: #2b91af">DependencyProperty</span>.UnsetValue;
<span style="color: blue">return </span>value;
}
}
}</font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    All we are doing here is **not** binding when we encounter the “new row” instance. Notice the curly braces in the NewItemPlaceholder ToString() representation? This, I believe, is why it causes the FormatException since the ToString() is used to construct a formatString which is then passed to the TraceEvent method. However, because the curly braces aren’t escaped it expects a string token number, e.g. {0} as per string.Format().
    

    
    Anyhow, using the converter above means that binding to DataGrid.SelectedValue works as expected with the “new row” place holder.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">Window.Resources</span><span style="color: blue">&gt;</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">
&lt;</span><span style="color: #a31515">converters</span><span style="color: blue">:</span><span style="color: #a31515">IgnoreNewItemPlaceHolderConverter </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;ignoreNewItemPlaceHolderConverter&quot;/&gt;
&lt;/</span><span style="color: #a31515">Window.Resources</span><span style="color: blue">&gt;</span></font></font></pre>
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGrid </span><span style="color: red">ItemsSource</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">Persons</span><span style="color: blue">}&quot; </span><span style="color: red">AutoGenerateColumns</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;False&quot;  
                      </span><span style="color: red">SelectedItem</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">SelectedPerson</span><span style="color: blue">,</span><span style="color: red">Converter</span><span style="color: blue">={</span><span style="color: #a31515">StaticResource </span><span style="color: red">ignoreNewItemPlaceHolderConverter</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}}&quot;
</span><span style="color: red">IsSynchronizedWithCurrentItem</span><span style="color: blue">=&quot;True&quot;&gt;  
        …</span></font></font>

<a href="http://11011.net/software/vspaste"></a><a href="http://11011.net/software/vspaste"></a>

