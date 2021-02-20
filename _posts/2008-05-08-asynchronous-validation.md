---

title: "Asynchronous Validation"
date: 2008-05-08 14:59
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


Occasionally I've wanted to execute Validation Rules that take a significant duration to execute (anything more than half a second for example). Normally these involve some cross-tier communication, e.g. database access, web service call etc.&nbsp; Examples of these types of validation include:
 

1.  Validating that a field is unique - for example when entering a new Inventory Item which requires a unique code. This could also require a unique combination of field values, for example an item name that must be unique during its effective lifetime specified by a from/to date.  Validating stock levels for a selected product. 

It could be argued that these types of validation are best performed by the business layer on the application server after the user has committed the transaction, i.e. pressed the Save/Submit button. Of course the rules must be validated at that point anyway - since the business layer on your application server should never trust any data being sent to it. But that doesn't stop us using the same rules to provide timely warnings to the user prior them submitting a form full of data. 
 

The following code shows an sample ValidationRule designed to execute asynchronously.


<span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> </span><span style="color: rgb(128,128,128)">&lt;summary&gt;
///</span><span style="color: rgb(0,128,0)"> Sample validation rule that executes asynchronously by default.
</span><span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> </span><span style="color: rgb(128,128,128)">&lt;/summary&gt;
</span><span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">SampleAsyncRule</span> : <span style="color: rgb(43,145,175)">ValidationRule
</span>{
<span style="color: rgb(0,0,255)">private</span> <span style="color: rgb(0,0,255)">int</span> _milliSecondsToDelay;
<span style="color: rgb(0,0,255)">public</span> SampleAsyncRule(<span style="color: rgb(0,0,255)">int</span> milliSecondsToDelay)
{
_milliSecondsToDelay = milliSecondsToDelay;
IsAsync = <span style="color: rgb(0,0,255)">true</span>; <span style="color: rgb(0,128,0)">// Setting IsAsync to true ensures the Validate method is executed on a background thread.
</span>    }
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">override</span> <span style="color: rgb(43,145,175)">ValidationResult</span> Validate(<span style="color: rgb(0,0,255)">object</span> value, System.Globalization.<span style="color: rgb(43,145,175)">CultureInfo</span> cultureInfo)
{
<span style="color: rgb(0,128,0)">// This call could be replaced with a cal to the application service tier via a web service, remoting etc.
</span>        System.Threading.<span style="color: rgb(43,145,175)">Thread</span>.Sleep(_milliSecondsToDelay);
<span style="color: rgb(43,145,175)">Random</span> random = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Random</span>();
<span style="color: rgb(0,0,255)">if</span> (random.Next(100) &lt; 50)
{
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">ValidationResult</span>(<span style="color: rgb(43,145,175)">ContentSeverity</span>.Error, <span style="color: rgb(163,21,21)">"Async validation has determined an error."</span>);
}
<span style="color: rgb(0,0,255)">else
</span>        {
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(43,145,175)">ValidationResult.ValidResult;</span>
}
}
}</span></pre><a href="http://11011.net/software/vspaste"></a>

    
    When validating a registered rule the FormValidator checks the ValidationRule.IsAsync flag. If set to true it executes the Validate method on the rule using an asynchronous delegate call. The rule is interpreted as having returned a "pending" ValidationResult which will put the Validator in an indeterminate state (assuming it was previously in a Valid state). When the async delegate completes a callback method on the Validator is fired which extracts the real ValidationResult and removes the temporary "pending" result.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,0,255)">protected</span> <span style="color: rgb(0,0,255)">override</span> <span style="color: rgb(0,0,255)">void</span> ValidateInternal(<span style="color: rgb(0,0,255)">object</span> validationSource)
{
OnValidating(<span style="color: rgb(43,145,175)">EventArgs</span>.Empty);
<span style="color: rgb(0,0,255)">foreach</span> (Spencen.Validation.Rules.<span style="color: rgb(43,145,175)">ValidationRule</span> rule <span style="color: rgb(0,0,255)">in</span> RegisteredRules[validationSource])
{
Spencen.Validation.<span style="color: rgb(43,145,175)">ValidationResult</span> result;
<span style="color: rgb(0,0,255)">if</span> (rule.IsAsync)
{
<span style="color: rgb(43,145,175)">AsyncValidateCaller</span> caller = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">AsyncValidateCaller</span>(rule.BeginValidate);
<span style="color: rgb(43,145,175)">IAsyncResult</span> asyncResult = caller.BeginInvoke(validationSource, <br>                                                                                 </span><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(43,145,175)">CultureInfo</span>.CurrentCulture, <br>                                                                                 ValidateCallback, caller);
result = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">AsyncValidationResult</span>(asyncResult);
}
<span style="color: rgb(0,0,255)">else
</span>        {
result = rule.Validate(validationSource, <span style="color: rgb(43,145,175)">CultureInfo</span>.CurrentCulture);
}
ExtractErrors(result);
}
OnValidated(<span style="color: rgb(43,145,175)">EventArgs</span>.Empty);
}</span>


