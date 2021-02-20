---
title: "I Command Thee - Exception?"
date: 2007-09-23 21:51
author: spencen
comments: true
categories: [.NET, Development, General]
tags: []
---

Ok - so I'm reading through [WPF Unleashed](http://blog.spencen.com/2007/08/28/wpf-book.aspx) and I finish reading the chapter on the various deployment types which also covers the inbuilt page navigation framework. Now this is something that I'm very interested in having attempted the feat myself a few times. So I decide its time to stop reading, roll up the sleeves and give it a try.
 

To start with I try something like this:
 

The startup XAML defines we're using a NavigationWindow with the startup page being HomePage.xaml.


<font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">NavigationWindow</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Class</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="WpfParameterPassing.Window1"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">x</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml"
</span>   <span style="color: rgb(255,0,0)"> Title</span><span style="color: rgb(0,0,255)">="Window1"</span><span style="color: rgb(255,0,0)"> Height</span><span style="color: rgb(0,0,255)">="300"</span><span style="color: rgb(255,0,0)"> Width</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="300"
</span>   <span style="color: rgb(255,0,0)"> Source</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="HomePage.xaml"&gt;
&lt;/</span><span style="color: rgb(163,21,21)">NavigationWindow</span><span style="color: rgb(0,0,255)">&gt;</span></font></font></pre>

    
    The HomePage contains a Button and a TextBox. The idea is that the Button triggers the navigation passing the content of the TextBox to the called page.<a href="http://11011.net/software/vspaste"></a>
    

    
    <span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Page</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Class</span><span style="color: rgb(0,0,255)">="WpfParameterPassing.HomePage"<br></span>&nbsp;&nbsp; <span style="color: rgb(255,0,0)">xmlns</span><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml/presentation"<br></span>&nbsp;&nbsp; <span style="color: rgb(255,0,0)">xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">x</span><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml"<br></span>&nbsp;&nbsp; <span style="color: rgb(255,0,0)">xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">local</span><span style="color: rgb(0,0,255)">="clr-namespace:WpfParameterPassing"<br></span>&nbsp;&nbsp; <span style="color: rgb(255,0,0)">Title</span><span style="color: rgb(0,0,255)">="HomePage"<br></span>&nbsp;&nbsp; <span style="color: rgb(255,0,0)">x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="_this"&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">StackPanel</span><span style="color: rgb(0,0,255)">&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Button</span><span style="color: rgb(255,0,0)"> Content</span><span style="color: rgb(0,0,255)">="Make Selection"</span><span style="color: rgb(255,0,0)"> Click</span><span style="color: rgb(0,0,255)"> ="Button_Click"/&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TextBox</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="selectionTextBox"/&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp; </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">StackPanel</span><span style="color: rgb(0,0,255)">&gt;<br>&lt;/</span><span style="color: rgb(163,21,21)">Page</span><span style="color: rgb(0,0,255)">&gt;</span>
    

    
    <span style="color: rgb(0,0,255)"></span>
    

    
    The page to call is defined as a PageFunction which means it allows a value to be returned. The type of this return value is defined by the x:TypeArguments attribute, in this case a string. The called form just has an Ok and Cancel button, together with a TextBox that is populated with the input parameter and which is used to populate the return value.
    <pre class="code"><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">PageFunction</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Class</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="WpfParameterPassing.SelectFunction"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">x</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">sys</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="clr-namespace:System;assembly=mscorlib"
