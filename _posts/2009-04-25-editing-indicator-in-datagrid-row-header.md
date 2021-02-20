---

title: "Editing Indicator in DataGrid Row Header"
date: 2009-04-25 15:55
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


Editable grids will quite commonly show an indicator in the row header area to indicate that a row is currently being edited. This is trivial to achieve using Microsoft’s WPF DataGrid.
  

&#160;![GridEditing - Edit Indicator](/images/GridEditing%20-%20Edit%20Indicator_6.png "GridEditing - Edit Indicator") 
  

All that is needed is a DataTemplate assigned to the RowHeaderTemplate property of the DataGrid. The template simply shows or hides the editing image based upon whether the current row is being edited.
  

<font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">SolidColorBrush </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span><span style="color: blue">=&quot;gridLineBrush&quot; </span><span style="color: red">Color</span><span style="color: blue">=&quot;#FFCDEFFE&quot;/&gt;</span></font></font></pre>
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">DataTemplate </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;rowHeaderTemplate&quot;&gt;
&lt;</span><span style="color: #a31515">StackPanel </span><span style="color: red">Orientation</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Horizontal&quot;&gt;
&lt;</span><span style="color: #a31515">Image </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Name</span><span style="color: blue">=&quot;editImage&quot; </span><span style="color: red">Source</span><span style="color: blue">=&quot;Images/Edit.png&quot; </span><span style="color: red">Width</span><span style="color: blue">=&quot;16&quot; </span><span style="color: red">Margin</span><span style="color: blue">=&quot;1,0&quot; </span><span style="color: red">Visibility</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Hidden&quot;/&gt;
&lt;/</span><span style="color: #a31515">StackPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">DataTemplate.Triggers</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">DataTrigger </span><span style="color: red">Binding</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">RelativeSource</span><span style="color: blue">={</span><span style="color: #a31515">RelativeSource </span><span style="color: red">FindAncestor</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">,  
                                           </span><span style="color: red">AncestorType</span><span style="color: blue">={</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">toolkit</span><span style="color: blue">:</span><span style="color: red">DataGridRow</span><span style="color: blue">}},</span><span style="color: red">Path</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=Item.IsEditing}&quot;   
                         </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;True&quot;&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">TargetName</span><span style="color: blue">=&quot;editImage&quot; </span><span style="color: red">Property</span><span style="color: blue">=&quot;Visibility&quot; </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Visible&quot;/&gt;
&lt;/</span><span style="color: #a31515">DataTrigger</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">DataTemplate.Triggers</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">DataTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">Style </span><span style="color: red">TargetType</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">toolkit</span><span style="color: blue">:</span><span style="color: red">DataGrid</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;GridLinesVisibility&quot; </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;All&quot;/&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;HorizontalGridLinesBrush&quot; </span><span style="color: red">Value</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">gridLineBrush</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;/&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;VerticalGridLinesBrush&quot; </span><span style="color: red">Value</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">gridLineBrush</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;/&gt;
<font color="#808080">    &lt;</font></span><font color="#808080"><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;RowHeaderTemplate&quot; </span><span style="color: red">Value</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">rowHeaderTemplate</span></font></font></font><font size="1"><font face="Verdana"><font color="#808080"><span style="color: blue">}&quot;/&gt;
<font color="#808080">    &lt;</font></span></font><font color="#808080"><span style="color: #a31515">Style.Resources</span></font></font></font><font color="#808080"><font size="1"><font face="Verdana"><span style="color: blue">&gt;
</span><span style="color: green">        </span><span style="color: blue">&lt;</span><span style="color: #a31515">SolidColorBrush </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Static </span><span style="color: red">SystemColors</span><span style="color: blue">.</span><span style="color: red">HighlightBrushKey</span><span style="color: blue">}&quot; </span><span style="color: red">Color</span></font></font></font><font color="#808080"><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Transparent&quot;/&gt;
&lt;</span><span style="color: #a31515">SolidColorBrush </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Static </span><span style="color: red">SystemColors</span><span style="color: blue">.</span><span style="color: red">HighlightTextBrushKey</span><span style="color: blue">}&quot; </span><span style="color: red">Color</span></font></font></font><font color="#808080"><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Black&quot;/&gt;
&lt;</span><span style="color: #a31515">SolidColorBrush </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Static </span><span style="color: red">toolkit</span><span style="color: blue">:</span><span style="color: red">DataGrid</span><span style="color: blue">.FocusBorderBrushKey}&quot; </span><span style="color: red">Color</span></font></font></font><font color="#808080"><font size="1"><font face="Verdana"><span style="color: blue">=&quot;{<font color="#800000">StaticResource</font> <font color="#ff0000">gridLineBrush</font>}&quot;/&gt;
&lt;/</span><span style="color: #a31515">Style.Resources</span></font></font></font><font size="1"><font face="Verdana"><span style="color: blue"><font color="#808080">&gt;</font>
&lt;/</span><span style="color: #a31515">Style</span><span style="color: blue">&gt;</span></font></font></pre>

    
    There is an IsEditingRowItem property on the DataGrid but unfortunately its private. As it happens my base model class has an IsEditing property as part of its IEditableObject implementation. So I simply bind the visibility of the image to that.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">#region </span>IEditableObject Members
<span style="color: blue">public void </span>BeginEdit()
{
IsEditing = <span style="color: blue">true</span>;
}
<span style="color: blue">public void </span>CancelEdit()
{
IsEditing = <span style="color: blue">false</span>;
</font></font><font size="1"><font face="Verdana"><font color="#008000">    // RollbackPropertyValues( _preEditValues );
</font>}
<span style="color: blue">public void </span>EndEdit()
{
IsEditing = <span style="color: blue">false</span>;
}
<span style="color: blue">#endregion</span></font></font>

<a href="http://11011.net/software/vspaste"></a>

