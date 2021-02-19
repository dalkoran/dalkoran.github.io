---
layout: post
title: "Binding an Enum Property to a ComboBox using customized text"
date: 2009-04-18 15:35
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


### The problem

  

I want to data bind a property on my Model to a ComboBox that allows selection from a list of Enum values. For example, my **Person** class has a **HighestEducationLevel** property of type **EducationLevel**.
  

&#160;![GridEditing](/images/GridEditing_6.png "GridEditing") 
  

The second part of this problem is that I want to provide an optional customized text description for each of my enumeration values. For example, the enumeration value** EducationLevel.PreSchool** should be displayed as “Pre-school” in the ComboBox.
  

![GridEditing - TypeConverter ComboBox](/images/GridEditing%20-%20TypeConverter%20ComboBox_3.png "GridEditing - TypeConverter ComboBox") 
  

### Solution 1 – Bind to Enum.GetValues()

  

There are plenty of blog posts and forum answers that show the following technique that can be accomplished in XAML alone. I first saw this on a <a href="http://www.codeproject.com/KB/WPF/FillComboboxWSortedEnum.aspx" target="_blank">post by Karl Schifflet</a>.
  

You define a static resource as an ObjectDataProvider that simply uses a method call to get an array of the enumerated values.
  

<font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">ObjectDataProvider </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span><span style="color: blue">=&quot;EducationLevelList&quot; </span><span style="color: red">MethodName</span><span style="color: blue">=&quot;GetValues&quot; </span><span style="color: red">ObjectType</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">local</span><span style="color: blue">:</span><span style="color: red">EducationLevel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;&gt;
&lt;</span><span style="color: #a31515">ObjectDataProvider.MethodParameters</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">TypeName</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;local:EducationLevel&quot;/&gt;
&lt;/</span><span style="color: #a31515">ObjectDataProvider.MethodParameters</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ObjectDataProvider</span><span style="color: blue">&gt;</span></font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    
    

    
    This can then be data bound as the ItemsSource for a ComboBox – in my case its a ComboBox column of a WPF DataGrid.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridComboBoxColumn </span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Education Level&quot;
</span><span style="color: red">SelectedValueBinding</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">HighestEducationLevel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;
</span><span style="color: red">ItemsSource</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">Source</span><span style="color: blue">={</span><span style="color: #a31515">StaticResource </span><span style="color: red">EducationLevelList</span><span style="color: blue">}}&quot;/&gt;</span></font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    ### Solution 2 – Bind to Custom Method
    
    

    
    This technique can be taken a step further by providing your own method to be used by the ObjectDataProvider rather than relying on Enum.GetValues(Type). This then provides the opportunity to provide custom sorting, filtering and text for the list of enumeration values.
    

    
    One way to achieve the custom text values is to have the method return a list of “wrapper” objects that provide access to both the underlying enumeration value and the custom text.
    
<pre class="code"><span style="color: blue"><font size="1" face="Verdana">public class </font></span><font size="1"><font face="Verdana"><span style="color: #2b91af">EnumMapper
</span>{
<span style="color: blue">public </span>EnumMapper( <span style="color: blue">object </span>enumValue, <span style="color: blue">string </span>enumDescription )
{
Enum = enumValue;
Description = enumDescription;
}
<span style="color: blue">public object </span>Enum { <span style="color: blue">get</span>; <span style="color: blue">private set</span>; }
<span style="color: blue">public string </span>Description { <span style="color: blue">get</span>; <span style="color: blue">private set</span>; }
}</font></font></pre>

    
    The enum type can then have each of its members (fields) that require a custom text decorated with a custom attribute as follows.
    
<pre class="code"><span style="color: blue"><font size="1" face="Verdana">public enum </font></span><font size="1"><font face="Verdana"><span style="color: #2b91af">EducationLevel
</span>{
None,
[<span style="color: #2b91af">EnumDisplayName</span>(<span style="color: #a31515">&quot;Pre-school&quot;</span>)]
PreSchool,
[<span style="color: #2b91af">EnumDisplayName</span>( <span style="color: #a31515">&quot;Junior school&quot; </span>)]
JuniorSchool,
[<span style="color: #2b91af">EnumDisplayName</span>( <span style="color: #a31515">&quot;Senior School&quot; </span>)]
SeniorSchool,
Graduate,
[<span style="color: #2b91af">EnumDisplayName</span>( <span style="color: #a31515">&quot;Post Graduate&quot; </span>)]
PostGraduate,
Professor
}</font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    The new attribute class itself is trivial…
    
