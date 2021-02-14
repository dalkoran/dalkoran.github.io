---
layout: post
title: "Validation Interfaces"
date: 2008-05-07 15:37
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


#### IValidationRuleProvider

 

Rules can be provided to a Validator in two ways. The first is to simply register an individual ValidationRule with the Validator using the RegisterRule() method. 


<span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(43,145,175)">SampleAsyncRule</span> asyncRule = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">SampleAsyncRule</span>(3000);
validator.RegisterRule(<span style="color: rgb(0,0,255)">this</span>, asyncRule);</span></pre><a href="http://11011.net/software/vspaste"><a href="http://11011.net/software/vspaste"></a>

    
    ![IValidationRuleProvider](http://blog.spencen.com/images/83489-72989/IValidationRuleProvider_6.png) The second is to nominate an IValidationRuleProvider by adding it to the RuleProviders collection property. A RuleProvider is responsible for providing zero or more ValidationRules to the Validator when it is asked to validate an object instance for the first time. An example implementation is the EntityRuleProvider class which creates validation rules&nbsp; by using TypeDescriptor methods to iterate over all Property and Type level Attributes searching for those that implement the IAttributeRuleProvider interface (see below). 
    <pre class="code"><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,128,0)">// Create an EntityValidator for each business model class that requires validation.
