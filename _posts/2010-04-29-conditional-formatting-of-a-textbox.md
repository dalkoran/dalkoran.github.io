---
title: "Conditional Formatting of a TextBox"
date: 2010-04-29 04:52
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


![FormatAndPaste](/images/FormatAndPaste_3.png "FormatAndPaste") 
  

I recently came across a scenario where I needed to bind a TextBox to a domain property but also have the value formatted for display. To make things more interesting the format was to be dynamic and the value needed to be editable.
  

The initial investigation led me to consider a ValueConverter. Ideally the TextBox.Text property could be bound and the Converter could be used to format to/from the required on-screen value. For a dynamic format it would be nice to bind the ConverterParameter to a property that exposed the format. Of course that doesn’t work because [ConverterParameter doesn’t support data binding](http://social.msdn.microsoft.com/Forums/en-US/wpf/thread/88a22766-5e6f-4a16-98a6-1ab39877dd09). I found a [hack that gets around this](http://marlongrech.wordpress.com/2008/08/03/my-wish-came-true-i-can-now-use-databinding-in-a-converterparameter/) – but it isn't pretty. There are also some examples of using a [MultiValueConverter](http://social.msdn.microsoft.com/Forums/en-US/wpf/thread/d6a95f05-4338-44a4-a834-bbfe71e893ac/) and passing both the value to format and the format string itself as separate individual bindings. This approach has some difficulties too when converting both ways and its just feels like an abuse of the ValueConverter.
  

This lead me to think about the problem a little more… maybe a different approach is required? Thinking back to the WinForms days and I realised that I had solved this problem before, several times in fact. My approach to this problem for WinForms had been:
  

*   Subclass TextBox and add a Value property of type object that allows data binding to data types other than just string. Common types that could be used with a TextBox include int, decimal, double, bool, DateTime and enums. 
*   The inherited TextBox also has a Format property. On GotFocus the Value property is formatted and used to populate the Text property. On LostFocus the reverse happens, the Text property is parsed back into the Value property. Of course this requires the data type to be known so a DataType property is required as well.   

The benefits that this has:
  

*   TextBox works for data types other than string. 
*   The value is formatted as required for display but upon data entry (GotFocus) the formatting is removed. This actually makes it easier to enter/modify the value because you don’t need to parse currency symbols, percentage signs and the like.   

So the approach sounds good and its worked well for me in WinForms but its… well… not very WPF’ish. Upon starting any major development the first requirement in WinForms was to subclass all the controls – because they were just so lacking if functionality and even more importantly didn’t expose a common set of interfaces. However, I very rarely subclass controls in WPF – instead we can use attached behaviors to extend the control. 
  

The attached behaviors required are:
  

*   <span class="kwrd">object</span> TypedValue</pre>

*   <pre class="csharpcode"><span class="kwrd">Type</span> DataType </pre>

*   <pre class="csharpcode"><span class="kwrd">string</span> StringFormat</pre>


    
    In XAML instead of binding to the TextBox.Text property we bind to the TypedValue attached property. The StringFormat can also be bound. The DataType can be inferred by the TypedValue – but for nullable types its best to be set explicitly. With a sample class as follows:
    
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> ModelItem
{
<span class="kwrd">public</span> <span class="kwrd">object</span> Value { get; set; }
<span class="kwrd">public</span> <span class="kwrd">string</span> Format { get; set; }
}</pre>
<style type="text/css">
.csharpcode, .csharpcode pre
{
font-size: big;
color: black;
font-family: verdana, "Consolas", "Courier New", courier, monospace;
background-color: #ffffff;
/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt
{
background-color: #f4f4f4;
width: 100%;
margin: 0em;
}
.csharpcode .lnum { color: #606060; }</style>

    
    The XAML is then:
    
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">DataGrid</span> <span class="attr">ItemsSource</span><span class="kwrd">=&quot;{Binding Items}&quot;</span> <span class="attr">AutoGenerateColumns</span><span class="kwrd">=&quot;False&quot;</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">DataGrid.Columns</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">DataGridTextColumn</span> <span class="attr">Header</span><span class="kwrd">=&quot;Any Type TextBox&quot;</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">DataGridTextColumn.ElementStyle</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">Style</span> <span class="attr">TargetType</span><span class="kwrd">=&quot;{x:Type TextBlock}&quot;</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">Setter</span> <span class="attr">Property</span><span class="kwrd">=&quot;local:TextBoxExtensions.StringFormat&quot;</span> <span class="attr">Value</span><span class="kwrd">=&quot;{Binding Format}&quot;</span><span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;</span><span class="html">Setter</span> <span class="attr">Property</span><span class="kwrd">=&quot;local:TextBoxExtensions.TypedValue&quot;</span> <span class="attr">Value</span><span class="kwrd">=&quot;{Binding Value}&quot;</span><span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">Style</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">DataGridTextColumn.ElementStyle</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">DataGridTextColumn.EditingElementStyle</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">Style</span> <span class="attr">TargetType</span><span class="kwrd">=&quot;{x:Type TextBox}&quot;</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">Setter</span> <span class="attr">Property</span><span class="kwrd">=&quot;local:TextBoxExtensions.StringFormat&quot;</span> <span class="attr">Value</span><span class="kwrd">=&quot;{Binding Format}&quot;</span><span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;</span><span class="html">Setter</span> <span class="attr">Property</span><span class="kwrd">=&quot;local:TextBoxExtensions.TypedValue&quot;</span> <span class="attr">Value</span><span class="kwrd">=&quot;{Binding Value}&quot;</span><span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">Style</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">DataGridTextColumn.EditingElementStyle</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">DataGridTextColumn</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">DataGridTextColumn</span> <span class="attr">Header</span><span class="kwrd">=&quot;Format&quot;</span> <span class="attr">Binding</span><span class="kwrd">=&quot;{Binding Format}&quot;</span> <span class="attr">IsReadOnly</span><span class="kwrd">=&quot;True&quot;</span><span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;</span><span class="html">DataGridTextColumn</span> <span class="attr">Header</span><span class="kwrd">=&quot;Value&quot;</span> <span class="attr">Binding</span><span class="kwrd">=&quot;{Binding Value}&quot;</span> <span class="attr">IsReadOnly</span><span class="kwrd">=&quot;True&quot;</span><span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">DataGrid.Columns</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">DataGrid</span><span class="kwrd">&gt;</span></pre>

    
    Which generates a DataGrid bound to a collection of ModelItems. Each ModelItem allows a different data type and format to be applied – great for a “user-defined fields” scenario.
    

    
    Populating the ModelItems collection as follows in our main ViewModel:
    
<pre class="csharpcode">    <span class="kwrd">public</span> <span class="kwrd">class</span> Model
{
<span class="kwrd">public</span> Model()
{
Primary = <span class="kwrd">new</span> ModelItem() { Format = <span class="str">&quot;{0:#,##0.0}&quot;</span>, Value = 12345678.765 };
Items = <span class="kwrd">new</span> ObservableCollection&lt;ModelItem&gt;();
Items.Add(<span class="kwrd">new</span> ModelItem() { Format = <span class="str">&quot;{0:C2}&quot;</span>, Value = 123.42 });
Items.Add(<span class="kwrd">new</span> ModelItem() { Format = <span class="str">&quot;{0![](http://blog.spencen.com/emoticons/tongue.png)2}&quot;</span>, Value= 0.125 });
Items.Add(<span class="kwrd">new</span> ModelItem() { Format = <span class="str">&quot;{0}&quot;</span>, Value = <span class="str">&quot;Fred&quot;</span> });
Items.Add(<span class="kwrd">new</span> ModelItem() { Format = <span class="kwrd">null</span>, Value = <span class="kwrd">true</span> });
Items.Add(<span class="kwrd">new</span> ModelItem() { Format = <span class="str">&quot;Uncle {0}&quot;</span>, Value = <span class="str">&quot;George&quot;</span> });
Items.Add(<span class="kwrd">new</span> ModelItem() { Format = <span class="kwrd">null</span>, Value = Colors.Black });
Items.Add(<span class="kwrd">new</span> ModelItem() { Format = <span class="kwrd">null</span>, Value = System.DayOfWeek.Monday });
Items.Add(<span class="kwrd">new</span> ModelItem() { Format = <span class="str">&quot;{0:0;minus 0;zip}&quot;</span>, Value = -123.4 });
}
<span class="kwrd">public</span> ModelItem Primary { get; set; }
<span class="kwrd">public</span> ObservableCollection&lt;ModelItem&gt; Items { get; <span class="kwrd">private</span> set; }
}



Generates the following grid, which allows for editing of the strongly typed values.



![FormatAndPaste](/images/FormatAndPaste_1.png "FormatAndPaste")