<pre class="code"><font size="1" face="Verdana">[<span style="color: #2b91af">AttributeUsage</span>(<span style="color: #2b91af">AttributeTargets</span>.Field, AllowMultiple=<span style="color: blue">false</span>)]
<span style="color: blue">public class </span><span style="color: #2b91af">EnumDisplayNameAttribute </span>: </font><font size="1"><font face="Verdana"><span style="color: #2b91af">Attribute
</span>{
<span style="color: blue">public </span>EnumDisplayNameAttribute( <span style="color: blue">string </span>displayName )
{
DisplayName = displayName;
}
<span style="color: blue">public string </span>DisplayName { <span style="color: blue">get</span>; <span style="color: blue">set</span>; }
}</font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    The custom method to generate the list then becomes...
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">public static </span><span style="color: #2b91af">IList</span>&lt;<span style="color: #2b91af">EnumMapper</span>&gt; GetEnumDescriptions( <span style="color: #2b91af">Type </span>enumType )
{
<span style="color: blue">if </span>( !enumType.IsEnum )
<span style="color: blue">throw new </span><span style="color: #2b91af">ArgumentException</span>( <span style="color: #a31515">&quot;This method can only be called for enum types.&quot; </span>);
<span style="color: blue">var </span>list = <span style="color: blue">new </span><span style="color: #2b91af">List</span>&lt;<span style="color: #2b91af">EnumMapper</span>&gt;();
<span style="color: blue">foreach </span>( <span style="color: blue">var </span>enumValue <span style="color: blue">in </span><span style="color: #2b91af">Enum</span>.GetValues( enumType ) )
list.Add( <span style="color: blue">new </span><span style="color: #2b91af">EnumMapper</span>( enumValue, enumType.GetDisplayName( enumValue ) ) );
<span style="color: blue">return </span>list;
}
<span style="color: blue">public static string </span>GetDisplayName( <span style="color: blue">this </span><span style="color: #2b91af">Type </span>enumType, <span style="color: blue">object </span>enumValue )
{
<span style="color: blue">if </span>( !enumType.IsEnum )
<span style="color: blue">throw new </span><span style="color: #2b91af">ArgumentException</span>( <span style="color: #a31515">&quot;This method can only be called for enum types.&quot; </span>);
<span style="color: blue">var </span>displayNameAttribute = enumType.GetField( enumValue.ToString() )  
                                                             .GetCustomAttributes( <span style="color: blue">typeof</span>( <span style="color: #2b91af">EnumDisplayNameAttribute </span>), <span style="color: blue">false </span>)  
                                                             .FirstOrDefault() <span style="color: blue">as </span><span style="color: #2b91af">EnumDisplayNameAttribute</span>;
<span style="color: blue">if </span>( displayNameAttribute != <span style="color: blue">null </span>)
<span style="color: blue">return </span>displayNameAttribute.DisplayName;
<span style="color: blue">return </span><span style="color: #2b91af">Enum</span>.GetName( enumType, enumValue );
}</font></font></pre>

    
    The XAML has to change a little. First the ObjectDataProvider must use the new method, and secondly because the ItemsSource is now a list of **EnumMapper** instances we must provide DisplayMemberPath and SelectedValuePath properties for the ComboBox.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">ObjectDataProvider </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span><span style="color: blue">=&quot;EducationLevelList&quot; </span><span style="color: red">MethodName</span><span style="color: blue">=&quot;GetEnumDescriptions&quot; </span><span style="color: red">ObjectType</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">local</span><span style="color: blue">:</span><span style="color: red">BindingSupport</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;&gt;