</span><span style="color: rgb(43,145,175)">EntityValidator</span> holidayValidator = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">EntityValidator</span>();
<span style="color: rgb(0,128,0)">// Register the entity with the validator - this will create all the necessary rules as
</span><span style="color: rgb(0,128,0)">// determined by the rule Provider.
</span>holidayValidator.RegisterEntity(_holiday, <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">EntityRuleProvider</span>());</span></pre><a href="http://11011.net/software/vspaste"></a>

    
    Alternatively a different IValidationRuleProvider could be used so that rather than defining validation rules declaratively using .NET Attributes they could be extracted from some external source, for example an XML config file.
    

    
    #### IValidationValueProvider
    
    

    
    <a href="http://blog.spencen.com/images/83489-72989/IValidationValueProvider_2.png">![IValidationValueProvider](http://blog.spencen.com/images/83489-72989/IValidationValueProvider_thumb.png)</a> This interface is used by the ValidationRules themselves. When a rule is Validated by a Validator it is passed a single value. This value will be the instance that the Validator has been asked to Validate. For example an EntityValidator may use INotifyPropertyChanged to validate a business entity on every property change - and the business entity instance would be passed to the ValidationRule. 
    

    
    Implementations of the IValidationValueProvider allow the value that is to be validated to be derived immediately prior to the rule's execution. If no IValidationValueProvider is assigned to the ValidationRule it will simply use the value passed by the Validator (e.g. the business entity). A very simple IValidationValueProvider is the PropertyValueProvider which simply uses a PropertyDescriptor to resolve the property value from the instance being validated for rules that act on a Property rather than a Type.
    

    
    This could be extended for example by having a ControlValueProvider that extracts the value from an unbound User Interface control based on the type of control. This would allow validation at the control level in the rare cases where validation wasn't being performed on Controller/Presenter elements.
    

    
    In addition to providing a value this interface is also responsible for identifying the *context* in which any errors occurred. More on this below.
    

    
    #### IAttributeRuleProvider
    
    

    
    Each custom Attribute class is responsible for creating its own ValidationRule(s) via this interface. This makes declaratively defining new custom ValidationRules very easy.
    

    
    ![IAttributeRuleProvider](http://blog.spencen.com/images/83489-72989/IAttributeRuleProvider_6.png) 
    

    
    The code below shows how the EntityRuleProvider implements IValidationRuleProvider.GetRules by making use of the IAttributeRuleProvider.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(43,145,175)">ICollection</span>&lt;<span style="color: rgb(43,145,175)">ValidationRule</span>&gt; GetRules(<span style="color: rgb(0,0,255)">object</span> value)
{
<span style="color: rgb(43,145,175)">List</span>&lt;<span style="color: rgb(43,145,175)">ValidationRule</span>&gt; rules = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">List</span>&lt;<span style="color: rgb(43,145,175)">ValidationRule</span>&gt;();
<span style="color: rgb(0,128,0)">// Get a list of all properties defined on this object
</span>    <span style="color: rgb(43,145,175)">PropertyDescriptorCollection</span> propertyDescriptors = <span style="color: rgb(43,145,175)">TypeDescriptor</span>.GetProperties(value);
<span style="color: rgb(0,128,0)">// Iterate through the properties and pull of all the attribute defined validation rules
</span>    <span style="color: rgb(0,0,255)">foreach</span> (<span style="color: rgb(43,145,175)">PropertyDescriptor</span> propertyDescriptor <span style="color: rgb(0,0,255)">in</span> propertyDescriptors)
{
<span style="color: rgb(0,0,255)">foreach</span> (<span style="color: rgb(43,145,175)">Attribute</span> attribute <span style="color: rgb(0,0,255)">in</span> propertyDescriptor.Attributes)
{
<span style="color: rgb(43,145,175)">IAttributeRuleProvider</span> attributeRuleProvider = attribute <span style="color: rgb(0,0,255)">as</span> <span style="color: rgb(43,145,175)">IAttributeRuleProvider</span>;
<span style="color: rgb(0,0,255)">if</span> (attributeRuleProvider != <span style="color: rgb(0,0,255)">null</span>)
{
<span style="color: rgb(43,145,175)">ValidationRuleCollection</span> newRules = attributeRuleProvider.GetRules(value, propertyDescriptor);
<span style="color: rgb(0,0,255)">foreach</span> (<span style="color: rgb(43,145,175)">ValidationRule</span> newRule <span style="color: rgb(0,0,255)">in</span> newRules)
{
newRule.ValueProvider = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">PropertyValueProvider</span>(propertyDescriptor);
newRule.DefaultMessageParameters.Add(<span style="color: rgb(163,21,21)">"value"</span>, propertyDescriptor.DisplayName);
rules.Add(newRule);
}
}
}
}
<span style="color: rgb(0,128,0)">// Now pull the attribute defined validation rules off the class itself.
</span>    <span style="color: rgb(0,0,255)">foreach</span> (<span style="color: rgb(43,145,175)">Attribute</span> attribute <span style="color: rgb(0,0,255)">in</span> <span style="color: rgb(43,145,175)">TypeDescriptor</span>.GetAttributes(value))
{
<span style="color: rgb(43,145,175)">IAttributeRuleProvider</span> attributeRuleProvider = attribute <span style="color: rgb(0,0,255)">as</span> <span style="color: rgb(43,145,175)">IAttributeRuleProvider</span>;
<span style="color: rgb(0,0,255)">if</span> (attributeRuleProvider != <span style="color: rgb(0,0,255)">null</span>)
{
<span style="color: rgb(43,145,175)">ValidationRuleCollection</span> newRules = attributeRuleProvider.GetRules(value, <span style="color: rgb(0,0,255)">null</span>);
<span style="color: rgb(0,0,255)">foreach</span> (<span style="color: rgb(43,145,175)">ValidationRule</span> newRule <span style="color: rgb(0,0,255)">in</span> newRules)
{
newRule.ValueProvider = <span style="color: rgb(0,0,255)">null</span>;
newRule.DefaultMessageParameters.Add(<span style="color: rgb(163,21,21)">"value"</span>, value.GetType().Name);
rules.Add(newRule);
}
}
}
<span style="color: rgb(0,0,255)">return</span> rules;
}</span>
<a href="http://11011.net/software/vspaste"></a>










Note that by using PropertyDescriptor.DisplayName we get the "friendly" name of the property as can be provided via the System.ComponentModel.DisplayName attribute. This is important because its gets passed in as a "token" for optional use by the ValidationRule's message, e.g. "The {value} is mandatory." would automatically resolve {value} to the DisplayName.



#### ValidationContext




This abstract class is used to determine the context in which a ValidationRule failed. There are three concrete Context's so far as shown below.



![ValidationContext](http://blog.spencen.com/images/83489-72989/ValidationContext_3.png) 



Again, if User Interface Control level validation was required it would be easy to add a ControlContext class deriving from ValidationContext. One of the things that the ValidationContext is used for is displaying visual cues in the user interface to&nbsp; indicate validation failure. For example a rule could be executed in the business tier (on an application server) and transmitted back to the client. The user interface can then use data binding (or some other method) to match the context to a relevant user interface element.


