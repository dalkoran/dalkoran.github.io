---
layout: post
title: "IDataErrorInfo – is there really any point?"
date: 2008-11-24 13:48
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


When looking into data validation systems that focus around WinForms and WPF technologies the IDataErrorInfo interface is a common focus. Personally I’ve never been satisfied with this interface. Not only does it appear to have some very poorly named members, it just doesn’t seem anywhere near functional enough. Don’t get me wrong – its a great concept – it just seems to be a super-lightweight example.
  

From MetaData on **<font color="#0080c0">IDataErrorInfo</font>**:
  

<font face="Verdana" size="1">&#160;&#160; </font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">// Summary:            
</span>&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//&#160;&#160;&#160;&#160; Provides the functionality to offer custom error information that a user            
</span>&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//&#160;&#160;&#160;&#160; interface can bind to.            
</span>&#160;&#160;&#160; <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">interface</span> </font></font><font size="1"><font face="Verdana"><span style="color: rgb(43,145,175)">IDataErrorInfo            
</span>&#160;&#160;&#160; {           
&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">// Summary:            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//&#160;&#160;&#160;&#160; Gets an error message indicating what is wrong with this object.            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">// Returns:            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//&#160;&#160;&#160;&#160; An error message indicating what is wrong with this object. The default is            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//&#160;&#160;&#160;&#160; an empty string (&quot;&quot![](http://blog.spencen.com/emoticons/wink.png).            
</span>&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: rgb(0,0,255)">string</span> Error { <span style="color: rgb(0,0,255)">get</span>; }           
          
&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">// Summary:            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//&#160;&#160;&#160;&#160; Gets the error message for the property with the given name.            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">// Parameters:            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//&#160;&#160; columnName:            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//&#160;&#160;&#160;&#160; The name of the property whose error message to get.            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">// Returns:            
</span>&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,128,0)">//&#160;&#160;&#160;&#160; The error message for the property. The default is an empty string (&quot;&quot![](http://blog.spencen.com/emoticons/wink.png).            
</span>&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: rgb(0,0,255)">string</span> <span style="color: rgb(0,0,255)">this</span>[<span style="color: rgb(0,0,255)">string</span> columnName] { <span style="color: rgb(0,0,255)">get</span>; }</font></font>
  

<font face="Verdana" size="1">    }</font>

<a href="http://11011.net/software/vspaste"></a>


So if I have a business object, **<font color="#0080c0">Contact</font>**, that implements **<font color="#0080c0">IDataErrorInfo</font>** then to get a description of the validation state for a particular property we use <font size="1"><font face="Verdana"><font size="2">myContact[propertyName]</font>.</font></font> That really doesn’t seem very intuitive, surely that should be a method call, e.g. <font face="Verdana" size="2">myContact.GetPropertyValidationState(propertyName)</font>? Of course because all the return results are strings rather than object there’s no clean path for extensibility here either.



Does anyone really use this interface for anything other that quick data-binding sample applications? Really – I’d love to know.



[Paul’s latest foray into a WPF Validation framework](http://www.paulstovell.com/blog/validation-scopes-draft) got me started on this. Though to be fair his draft framework has clear extensibility points for plugging in a much richer business object validation interface.