&lt;</span><span style="color: #a31515">ObjectDataProvider.MethodParameters</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">TypeName</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;local:EducationList&quot;/&gt;
&lt;/</span><span style="color: #a31515">ObjectDataProvider.MethodParameters</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ObjectDataProvider</span><span style="color: blue">&gt;</span></font></font></pre>
<a href="http://11011.net/software/vspaste"></a>
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridComboBoxColumn </span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Highest Education Level&quot;
</span><span style="color: blue">                                </span><span style="color: red">SelectedValueBinding</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">HighestEducationLevel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;
</span><span style="color: red">ItemsSource</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">Source</span><span style="color: blue">={</span><span style="color: #a31515">StaticResource </span><span style="color: red">EducationLevelList</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}}&quot;
</span><span style="color: red">DisplayMemberPath</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Description&quot;
</span><span style="color: red">SelectedValuePath</span><span style="color: blue">=&quot;Enum&quot;/&gt;</span></font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    ### Solution 3 – Use a customized TypeConverter
    
    

    
    Solution 2 provides quite a lot of flexibility but it does mean a couple of extra property setters are required in the XAML (though I guess these could go into a Style). The third approach is to use a TypeConverter to “magically” convert enum values to strings. The major benefit of this approach is that it will work not just for ComboBoxs but anywhere an enum is bound to text property, e.g. in a TextBlock or TextBox.
    

    
    First we declare a new TypeConverter that has some special processing that allows its to generate the custom text. All enums by default use the EnumConverter anyway – we are just providing an extra lookup to check for a custom attribute. 
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">public class </span><span style="color: #2b91af">EnumTypeConverter </span>: </font></font><font size="1"><font face="Verdana"><span style="color: #2b91af">EnumConverter
</span>{
<span style="color: blue">public </span>EnumTypeConverter( <span style="color: #2b91af">Type </span>enumType ) : <span style="color: blue">base</span>( enumType ) { }
<span style="color: blue">public override object </span>ConvertTo( <span style="color: #2b91af">ITypeDescriptorContext </span>context, <span style="color: #2b91af">CultureInfo </span>culture, <span style="color: blue">object </span>value, <span style="color: #2b91af">Type </span>destinationType )
{
<span style="color: blue">if </span>( destinationType == <span style="color: blue">typeof</span>(<span style="color: blue">string</span>) &amp;&amp; value != <span style="color: blue">null </span>)
{
<span style="color: blue">var </span>enumType = value.GetType();
<span style="color: blue">if </span>( enumType.IsEnum )
<span style="color: blue">return </span>GetDisplayName( value );
}
<span style="color: blue">return base</span>.ConvertTo( context, culture, value, destinationType );
}</font></font></pre>
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">    private string </span>GetDisplayName( <span style="color: blue">object </span>enumValue )
{
<span style="color: blue">var </span>displayNameAttribute = EnumType.GetField( enumValue.ToString() )  
                                                                 .GetCustomAttributes( <span style="color: blue">typeof</span>( <span style="color: #2b91af">EnumDisplayNameAttribute </span>), <span style="color: blue">false </span>)  
                                                                 .FirstOrDefault() <span style="color: blue">as </span><span style="color: #2b91af">EnumDisplayNameAttribute</span>;
<span style="color: blue">if </span>( displayNameAttribute != <span style="color: blue">null </span>)
<span style="color: blue">return </span>displayNameAttribute.DisplayName;
<span style="color: blue">return </span><span style="color: #2b91af">Enum</span>.GetName( EnumType, enumValue );
}
}</font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    The next step is to make sure that all our enums use the new TypeConverter. This is done by decorating the enum with a TypeConverter attribute.
    
<pre class="code"><font size="1"><font face="Verdana">[<span style="color: #2b91af">TypeConverter</span>(<span style="color: blue">typeof</span>(<span style="color: #2b91af">EnumTypeConverter</span>))]
<span style="color: blue">public enum </span></font></font><font size="1"><font face="Verdana"><span style="color: #2b91af">EducationLevel
</span>{
None,
[<span style="color: #2b91af">EnumDisplayName</span>(<span style="color: #a31515">&quot;Pre-school&quot;</span>)]
PreSchool,
[<span style="color: #2b91af">EnumDisplayName</span>( <span style="color: #a31515">&quot;Junior school&quot; </span>)]
JuniorSchool,
[<span style="color: #2b91af">EnumDisplayName</span>( <span style="color: #a31515">&quot;Senior School&quot; </span>)]
SeniorSchool,
Graduate,
[<span style="color: #2b91af">EnumDisplayName</span>( <span style="color: #a31515">&quot;Post Graduate&quot; </span>)]
PostGraduate,
Professor
}</font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    Now we can change our XAML back to its original simplified form (as per Solution 1) and yet we still get our customized text appearing.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">ObjectDataProvider </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span><span style="color: blue">=&quot;EducationLevelList&quot; </span><span style="color: red">MethodName</span><span style="color: blue">=&quot;GetValues&quot; </span><span style="color: red">ObjectType</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">local</span><span style="color: blue">:</span><span style="color: red">EducationLevel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;&gt;
&lt;</span><span style="color: #a31515">ObjectDataProvider.MethodParameters</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">TypeName</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;local:EducationLevel&quot;/&gt;
&lt;/</span><span style="color: #a31515">ObjectDataProvider.MethodParameters</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">ObjectDataProvider</span><span style="color: blue">&gt;</span></font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    
    

    
    <font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridComboBoxColumn </span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Education Level&quot;
  
    &#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </span><span style="color: red">SelectedValueBinding</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">HighestEducationLevel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;
  
    &#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </span><span style="color: red">ItemsSource</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">Source</span><span style="color: blue">={</span><span style="color: #a31515">StaticResource </span><span style="color: red">EducationLevelList</span><span style="color: blue">}}&quot;/&gt;</span></font></font>
    

    
    ### Other Considerations
    
    

    
    In the example here I’ve just used hard-coded strings for the **EnumDisplayName** attributes. However, there is no reason these couldn’t be resource IDs or the like and the GetDisplayName method changed accordingly.
    

    
    Also, if we want the TypeConverter to have the ability to ConvertFrom the customized text then we need to do a little more work. This is handy in the scenarios where the user may be able to type (as opposed to select from a list) the enum values. So the EnumTypeConverter changes to the following.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">public class </span><span style="color: #2b91af">EnumTypeConverter </span>: </font></font><font size="1"><font face="Verdana"><span style="color: #2b91af">EnumConverter
