---
layout: post
title: "Applying MetaData to WPF Bindings"
date: 2008-05-12 15:31
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


This post describes how I'm applying MetaData defined in my model to WPF controls at runtime. The goal is to keep the XAML concise without cluttering it with property settings that obscure the form layout intent and ensuring it stays in sync with the model across all forms in the application. Refer to [Karl Schifflet's recent passionate post](http://karlshifflett.wordpress.com/2008/05/08/metadata-a-voice-crying-in-the-wilderness-hey-im-over-here/) about why MetaData is important.
 

In WinForms I've traditionally used the data Binding to identify how MetaData should map to controls. So the following describes my attempt at the same thing in WPF.
 

I'm using attributes to define MetaData directly against my model classes. I'd like for it to be more open than that - for instance extracting the MetaData from a database, config file etc. - but attributes is a good place to start. Here's an example property from my Holiday model class.


<span style="font-size: 8pt; font-family: verdana">    [<span style="color: rgb(43,145,175)">Mandatory</span>]
[<span style="color: rgb(43,145,175)">Annotation</span>(<span style="color: rgb(163,21,21)">"Name"</span>, <span style="color: rgb(163,21,21)">"Name of this holiday"</span>)]
[<span style="color: rgb(43,145,175)">StringData</span>(MaximumLength=50, Case=<span style="color: rgb(43,145,175)">CharacterCasing</span>.Upper)]
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">string</span> Name
{
<span style="color: rgb(0,0,255)">get</span> { <span style="color: rgb(0,0,255)">return</span> _name; }
<span style="color: rgb(0,0,255)">set</span>
{
<span style="color: rgb(0,0,255)">if</span> (!<span style="color: rgb(43,145,175)">Object</span>.Equals(_name, <span style="color: rgb(0,0,255)">value</span>))
{
_name = <span style="color: rgb(0,0,255)">value</span>;
OnPropertyChanged(<span style="color: rgb(163,21,21)">"Name"</span>);
}
}
}</span></pre><a href="http://11011.net/software/vspaste"><a href="http://11011.net/software/vspaste"></a>

    
    So in this example I'm using three custom attributes. [Mandatory] is pretty self explanatory. Whilst its not used by the MetaData Applicator it is used by the ValidationEngine to create a MandatoryRule.
    

    
    The [Annotation] attribute derives from System.ComponentModel.DisplayName and extends it by including extended descriptions for tooltips and possible help keys etc.
    

    
    The third [StringData] attribute is the one that defines the MetaData specific to string data types. The attributes themselves are defined in a neutral namespace and assembly (Spencen.MetaData) with no references to any presentation assemblies.
    

    
    Taking a look at the StringDataAttribute class its just a bunch of properties with no inherent behaviour (like most attributes).
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">    [<span style="color: rgb(43,145,175)">MetaData</span>]
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">StringDataAttribute</span> : <span style="color: rgb(43,145,175)">StringLengthAttribute</span> <span style="color: rgb(0,128,0)">
</span>    {
<span style="color: rgb(0,0,255)">public</span> StringDataAttribute() : <span style="color: rgb(0,0,255)">base</span>()
{
}
<span style="color: rgb(0,0,255)">        #region</span> Public Properties
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">string</span> Format { <span style="color: rgb(0,0,255)">get</span>; <span style="color: rgb(0,0,255)">set</span>; }
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(43,145,175)">Type</span> DataType { <span style="color: rgb(0,0,255)">get</span>; <span style="color: rgb(0,0,255)">set</span>; }
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(43,145,175)">CharacterCasing</span> Case { <span style="color: rgb(0,0,255)">get</span>; <span style="color: rgb(0,0,255)">set</span>; }
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">bool</span> MultiLine { <span style="color: rgb(0,0,255)">get</span>; <span style="color: rgb(0,0,255)">set</span>; }
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">bool</span> AllowTab { <span style="color: rgb(0,0,255)">get</span>; <span style="color: rgb(0,0,255)">set</span>; }
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">bool</span> AllowNewline { <span style="color: rgb(0,0,255)">get</span>; <span style="color: rgb(0,0,255)">set</span>; }
<span style="color: rgb(0,0,255)">        #endregion
</span>    }</span></pre>

    
    <a href="http://11011.net/software/vspaste"></a>The StringDataAttribute class is itself decorated with a [MetaData] attribute. This tells the MetaData Applicator that it needs to be considered when applying MetaData properties to the UI. [The StringLengthAttribute that this class derives from is actually another ValidationRule attribute that results in the creation of the StringLengthValidationRule.]
    

    
    Now the tricky part in all of this was trying to "hook" into the WPF Binding pipeline. I tried all the likely approaches - inheriting from Binding, looking at BindingExpression and eventually creating a custom MarkupExtension that offloads most of the work to the standard Binding MarkupExtension.
    

    
    Once I got that far I looked around and found that the [MarkupExtension appears to be the current approach](http://www.hardcodet.net/2008/04/wpf-custom-binding-class). Mine is currently a very simplified implementation - as I later found out there are much better examples around including [this one](http://www.hardcodet.net/2008/04/wpf-custom-binding-class) or the one used by the [Enterprise Library Contrib's Standalone Validation Block](http://www.codeplex.com/entlibcontrib/Wiki/View.aspx?title=Standalone%20Validation%20Block).
    

    
    Having a customised Binding gives several advantages not the least of which is applying default values for the Binding itself. Ever got tired of adding ValidatesOnDataErrors=true, ValidatesOnExceptions=true, NotifyOnValidationError=true, UpdateSourceTrigger=UpdateSourceTrigger.PropertyChanged to every Binding!?
    

    
    Anyhow - more on that later - for now I use the following code within the ProvideValue() method of my custom MarkupExtension.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">    <span style="color: rgb(43,145,175)">IProvideValueTarget</span> valueTarget = (<span style="color: rgb(43,145,175)">IProvideValueTarget</span>)serviceProvider.GetService(<br>                                                                                                      <span style="color: rgb(0,0,255)">typeof</span>(<span style="color: rgb(43,145,175)">IProvideValueTarget</span>));
<span style="color: rgb(43,145,175)">DependencyObject</span> dependencyObject = valueTarget.TargetObject <span style="color: rgb(0,0,255)">as</span> <span style="color: rgb(43,145,175)">DependencyObject</span>;
<span style="color: rgb(43,145,175)">DependencyProperty</span> dependencyProperty = valueTarget.TargetProperty <span style="color: rgb(0,0,255)">as</span> <span style="color: rgb(43,145,175)">DependencyProperty</span>;
<span style="color: rgb(0,0,255)">if</span> (dependencyObject != <span style="color: rgb(0,0,255)">null</span> &amp;&amp; dependencyProperty != <span style="color: rgb(0,0,255)">null</span>)
{
<span style="color: rgb(0,0,255)">if</span> (dependencyObject <span style="color: rgb(0,0,255)">is</span> <span style="color: rgb(43,145,175)">FrameworkElement</span>)
{
<span style="color: rgb(0,0,255)">object</span> dataContext = ((<span style="color: rgb(43,145,175)">FrameworkElement</span>) dependencyObject).GetValue(<br>                                                                           <span style="color: rgb(43,145,175)">FrameworkElement</span>.DataContextProperty);
<span style="color: rgb(0,0,255)">if</span> (Applicator != <span style="color: rgb(0,0,255)">null</span>)
{
<span style="color: rgb(43,145,175)">PropertyDescriptor</span> property = <span style="color: rgb(43,145,175)">TypeDescriptor</span>.GetProperties(dataContext).Find(_path, <span style="color: rgb(0,0,255)">false</span>);
<span style="color: rgb(0,0,255)">if</span> (property != <span style="color: rgb(0,0,255)">null</span>)
{
<span style="color: rgb(0,0,255)">foreach</span> (<span style="color: rgb(43,145,175)">Attribute</span> propertyAttribute <span style="color: rgb(0,0,255)">in</span> property.Attributes)
{
<span style="color: rgb(0,128,0)">// Check if the attribute class is itself decorated with a MetaData attribute.
</span>                        <span style="color: rgb(0,0,255)">if</span> (<span style="color: rgb(43,145,175)">TypeDescriptor</span>.GetAttributes(propertyAttribute).Contains(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">MetaDataAttribute</span>()))
{
Applicator.ApplyTo(propertyAttribute, dependencyObject, dependencyProperty);
}
}
}
}
}
}</span></pre>

    
    <a href="http://11011.net/software/vspaste"></a>Essentially its just checking each Binding and then looking for [MetaData] decorated attributes on the data source. [Note that this code overly simplifies resolving the DataContext and Path. Remember the path is just that not necessarily just a property name, it can even have things like indexers etc. in it.]. Once its found an attribute it calls out to the statically assigned Applicator.
    

    
    The Applicator simply consists of a registered list of types and IMetaDataApplicators. Prior to any custom data Binding the application registered those [MetaData] attributes that it wishes to apply to the UI with the Applicator on the markup extension. Like so...
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">    <span style="color: rgb(43,145,175)">MetaDataExtension</span>.Applicator = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">MetaDataApplicator</span>();
<span style="color: rgb(43,145,175)">MetaDataExtension</span>.Applicator.RegisteredApplicators.Add(<br>                                                         <span style="color: rgb(0,0,255)">typeof</span>(<span style="color: rgb(43,145,175)">StringDataAttribute</span>), <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">StringDataApplicator</span>());
</span></pre>

    
    The StringDataApplicator class then does whatever it desires with the StringDataAttribute, target object and property that its been given. Here's a simple partially complete example:
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">StringDataApplicator</span> : <span style="color: rgb(43,145,175)">IMetaDataApplicator
</span>    {
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">void</span> ApplyTo(<span style="color: rgb(43,145,175)">Attribute</span> attribute, <span style="color: rgb(0,0,255)">object</span> targetObject, <span style="color: rgb(0,0,255)">object</span> targetProperty)
{
<span style="color: rgb(43,145,175)">StringDataAttribute</span> stringData = attribute <span style="color: rgb(0,0,255)">as</span> <span style="color: rgb(43,145,175)">StringDataAttribute</span>;
<span style="color: rgb(0,0,255)">if</span> (stringData == <span style="color: rgb(0,0,255)">null</span>) <br>                <span style="color: rgb(0,0,255)">throw</span> <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">InvalidOperationException</span>(<span style="color: rgb(163,21,21)">"StringDataApplication only supports StringDataAttribute."</span>);
<span style="color: rgb(43,145,175)">TextBoxBase</span> textBoxBase = targetObject <span style="color: rgb(0,0,255)">as</span> <span style="color: rgb(43,145,175)">TextBoxBase</span>;
<span style="color: rgb(0,0,255)">if</span> (textBoxBase != <span style="color: rgb(0,0,255)">null</span>)
{
textBoxBase.AcceptsTab = stringData.AllowTab;
textBoxBase.AcceptsReturn = stringData.AllowNewline;<br>            }
<span style="color: rgb(43,145,175)">TextBox</span> textBox = targetObject <span style="color: rgb(0,0,255)">as</span> <span style="color: rgb(43,145,175)">TextBox</span>;
<span style="color: rgb(0,0,255)">if</span> (textBox != <span style="color: rgb(0,0,255)">null</span>)
{
<span style="color: rgb(0,0,255)">switch</span>(stringData.Case)
{
<span style="color: rgb(0,0,255)">case</span> Spencen.MetaData.<span style="color: rgb(43,145,175)">CharacterCasing</span>.Lower:
textBox.CharacterCasing = System.Windows.Controls.<span style="color: rgb(43,145,175)">CharacterCasing</span>.Lower;
<span style="color: rgb(0,0,255)">break</span>;
<span style="color: rgb(0,0,255)">case</span> Spencen.MetaData.<span style="color: rgb(43,145,175)">CharacterCasing</span>.Upper:
textBox.CharacterCasing = System.Windows.Controls.<span style="color: rgb(43,145,175)">CharacterCasing</span>.Upper;
<span style="color: rgb(0,0,255)">break</span>;
<span style="color: rgb(0,0,255)">case</span> Spencen.MetaData.<span style="color: rgb(43,145,175)">CharacterCasing</span>.Camel:
<span style="color: rgb(0,128,0)">// TODO: Put hooks in to do formatting.
</span>                        <span style="color: rgb(0,0,255)">break</span>;
<span style="color: rgb(0,0,255)">case</span> Spencen.MetaData.<span style="color: rgb(43,145,175)">CharacterCasing</span>.Title:
<span style="color: rgb(0,128,0)">// TODO: Put hooks in to do formatting.
</span>                        <span style="color: rgb(0,0,255)">break</span>;
}
textBox.MaxLength = stringData.MaximumLength;
}
}
}
</span></pre>

    
    The XAML to bind the Holiday classes Name property using the MetaData MarkupExtension would be:
    <pre class="code"><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TextBox</span><span style="color: rgb(255,0,0)"> Grid.Column</span><span style="color: rgb(0,0,255)">="1"</span><span style="color: rgb(255,0,0)"> Grid.Row</span><span style="color: rgb(0,0,255)">="0"</span><span style="color: rgb(255,0,0)"> Width</span><span style="color: rgb(0,0,255)">="150"</span><span style="color: rgb(255,0,0)"> Text</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">meta</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">MetaData</span><span style="color: rgb(255,0,0)"> Name</span><span style="color: rgb(0,0,255)">}"/&gt;
</span></span>
<a href="http://11011.net/software/vspaste"></a>


The rendered TextBox would use the Binding properties defined as the default on the MetaData MarkupExtension (as opposed to the standard Binding class defaults). It will have its MaxLength set to 10 and entry forced to uppercase characters. Of course there are plenty of other properties on the StringDataAttribute that could have been used. For example: 



*   Defining a DataType that specifies a type that is used as an IValueConverter and/or formatter. This can be used to setup all the necessary plumbing for allowing data entry of specific string types - such as phone numbers, e-mail addresses etc.
*   Using MaximumLength to determine the optimum length for a TextBox. Again this helps with consistency - all 4 character code fields are automatically set to the standard width for a four character field.
*   Using the AnnotationAttribute to set the Tooltip.


<a href="http://blog.spencen.com/images/83489-72989/MetaData_2.png" target="_blank">![MetaData](http://blog.spencen.com/images/83489-72989/MetaData_thumb_1.png)</a>


