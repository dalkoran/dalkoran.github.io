---
title: "Preventing a bound TextBox from resetting the caret position"
date: 2010-05-07 11:54
author: spencen
comments: true
categories: [.NET, Development, Lab49, WPF]
tags: []
---

Someone posed a question on our internal mailing list today at work that reminded me of a problem I’d tackled previously whilst working as a *[developer of fortune](http://blog.spencen.com/2010/01/28/wrapping-up-a-contract.aspx)*.
  

Here’s the challenge. A TextBox is bound to a data value that is being constantly updated. In my scenario the TextBox was bound to a data feed coming from a serial port connected weigh bridge. Even though the value is being automatically updated the operator has the ability to override the value with their own – at which point it would normally cease being updated by the data service.
  

Sounds fairly straight-forward. The main problem is that every time the TextBox value is updated via data-binding the selection text and position of the caret is reset. This is particularly annoying if the operator positions the caret about to make their change and a fraction of a second before they press a key the caret moves to the left edge.
  

I can’t remember exactly how we solved this problem in my earlier engagement (Raaj if you’re listening you could jog my memory) but here was my quick re-attempt.
  

First I’ll set the scene with a mock environment.
  

<span class="kwrd">public</span> <span class="kwrd">partial</span> <span class="kwrd">class</span> MainWindow : Window
{
<span class="kwrd">private</span> ViewModel viewModel;
<span class="kwrd">private</span> DispatcherTimer timer;
<span class="kwrd">public</span> MainWindow()
{
InitializeComponent();
<span class="kwrd">this</span>.viewModel = <span class="kwrd">new</span> ViewModel();
<span class="kwrd">this</span>.DataContext = viewModel;
<span class="kwrd">this</span>.timer = <span class="kwrd">new</span> DispatcherTimer(  
                <span class="kwrd">new</span> TimeSpan(0, 0, 1),   
                DispatcherPriority.Background,   
                UpdateValue,   
                <span class="kwrd">this</span>.Dispatcher);
}
<span class="kwrd">private</span> <span class="kwrd">void</span> UpdateValue(<span class="kwrd">object</span> sender, EventArgs e)
{
<span class="kwrd">this</span>.viewModel.Value += 0.01;
}
}</pre>
<style type="text/css">
.csharpcode, .csharpcode pre
{
font-size: small;
color: black;
font-family: verdana, consolas, "Courier New", courier, monospace;
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

    
    This sets up a simple form whose DataContext refers to a ViewModel with a Value property. The Value property is updated every second by a thread safe timer.
    
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">Window</span> <span class="attr">x:Class</span><span class="kwrd">=&quot;TextBoxOverlay.MainWindow&quot;</span>
<span class="attr">xmlns</span><span class="kwrd">=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;</span>
<span class="attr">xmlns:x</span><span class="kwrd">=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;</span>
<span class="attr">Title</span><span class="kwrd">=&quot;MainWindow&quot;</span> <span class="attr">Height</span><span class="kwrd">=&quot;350&quot;</span> <span class="attr">Width</span><span class="kwrd">=&quot;525&quot;</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">Grid</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">StackPanel</span> <span class="attr">Width</span><span class="kwrd">=&quot;150&quot;</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">TextBox</span>
<span class="attr">Text</span><span class="kwrd">=&quot;{Binding Value,StringFormat=0.00,UpdateSourceTrigger=PropertyChanged}&quot;</span><span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">StackPanel</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">Grid</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">Window</span><span class="kwrd">&gt;</span></pre>

    
    The XAML simple binds a TextBox to the Value property. Running this sample and the problem can be immediately realised. Attempting to edit the value in the TextBox using the keyboard is extremely frustrating. The caret won’t go where you want it to.
    

    
    So – the next step is to create a TextBlock that overlays the TextBox and instead bind this to the Value property. We set the IsHitTestVisible property on this TextBlock to False so that the user can still interact with the TextBox underneath. Then – and this is where things get a little sneaky – we make the TextBox’s text transparent. This allows us the strange freedom to interact with the TextBox’s content by selecting it and moving the caret – and because we can see the same text in the overlaid TextBlock things appear as normal.
    
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">Window</span> <span class="attr">x:Class</span><span class="kwrd">=&quot;TextBoxOverlay.MainWindow&quot;</span>
<span class="attr">xmlns</span><span class="kwrd">=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;</span>
<span class="attr">xmlns:x</span><span class="kwrd">=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;</span>
<span class="attr">Title</span><span class="kwrd">=&quot;MainWindow&quot;</span> <span class="attr">Height</span><span class="kwrd">=&quot;350&quot;</span> <span class="attr">Width</span><span class="kwrd">=&quot;525&quot;</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">Grid</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">StackPanel</span> <span class="attr">Width</span><span class="kwrd">=&quot;150&quot;</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">Grid</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">TextBox</span>
<span class="attr">Text</span><span class="kwrd">=&quot;{Binding ModifiedValue,StringFormat=0.00,  
                                       UpdateSourceTrigger=PropertyChanged}&quot;</span>
<span class="attr">PreviewTextInput</span><span class="kwrd">=&quot;TextBoxPreviewTextInput&quot;</span>
<span class="attr">Foreground</span><span class="kwrd">=&quot;Transparent&quot;</span><span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;</span><span class="html">TextBlock</span>
<span class="attr">IsHitTestVisible</span><span class="kwrd">=&quot;False&quot;</span>
<span class="attr">Margin</span><span class="kwrd">=&quot;5,0&quot;</span>
<span class="attr">Text</span><span class="kwrd">=&quot;{Binding Value,StringFormat=0.00}&quot;</span><span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">Grid</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">StackPanel</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">Grid</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">Window</span><span class="kwrd">&gt;</span></pre>

    
    You can see from the XAML that the TextBox is bound to a new field on our ViewModel called ModifiedValue. We also hook up to the PreviewTextInput event. We could have used an attached behaviour here rather than resorting to code-behind – but I wanted to keep things simple. So the code behind on the form has:
    
<pre class="csharpcode">        <span class="kwrd">private</span> <span class="kwrd">void</span> TextBoxPreviewTextInput(<span class="kwrd">object</span> sender, TextCompositionEventArgs e)
{
var textBox = sender <span class="kwrd">as</span> TextBox;
var selectionStart = textBox.SelectionStart;
var selectionLength = textBox.SelectionLength;
var caretIndex = textBox.CaretIndex;
<span class="kwrd">this</span>.viewModel.ModifiedValue = <span class="kwrd">this</span>.viewModel.Value;
textBox.CaretIndex = caretIndex;   
                textBox.SelectionStart = selectionStart;
textBox.SelectionLength = selectionLength;
}</pre>

    
    Here we save and restore the TextBox’s SelectionStart, SelectionLength and CaretIndex whilst updating the ModifiedValue that is about to be changed to equal the Value that the user can actually see (remember the ModifiedValue is transparent).
    

    
    The very last trick is within the ModifiedValue’s setter where we update the Value property. This ensures that whatever changes the operator makes to the TextBox are visible in the overlaid TextBlock. Of course the whole point of doing all of this is that the caret position and selection remains completely unchanged whilst the value appears to update.
    
<pre class="csharpcode">        <span class="kwrd">public</span> <span class="kwrd">double</span>? ModifiedValue
{
get
{
<span class="kwrd">return</span> <span class="kwrd">this</span>.modifiedValue;
}
set
{
<span class="kwrd">if</span> (<span class="kwrd">this</span>.modifiedValue != <span class="kwrd">value</span>)
{
<span class="kwrd">this</span>.modifiedValue = <span class="kwrd">value</span>;
NotifyPropertyChanged(<span class="str">&quot;ModifiedValue&quot;</span>);
<span class="kwrd">if</span> (<span class="kwrd">this</span>.ModifiedValue.HasValue)
Value = ModifiedValue.Value;
}
}
}</pre>

    
    Source code [here](http://www.spencen.com/Downloads/TextBoxOverlay.zip).
    

    
    So aside from the tacky code-behind to keep the code here to a minimum, I’m wondering if there isn’t a neater solution? 
    

    
    **<font color="#ff0000">UPDATE: Using an attached behaviour</font>**
    

    
    It was pointed out to me by a colleague that there is a simpler, more versatile solution. Simple encapsulate the text change with selection restore within an attached property. Then we can use multiple bindings to achieve the effect.
    
<pre class="csharpcode">        <span class="kwrd">public</span> <span class="kwrd">static</span> <span class="kwrd">string</span> GetNonIntrusiveText(DependencyObject obj)
{
<span class="kwrd">return</span> (<span class="kwrd">string</span>)obj.GetValue(NonIntrusiveTextProperty);
}
<span class="kwrd">public</span> <span class="kwrd">static</span> <span class="kwrd">void</span> SetNonIntrusiveText(DependencyObject obj, <span class="kwrd">string</span> <span class="kwrd">value</span>)
{
obj.SetValue(NonIntrusiveTextProperty, <span class="kwrd">value</span>);
}
<span class="kwrd">public</span> <span class="kwrd">static</span> <span class="kwrd">readonly</span> DependencyProperty NonIntrusiveTextProperty =
DependencyProperty.RegisterAttached(  
                    <span class="str">&quot;NonIntrusiveText&quot;</span>,   
                    <span class="kwrd">typeof</span>(<span class="kwrd">string</span>),   
                    <span class="kwrd">typeof</span>(TextBoxExtensions),
<span class="kwrd">new</span> FrameworkPropertyMetadata(  
                        <span class="kwrd">null</span>,   
                        FrameworkPropertyMetadataOptions.BindsTwoWayByDefault,   
                        NonIntrusiveTextChanged));
<span class="kwrd">public</span> <span class="kwrd">static</span> <span class="kwrd">void</span> NonIntrusiveTextChanged(  
                 <span class="kwrd">object</span> sender,   
                 DependencyPropertyChangedEventArgs e)
{
var textBox = sender <span class="kwrd">as</span> TextBox;
<span class="kwrd">if</span> (textBox == <span class="kwrd">null</span>) <span class="kwrd">return</span>;
var caretIndex = textBox.CaretIndex;
var selectionStart = textBox.SelectionStart;
var selectionLength = textBox.SelectionLength;
textBox.Text = (<span class="kwrd">string</span>) e.NewValue;
textBox.CaretIndex = caretIndex;
textBox.SelectionStart = selectionStart;
textBox.SelectionLength = selectionLength;
}</pre>

    
    Now the XAML no longer requires the tricky TextBlock overlay, we simple have a TextBox with two bindings.
    
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">TextBox</span>
<span class="attr">Text</span><span class="kwrd">=&quot;{Binding Value,StringFormat=0.00,  
                             UpdateSourceTrigger=PropertyChanged,  
                             Mode=OneWayToSource}&quot;</span>
<span class="attr">local:TextBoxExtensions</span>.<span class="attr">NonIntrusiveText</span><span class="kwrd">=&quot;{Binding Value,StringFormat=0.00,  
                                                                              UpdateSourceTrigger=PropertyChanged,  
                                                                              Mode=TwoWay}&quot;</span><span class="kwrd">/&gt;</span>