</span>{
<span style="color: blue">private </span><span style="color: #2b91af">IEnumerable</span>&lt;<span style="color: #2b91af">EnumMapper</span>&gt; _mappings;
<span style="color: blue">public </span>EnumTypeConverter( <span style="color: #2b91af">Type </span>enumType ) : <span style="color: blue">base</span>( enumType )
{
_mappings = <span style="color: blue">from object </span>enumValue <span style="color: blue">in </span><span style="color: #2b91af">Enum</span>.GetValues(enumType)
<span style="color: blue">select new </span><span style="color: #2b91af">EnumMapper</span>( enumValue, GetDisplayName(enumValue) );
}
<span style="color: blue">public override object </span>ConvertTo( <span style="color: #2b91af">ITypeDescriptorContext </span>context, <span style="color: #2b91af">CultureInfo </span>culture, <span style="color: blue">object </span>value, <span style="color: #2b91af">Type </span>destinationType )
{
<span style="color: blue">if </span>( destinationType == <span style="color: blue">typeof</span>(<span style="color: blue">string</span>) &amp;&amp; value != <span style="color: blue">null </span>)
{
<span style="color: blue">var </span>enumType = value.GetType();
<span style="color: blue">if </span>( enumType.IsEnum )
<span style="color: blue">return </span>GetDisplayName( value );
}
<span style="color: blue">return base</span>.ConvertTo( context, culture, value, destinationType );
}
<span style="color: blue">public override object </span>ConvertFrom( <span style="color: #2b91af">ITypeDescriptorContext </span>context, <span style="color: #2b91af">CultureInfo </span>culture, <span style="color: blue">object </span>value )
{
<span style="color: blue">if </span>( value <span style="color: blue">is string </span>)
{
<span style="color: blue">var </span>match = _mappings.FirstOrDefault( mapping =&gt; <span style="color: blue">string</span>.Compare( mapping.Description, (<span style="color: blue">string</span>) value, <span style="color: blue">true</span>, culture ) == 0 );
<span style="color: blue">if </span>( match != <span style="color: blue">null </span>)
<span style="color: blue">return </span>match.Enum;
}
<span style="color: blue">return base</span>.ConvertFrom( context, culture, value );
}  
    </font></font><font size="1"><font face="Verdana"><span style="color: blue">      
        private string </span>GetDisplayName( <span style="color: blue">object </span>enumValue )   
        {   
            <span style="color: blue">var </span>displayNameAttribute = EnumType.GetField( enumValue.ToString() )  
                                               .GetCustomAttributes( <span style="color: blue">typeof</span>( <span style="color: #2b91af">EnumDisplayNameAttribute </span>), <span style="color: blue">false </span>)  
                                               .FirstOrDefault() <span style="color: blue">as </span><span style="color: #2b91af">EnumDisplayNameAttribute</span>;   
      
            <span style="color: blue">if </span>( displayNameAttribute != <span style="color: blue">null </span>)   
                <span style="color: blue">return </span>displayNameAttribute.DisplayName;   
              
            <span style="color: blue">return </span><span style="color: #2b91af">Enum</span>.GetName( EnumType, enumValue );   
        }
}</font></font>

<a href="http://11011.net/software/vspaste"></a>