</span>   <span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">TypeArguments</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="sys:String"
</span>   <span style="color: rgb(255,0,0)"> Title</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="SelectFunction"
</span>   <span style="color: rgb(255,0,0)"> RemoveFromJournal</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="True"&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">StackPanel</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">WrapPanel</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Label</span><span style="color: rgb(255,0,0)"> Content</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="Choice to return:"/&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TextBox</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="choice"</span><span style="color: rgb(255,0,0)"> HorizontalAlignment</span><span style="color: rgb(0,0,255)">="Stretch"</span><span style="color: rgb(255,0,0)"> Width</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="150"/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">WrapPanel</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">WrapPanel</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Button</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="okButton"</span><span style="color: rgb(255,0,0)"> IsDefault</span><span style="color: rgb(0,0,255)"> ="True"</span><span style="color: rgb(255,0,0)"> Content</span><span style="color: rgb(0,0,255)">="_OK"</span><span style="color: rgb(255,0,0)"> <br>                       HorizontalAlignment</span><span style="color: rgb(0,0,255)">="Right"</span><span style="color: rgb(255,0,0)"> Click</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="Button_Click" /&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Button</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="cancelButton"</span><span style="color: rgb(255,0,0)"> IsCancel</span><span style="color: rgb(0,0,255)">="True"</span><span style="color: rgb(255,0,0)"> Content</span><span style="color: rgb(0,0,255)">="_Cancel"</span><span style="color: rgb(255,0,0)"> <br>                       HorizontalAlignment</span><span style="color: rgb(0,0,255)">="Right"</span><span style="color: rgb(255,0,0)"> Click</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="cancelButton_Click" /&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">WrapPanel</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">StackPanel</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
&lt;/</span><span style="color: rgb(163,21,21)">PageFunction</span></font></font><span style="color: rgb(0,0,255)"><font face="Verdana" size="2">&gt;</font>
</span></pre>

    
    A little bit of code behind for the HomePage is used to trigger the navigation and to subscribe to the Return event so we can update the TextBox with the value returned from the called page.
    <pre class="code"><font face="Verdana" size="2">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">partial</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">HomePage</span> : </font><font size="2"><font face="Verdana"><span style="color: rgb(43,145,175)">Page
</span>    {
<span style="color: rgb(0,0,255)">public</span> HomePage()
{
InitializeComponent();
}
<span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">void</span> Button_Click(<span style="color: rgb(0,0,255)">object</span> sender, <span style="color: rgb(43,145,175)">RoutedEventArgs</span> e)
{
<span style="color: rgb(43,145,175)">SelectFunction</span> nextPage = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">SelectFunction</span>(selectionTextBox.Text);
nextPage.Return += <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ReturnEventHandler</span>&lt;<span style="color: rgb(0,0,255)">string</span>&gt;(nextPage_Return);
<span style="color: rgb(0,0,255)">this</span>.NavigationService.Navigate(nextPage);
}
<span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">void</span> nextPage_Return(<span style="color: rgb(0,0,255)">object</span> sender, <span style="color: rgb(43,145,175)">ReturnEventArgs</span>&lt;<span style="color: rgb(0,0,255)">string</span>&gt; e)
{
<span style="color: rgb(0,0,255)">if</span> (!<span style="color: rgb(0,0,255)">string</span>.IsNullOrEmpty(e.Result))
selectionTextBox.Text = e.Result;
}
}</font></font></pre><a href="http://11011.net/software/vspaste"></a>

    
    Some code behind on the PageFunction derived class is used to wire up the Ok and Cancel buttons. The constructor overload is then used to allow the input parameters to be passed. I like the idea of using constructors to pass the parameters - neat. [Its a shame parameterized constructors can't be used to initialize objects in XAML - but that's a whole other story.]
    <pre class="code"><font face="Verdana" size="2">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">partial</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">SelectFunction</span> : <span style="color: rgb(43,145,175)">PageFunction</span>&lt;<span style="color: rgb(0,0,255)">string</span>&gt;
{
<span style="color: rgb(0,0,255)">public</span> SelectFunction()
{
InitializeComponent();
}
<span style="color: rgb(0,0,255)">public</span> SelectFunction(<span style="color: rgb(0,0,255)">string</span> previousSelection) : <span style="color: rgb(0,0,255)">this</span>()
{
choice.Text = previousSelection;
}
<span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">void</span> Button_Click(<span style="color: rgb(0,0,255)">object</span> sender, <span style="color: rgb(43,145,175)">RoutedEventArgs</span> e)
{
OnReturn(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ReturnEventArgs</span>&lt;<span style="color: rgb(0,0,255)">string</span>&gt;(choice.Text));
}
<span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">void</span> cancelButton_Click(<span style="color: rgb(0,0,255)">object</span> sender, <span style="color: rgb(43,145,175)">RoutedEventArgs</span> e)
{
OnReturn(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ReturnEventArgs</span>&lt;<span style="color: rgb(0,0,255)">string</span>&gt;());
}
}</font></pre><a href="http://11011.net/software/vspaste"></a>

    
    So - put all this together and it works as expected. Enter a value on the first page (HomePage) - click the button and the value is transferred to the second page (SelectFunction) where it can be modified and returned via the return result. 
    

    
    The next step is to get rid of all that code behind that binds HomePage to SelectFunction. What better way to do this than with a Command? This is where things began to get fuzzy. If I want to create my own command what should it derive from? After a few false starts attempting to derive from RoutedCommand etc. I decide not to derive from anything and just implement ICommand myself.
    <pre class="code"><font face="Verdana" size="2">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">SelectCommand</span> : <span style="color: rgb(43,145,175)">ICommand</span>, </font><font size="2"><font face="Verdana"><span style="color: rgb(43,145,175)">INotifyPropertyChanged
