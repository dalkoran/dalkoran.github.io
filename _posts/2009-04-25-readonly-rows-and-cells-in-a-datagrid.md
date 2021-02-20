---
title: "ReadOnly Rows and Cells in a DataGrid"
date: 2009-04-25 13:32
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---

A common requirement for a DataGrid control is to have cells, or entire rows and/or columns that are read-only, or in other words non-editable. My requirements for this are as follows:
  

1.  ReadOnly can be applied to the entire grid, a column, a row, or an individual cell. 
2.  The cell(s) must not allow the cell value to be modified. 
3.  The cell(s) must be highlighted in some manner (e.g. background colour) to indicate that they are different to the editable cells. 
4.  ReadOnly columns require only single direction data-binding (i.e. Mode=OneWay to read-only properties).   

![GridEditing - ReadOnly Rows](/images/GridEditing%20-%20ReadOnly%20Rows_5.png "GridEditing - ReadOnly Rows") 
  

The Microsoft WPF DataGrid meets these requirements via:
  

*   DataGrid.IsReadOnly property  

>   

<font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGrid </span><span style="color: red">IsReadOnly</span><span style="color: blue">=&quot;True&quot;</span></font></font></pre>

    
    
<a href="http://11011.net/software/vspaste"></a>


*   DataColumn.IsReadOnly property

    
    >
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridTextColumn </span><span style="color: red">Binding</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">FullName</span><span style="color: blue">,</span><span style="color: red">Mode</span><span style="color: blue">=OneWay}&quot; </span><span style="color: red">IsReadOnly</span><span style="color: blue">=&quot;True&quot;</span><span style="color: blue">/&gt;</span></font></font></pre>

    
    
<a href="http://11011.net/software/vspaste"></a>

    
    We can use a simple style targeting all DataGridCells to change the background colour. 
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">Style </span><span style="color: red">TargetType</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">toolkit</span><span style="color: blue">:</span><span style="color: red">DataGridCell</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;&gt;
</span><span style="color: green">    </span><span style="color: blue">&lt;</span><span style="color: #a31515">Style.Triggers</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
</span><span style="color: blue">        &lt;</span><span style="color: #a31515">Trigger </span><span style="color: red">Property</span><span style="color: blue">=&quot;IsReadOnly&quot; </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;True&quot;&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;Background&quot; </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;LightGray&quot;/&gt;
&lt;/</span><span style="color: #a31515">Trigger</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">Style.Triggers</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;/</span><span style="color: #a31515">Style</span><span style="color: blue">&gt;</span></font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    So the only thing that’s really missing here is the ability to mark an entire row as read-only. In my experience this is a common requirement – we have a list of records displayed in the grid some of which are locked/completed/secured, whilst others can be edited.
    

    
    One solution is to override the OnBeginningEdit method of the DataGrid. The following example assumes that I have an attached property ControlSupport.IsReadOnly.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">protected override void </span>OnBeginningEdit( <span style="color: #2b91af">DataGridBeginningEditEventArgs </span>e )
{
<span style="color: blue">base</span>.OnBeginningEdit( e );
<span style="color: blue">bool </span>isReadOnlyRow = <font color="#008080">ControlSupport</font>.GetIsReadOnly( e.Row );
<span style="color: blue">if </span>( isReadOnlyRow )
e.Cancel = <span style="color: blue">true</span>;
}</font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    However, since I’ve got used to “tweaking” some of the DataGrid code I decided to instead to simply add an IsReadOnly property to the DataGridRow class.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">public bool </span>IsReadOnly
{
<span style="color: blue">get </span>{ <span style="color: blue">return </span>(<span style="color: blue">bool</span>) GetValue( IsReadOnlyProperty ); }
<span style="color: blue">set </span>{ SetValue( IsReadOnlyProperty, <span style="color: blue">value </span>); }
}
</font></font><font size="1"><font face="Verdana"><span style="background: #f9fff9; color: green">// Using a DependencyProperty as the backing store for IsReadOnly.  This enables animation, styling, binding, etc...
</span><span style="color: blue">public static readonly </span><span style="color: #2b91af">DependencyProperty </span>IsReadOnlyProperty =
<span style="color: #2b91af">DependencyProperty</span>.Register( <span style="color: #a31515">&quot;IsReadOnly&quot;</span>, <span style="color: blue">typeof</span>( <span style="color: blue">bool </span>), <span style="color: blue">typeof</span>( <span style="color: #2b91af">DataGridRow </span>),   
                <span style="color: blue">new </span><span style="color: #2b91af">FrameworkPropertyMetadata</span>( <span style="color: blue">false</span>, OnNotifyRowAndCellsPropertyChanged ) );</font></font></pre>
