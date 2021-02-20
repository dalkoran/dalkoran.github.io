---
title: "WPF Validation Rules"
date: 2007-10-08 04:44
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---

Wow - just reading the section on Advanced DataBinding in the WPF Unleashed book. This talks about how to do validation during data binding - specifically by creating custom classes that inherit from ValidationRule and return a ValidationResult. Apart from being very déjà-vu - because at work we have our own validation rules engine that uses exactly the same class names it also sounds a little odd. Having a ValidationRules collection hanging right off the Binding itself - is that really the best place to define the rules?
 

My experience with our work validation engine is that certainly many of the rules - the simple ones - are actually derived directly from the binding. Therefore it would make sense to have them directly associated. Rules such as "is the field mandatory", "what is the fixed range", "what is the maximum text length", "what is the precision associated with a number". But what about inter-field rules such as "field A must be greater than field B", "field C is mandatory when field D is set to value x". Those are really entity based rules - not field based. Do we/could we put them at the DataBinding level?
 

I guess I'll have to keep reading to find out :-p
 

---
---
---
---
---
---
---
---

 

Ok - so now I've given this a quick test. To start with I've just created a simple class to bind to (Person) and hooked up a TextBox.


<font size="2"><font face="Verdana"><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TextBox</span><span style="color: rgb(255,0,0)"> SpellCheck.IsEnabled</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="True"&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TextBox.Text</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Source</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">StaticResource</span><span style="color: rgb(255,0,0)"> nos</span><span style="color: rgb(0,0,255)">}"</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">="FirstName"</span><span style="color: rgb(255,0,0)"> <br>                        </span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(255,0,0)">UpdateSourceTrigger</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">="PropertyChanged"&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Binding.ValidationRules</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">valid</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">MandatoryRule</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">/&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Binding.ValidationRules</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Binding.NotifyOnValidationError</span><span style="color: rgb(0,0,255)">&gt;</span><span style="color: rgb(163,21,21)">true</span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Binding.NotifyOnValidationError</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Binding</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">TextBox.Text</span></font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">TextBox</span><span style="color: rgb(0,0,255)">&gt;</span></font></font></pre>

    
    Ok - the spell checking was just for fun.
    

    
    Pretty longwinded isn't it. Setting the NotifyOnValidationError means that I can hook my [Status Panel](http://blog.spencen.com/2007/09/30/off-track.aspx) up to the events so that entries are added and removed. The ValidationErrorEventArgs has a host of properties that allows me to determine exactly which Binding and UI element. Also, because the error content is an object I could provide my own Error class - so instead of just returning a ValidationResult with a string (as per example below) I could generate a rich error instance (with primary/secondary text, priority, help URL etc.)
    

    
    My MandatoryRule is as simple as can be.
    <pre class="code"><font face="Verdana" size="2">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">MandatoryRule</span> : </font><font size="2"><font face="Verdana"><span style="color: rgb(43,145,175)">ValidationRule
</span>    {
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">override</span> <span style="color: rgb(43,145,175)">ValidationResult</span> Validate(<span style="color: rgb(0,0,255)">object</span> value, <span style="color: rgb(43,145,175)">CultureInfo</span> cultureInfo)
{
<span style="color: rgb(0,0,255)">if</span> (value == <span style="color: rgb(0,0,255)">null</span> || <span style="color: rgb(0,0,255)">object</span>.Equals(value, <span style="color: rgb(0,0,255)">string</span>.Empty))
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ValidationResult</span>(<span style="color: rgb(0,0,255)">false</span>, <span style="color: rgb(163,21,21)">"The field is mandatory."</span>);
</font></font><font size="2"><font face="Verdana"><span style="color: rgb(0,0,255)">else
</span>                <span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(43,145,175)">ValidationResult</span>.ValidResult;
}
}</font></font>



<font face="Verdana" size="2">Hopefully I will put all this together and post soon with a working example of the Status Panel, together with some ideas on other features that could be included to make it more functional - such as clicking on a message to focus the associated UI element.</font>