</span>    {
<span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">string</span> _selection;
<span style="color: rgb(0,0,255)">public</span> SelectCommand()
{
</font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,128,0)">//Text = "Select Choice";
</span>        }
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(43,145,175)">Page</span> CallingPage { <span style="color: rgb(0,0,255)">get</span>; <span style="color: rgb(0,0,255)">set</span>; }
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">string</span> Selection
{
<span style="color: rgb(0,0,255)">get</span>
{ <span style="color: rgb(0,0,255)">return</span> _selection; }
<span style="color: rgb(0,0,255)">set</span>
{
_selection = <span style="color: rgb(0,0,255)">value</span>;
OnPropertyChanged(<span style="color: rgb(163,21,21)">"Selection"</span>);
}
}
<span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">void</span> OnPropertyChanged(<span style="color: rgb(0,0,255)">string</span> p)
{
<span style="color: rgb(0,0,255)">if</span> (PropertyChanged != <span style="color: rgb(0,0,255)">null</span>)
PropertyChanged(<span style="color: rgb(0,0,255)">this</span>, <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">PropertyChangedEventArgs</span>(p));
}
</font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,128,0)">
</span><span style="color: rgb(0,0,255)">        #region</span> ICommand Members
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">bool</span> CanExecute(<span style="color: rgb(0,0,255)">object</span> parameter)
{
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(0,0,255)">true</span>;
}
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">event</span> <span style="color: rgb(43,145,175)">EventHandler</span> CanExecuteChanged;
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">void</span> Execute(<span style="color: rgb(0,0,255)">object</span> parameter)
{
<span style="color: rgb(0,0,255)">if</span> (CallingPage == <span style="color: rgb(0,0,255)">null</span>)
CallingPage = (<span style="color: rgb(43,145,175)">Page</span>) parameter;
<span style="color: rgb(0,0,255)">if</span> (CallingPage == <span style="color: rgb(0,0,255)">null</span>)
<span style="color: rgb(0,0,255)">throw</span> <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">InvalidOperationException</span>(<br>                                 <span style="color: rgb(163,21,21)">"CallingPage cannot be null when invoking the Execute method."</span>);
<span style="color: rgb(43,145,175)">SelectFunction</span> nextPage = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">SelectFunction</span>(Selection);
nextPage.Return += <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ReturnEventHandler</span>&lt;<span style="color: rgb(0,0,255)">string</span>&gt;(nextPage_Return);
CallingPage.NavigationService.Navigate(nextPage);
}
</font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">        #endregion
</span>        <span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">void</span> nextPage_Return(<span style="color: rgb(0,0,255)">object</span> sender, <span style="color: rgb(43,145,175)">ReturnEventArgs</span>&lt;<span style="color: rgb(0,0,255)">string</span>&gt; e)
{
</font></font><font size="2"><font face="Verdana">            <span style="color: rgb(0,0,255)">if</span> (!<span style="color: rgb(0,0,255)">string</span>.IsNullOrEmpty(e.Result))
Selection = e.Result;
}
<span style="color: rgb(0,0,255)">        #region</span> INotifyPropertyChanged Members
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">event</span> <span style="color: rgb(43,145,175)">PropertyChangedEventHandler</span> PropertyChanged;
</font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">        #endregion
</span>    }</font></font></pre><a href="http://11011.net/software/vspaste"></a>

    
    Get rid of all the code behind in HomePage and redefine the Button and TextBox to instantiate a new command and a bit of binding to glue it together.
    <a href="http://11011.net/software/vspaste"></a>

    
    <span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Button</span><span style="color: rgb(255,0,0)"> Content</span><span style="color: rgb(0,0,255)">="Make Selection"</span><span style="color: rgb(255,0,0)"> CommandParameter</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> ElementName</span><span style="color: rgb(0,0,255)">=</span>_<span style="color: rgb(0,0,255)">this}"&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Button.Command</span><span style="color: rgb(0,0,255)">&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">local</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">SelectCommand</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="selectCommand"/&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Button.Command</span><span style="color: rgb(0,0,255)">&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Button</span><span style="color: rgb(0,0,255)">&gt;<br></span><span style="color: rgb(163,21,21)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TextBox</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="selectionTextBox"</span><span style="color: rgb(255,0,0)"> Text</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> ElementName</span><span style="color: rgb(0,0,255)">=selectCommand,</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=Selection}"/&gt;</span>
    

    
    Now that's much better. My button is set to execute a command when its activated (Clicked) and the command controls the page navigation and parameter passing for me. We get the result back into the page simply by binding our output field (in this case selectionTextBox) to the Selection property of the command (essentially the output parameter).
    

    
    But does it work... well no. It generates this little beauty:
    

    
    ![WPF Exception](/images/WPF%20Exception.png) 
    

    
    Hmm... so is this a bug - or have I somehow hooked into some interop? If I remove the event handler subscription to the Return event everything works fine - except of course I don't get my value ![](http://blog.spencen.com/emoticons/sad.png).
    

    
    I even tried Dispatching back to the UI thread in case it was a weird threading related issue - no luck. Interestingly when I first attempted to create my command by inheriting from RoutedCommand the button was disabled. This was despite the fact that my CanExecute method is hard-coded to return true. Its at this point I think I need to go back to the books - I'm obviously missing something.
    

    
    ___________________________________
    

    
    And the answer is given [here](http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=1975763&amp;SiteID=1).
    

    
    To work around the limitation I need to make the callback for the return an instance method of the calling form. Hmm... easy enough passing as a callback delegate in C# but how to declare that in XAML? Not too sure about the merits of this limitation - means putting code in the HomePage code behind again.<pre class="code"><font face="Verdana" size="2">        <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">void</span> Returned(<span style="color: rgb(0,0,255)">object</span> sender, <span style="color: rgb(43,145,175)">ReturnEventArgs</span>&lt;<span style="color: rgb(0,0,255)">string</span>&gt; e)
{
<span style="color: rgb(0,0,255)">if</span> (!<span style="color: rgb(0,0,255)">string</span>.IsNullOrEmpty(e.Result))
selectionTextBox.Text = e.Result;
}</font>
<a href="http://11011.net/software/vspaste"></a>


Oh - and a bit of reading into RoutedUICommand makes things clearer as to why inheriting from it got me nowhere ![](http://blog.spencen.com/emoticons/smile.png) Maybe it can help me with the problem above though?