<a href="http://11011.net/software/vspaste"></a>
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">private static void </span>OnNotifyRowAndCellsPropertyChanged( <span style="color: #2b91af">DependencyObject </span>d,   
                                                                                     <span style="color: #2b91af">DependencyPropertyChangedEventArgs </span>e )
{
( d <span style="color: blue">as </span><span style="color: #2b91af">DataGridRow </span>).NotifyPropertyChanged( d, e, <span style="color: #2b91af">NotificationTarget</span>.Rows | <span style="color: #2b91af">NotificationTarget</span>.Cells );
}</font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    Then I just needed to make sure that the read-only property DataGridCell.IsReadOnly would correctly return the right value when its row was marked as read-only.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">private static object </span>OnCoerceIsReadOnly(<span style="color: #2b91af">DependencyObject </span>d, <span style="color: blue">object </span>baseValue)
{
<span style="color: blue">var </span>cell = d <span style="color: blue">as </span><span style="color: #2b91af">DataGridCell</span>;
<span style="color: blue">var </span>column = cell.Column;
<span style="color: blue">var </span>row = cell.RowOwner;
<span style="color: blue">var </span>dataGrid = cell.DataGridOwner;
<span style="color: blue">return </span><span style="color: #2b91af">DataGridHelper</span>.GetCoercedTransferPropertyValue(
cell,
baseValue,
IsReadOnlyProperty,
row,
<span style="color: #2b91af">DataGridRow</span>.IsReadOnlyProperty,
column,
<span style="color: #2b91af">DataGridColumn</span>.IsReadOnlyProperty,
dataGrid,
<span style="color: #2b91af">DataGrid</span>.IsReadOnlyProperty);
}</font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    There’s a fairly complex series of notification propagation calls going on within the DataGrid classes. For this solution to work it requires that the DataGridRow.IsReadOnly property changing flows down and causes the read-only DataGridCell.IsReadOnly property to be re-evaulated (via the coerce method above). I had to add another GetCoervedTransferPropertyValue method that took another pair of object/property parameters, and also tweaked the DataGridCell.NotifyPropertyChanged as follows:
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">else if </span>(e.Property == <span style="color: #2b91af">DataGrid</span>.IsReadOnlyProperty ||   
             e.Property == <span style="color: #2b91af">DataGridColumn</span>.IsReadOnlyProperty ||   
             e.Property == <span style="color: #2b91af">DataGridRow</span>.IsReadOnlyProperty ||   
             e.Property == IsReadOnlyProperty)
{
<span style="color: #2b91af">DataGridHelper</span>.TransferProperty(<span style="color: blue">this</span>, IsReadOnlyProperty);
}</font></font></pre>
<a href="http://11011.net/software/vspaste"></a>

    
    Now in my XAML I can do the following (assuming I have an IsReadOnly property on my business objects):
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">Style </span><span style="color: red">TargetType</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">toolkit</span><span style="color: blue">:</span><span style="color: red">DataGridRow</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;IsReadOnly&quot; </span><span style="color: red">Value</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">IsReadOnly</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;/&gt;
</span><span style="color: blue">&lt;/</span><span style="color: #a31515">Style</span><span style="color: blue">&gt;</span></font></font>

<a href="http://11011.net/software/vspaste"></a>

