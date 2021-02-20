---title: "INotifyPropertyChanged via Extension Methods"
date: 2009-03-03 15:06
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---
I imagine most developers that work with data-binding in WinForms or WPF have their preferred way of implementing INotifyPropertyChanged (or individual &lt;property&gt;Changed events). Normally I use a base class to hide the interface declaration and event and then use helper methods in the setters to take care of raising the event when applicable.
  

I’ve also spent some time looking at AOP alternatives using PostSharp. These look really promising, but it does require taking a dependency on PostSharp. 
  

Another alternative is to use Extension methods. I had this realisation today and figured I’d spend a few minutes putting together a quick test. Getting the event proved a little irksome, and I’m passing around property names as strings (as opposed to perhaps [using a member expression](http://blog.hightech.ir/2008/09/enhanced-inotifypropertychanged.html)). Here’s the extension class:
  

>   

<span style="color: blue"><font size="1" face="Verdana">public static class </font></span><font size="1"><font face="Verdana"><span style="color: #2b91af">PropertyChangedExtension
</span>{
<span style="color: blue">public static void </span>OnPropertyChanged(<span style="color: blue">this </span><span style="color: #2b91af">INotifyPropertyChanged </span>sender, <span style="color: blue">string </span>propertyName)
{
RaiseEvent(sender, propertyName,  <span style="color: #a31515">&quot;PropertyChanged&quot;</span>);
}
<span style="color: blue">public static void </span>OnPropertyChanging(<span style="color: blue">this </span><span style="color: #2b91af">INotifyPropertyChanging </span>sender, <span style="color: blue">string </span>propertyName)
{
RaiseEvent(sender, propertyName, <span style="color: #a31515">&quot;PropertyChanging&quot;</span>);
}
<span style="color: blue">public static bool </span>SetValue&lt;T&gt;(<span style="color: blue">this </span><span style="color: #2b91af">INotifyPropertyChanged </span>sender,   
                                                  </font></font><font size="1"><font face="Verdana"><span style="color: blue">ref </span>T backingField,   
                                                  T newValue,   
                                                  <span style="color: blue">string </span>propertyName)
{
<span style="color: blue">if </span>( Equals( backingField, newValue ))
<span style="color: blue">return false</span>;
<span style="color: blue">var </span>propertyChanging = sender <span style="color: blue">as </span><span style="color: #2b91af">INotifyPropertyChanging</span>;
<span style="color: blue">if </span>(propertyChanging != <span style="color: blue">null</span>)
propertyChanging.OnPropertyChanging(propertyName);
backingField = newValue;
OnPropertyChanged(sender, propertyName);
<span style="color: blue">return true</span>;
}
<span style="color: blue">private static void </span>RaiseEvent(<span style="color: blue">object </span>sender, <span style="color: blue">string </span>propertyName, <span style="color: blue">string </span>eventName)
{
<span style="color: blue">var </span>fieldInfo = sender.GetType().GetField(eventName, <span style="color: #2b91af">BindingFlags</span>.Instance | <span style="color: #2b91af">BindingFlags</span>.NonPublic);
<span style="color: blue">if </span>(fieldInfo != <span style="color: blue">null</span>)
{
<span style="color: blue">var </span>eventDelegate = fieldInfo.GetValue(sender) <span style="color: blue">as </span><span style="color: #2b91af">MulticastDelegate</span>;
<span style="color: blue">if </span>(eventDelegate != <span style="color: blue">null</span>)
eventDelegate.DynamicInvoke(  
                       <span style="color: blue">new object</span>[] { sender, <span style="color: blue">new </span><span style="color: #2b91af">PropertyChangedEventArgs</span>(propertyName) });
}
}
}</font></font>




<a href="http://11011.net/software/vspaste"></a>


Here’s a simple test class. 



>


<font size="1" face="Verdana"><span style="color: blue">public class</span><span style="color: #2b91af">Person</span>: <span style="color: #2b91af">INotifyPropertyChanging</span>, </font><font size="1"><font face="Verdana"><span style="color: #2b91af">INotifyPropertyChanged
  
</span>{
  
&#160;&#160;&#160; <span style="color: blue">public event</span><span style="color: #2b91af">PropertyChangedEventHandler </span>PropertyChanged;
  
&#160;&#160;&#160; <span style="color: blue">public event</span><span style="color: #2b91af">PropertyChangingEventHandler </span>PropertyChanging;
  

  
&#160;&#160;&#160; <span style="color: blue">private string</span>_firstName;
  
&#160;&#160;&#160; <span style="color: blue">private string</span>_lastName;
  

  
&#160;&#160;&#160; <span style="color: blue">public string</span>FirstName
  
&#160;&#160;&#160; {
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">get</span>{ <span style="color: blue">return</span>_firstName; }
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: blue">set
  
&#160;&#160;&#160;&#160;&#160;&#160; </span>{
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">if</span>(<span style="color: blue">this</span>.SetValue(<span style="color: blue">ref </span>_firstName, <span style="color: blue">value</span>, <span style="color: #a31515">&quot;FirstName&quot;</span>))
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">this</span>.OnPropertyChanged(<span style="color: #a31515">&quot;FullName&quot;</span>);
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160; }
  
&#160;&#160;&#160; }
  

  
&#160;&#160;&#160; <span style="color: blue">public string </span>LastName
  
&#160;&#160;&#160; {
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">get </span>{ <span style="color: blue">return </span>_lastName; }
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: blue">set
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160; </span>{
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">if </span>(<span style="color: blue">this</span>.SetValue(<span style="color: blue">ref </span>_lastName, <span style="color: blue">value</span>, <span style="color: #a31515">&quot;FirstName&quot;</span>))
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">this</span>.OnPropertyChanged(<span style="color: #a31515">&quot;FullName&quot;</span>);
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160; }
  
&#160;&#160;&#160; }
  

  
&#160;&#160;&#160; <span style="color: blue">public string </span>FullName
  
&#160;&#160;&#160; {
  
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">get </span>{ <span style="color: blue">return string</span>.Format(<span style="color: #a31515">&quot;{0} {1}&quot;</span>, FirstName, LastName); }
  
&#160;&#160;&#160; }
  
}</font></font>




<a href="http://11011.net/software/vspaste"></a>


Note that I use the boolean result from SetValue to determine whether I should fire property changed events on other dependent properties (again this is something that can be done declaratively using PostSharp).



The RaiseEvent method in the Extension class is a really nasty hack (in other words it won’t always work and is slow). You can simply [pass through the event handlers](http://blog.jeffhandley.com/archive/2008/10/07/inotifypropertychanged---extension-methods.aspx) which makes things much simpler, but what I was aiming for was the least amount of code in the setters. I wouldn’t use this code in a production environment (a base class is much better suited). However it is well suited for adding property change notification to classes that I’m just throwing together for a demo or prototype without having to worry about PostSharp dependencies or base classes.


