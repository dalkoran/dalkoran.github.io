---title: "WPF DataGrid Tips"
date: 2009-04-12 14:40
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---
I’ve been struggling at work to use Microsoft’s WPF DataGrid (from their WPF Toolkit) to fulfil a fairly basic set of requirement. The following is simply a list of lessons learned.
  

1.  Remember to set the SortMemberPath for all DataGridTemplateColumns. Otherwise the column header is “inactive” and doesn’t allow sorting regardless of setting CanUserSort. 
2.  Remember to set the ClipboardContentBinding for all DataGridTemplateColumns. Otherwise the column data will be copied to the clipboard as an empty string.        
        
<font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridTemplateColumn </span><span style="color: red">Header</span><span style="color: blue">=&quot;Date of Birth&quot;              
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </span><span style="color: red">SortMemberPath</span><span style="color: blue">=&quot;DateOfBirth&quot;              
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </span><span style="color: red">ClipboardContentBinding</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">DateOfBirth</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;&gt;             
&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridTemplateColumn.CellTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">DataTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">TextBlock </span><span style="color: red">Text</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">DateOfBirth</span><span style="color: blue">,</span><span style="color: red">Mode</span><span style="color: blue">=OneWay,</span><span style="color: red">StringFormat</span><span style="color: blue">=d}&quot; </span><span style="color: red">Margin</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;2,0,2,2&quot;/&gt;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;/</span><span style="color: #a31515">DataTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;   
&#160;&#160;&#160;&#160;&#160; &lt;/</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridTemplateColumn.CellTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;             
&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridTemplateColumn.CellEditingTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">DataTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DatePicker </span><span style="color: red">SelectedDate</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">DateOfBirth</span><span style="color: blue">,</span><span style="color: red">Mode</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=TwoWay}&quot;/&gt;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;/</span><span style="color: #a31515">DataTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;             
&#160;&#160;&#160;&#160;&#160; &lt;/</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridTemplateColumn.CellEditingTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;             
&#160; &lt;/</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridTemplateColumn</span><span style="color: blue">&gt;             
</span></font></font>
3.  As with other ItemsControls you will most likely want to redefine HighlightBrush and HighlightTextBrush to avoid the high contrast (and very old fashioned) row selection colours. 
4.  Use Styles embedded in the DataGrid’s style’s Resources collection to override attributes of controls that will be used for editing. For example, setting the BorderThickness=”0” and Padding=”0” on the DatePicker. 
5.  Watch out for implicit Styles that effect Button. The grid is made up of lots of buttons (grid, row and column headers) so having an implicit style define MinWidth or Margins can lead to some unsightly grid layouts. 
6.  DataGridCheckBoxColumn doesn’t centre vertically and its default margin doesn’t seem to match the DataGridTextColumn. Setting the Margin for DataGridCheckBoxColumns to&#160; “2”, or at least “0,2,0,0” seems to do the trick.
7.  Setting an EditingElementStyle for a DataGridTextBoxColumn to CharacterCasing=”Upper” is not honoured if the keystroke is used to enter edit mode. Requires a simple fix to the PrepareCellForEdit method in DataGridTextBoxColumn. Refer [http://www.codeplex.com/wpf/Thread/View.aspx?ThreadId=36985](http://www.codeplex.com/wpf/Thread/View.aspx?ThreadId=36985).         
        
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <font size="1" face="Verdana">&#160;&#160;&#160; </font><font size="1"><font face="Verdana"><span style="background: #f9fff9; color: green">// If text input started the edit, then replace the text with what was typed.              
</span>&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">string </span>inputText;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">switch </span>(textBox.CharacterCasing)             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; {             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">case </span><span style="color: #2b91af">CharacterCasing</span>.Upper:             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; inputText = textArgs.Text.ToUpper();             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">b
reak</span>;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">case </span><span style="color: #2b91af">CharacterCasing</span>.Lower:             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; inputText = textArgs.Text.ToLower();             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">break</span>;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">default</span>:             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; inputText = textArgs.Text;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">break</span>;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; }             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; textBox.Text = inputText;             
</font></font>
8.  Placing a DatePicker in a DataGridTemplateColumn doesn’t cause the DatePIcker to automatically gain focus when pressing F2 (enter edit). To get around that I overrode PrepareCellForEdit in DataGridTemplateColumn as follows:        
        
<font size="1"><font face="Verdana"><span style="color: blue">&#160;&#160;&#160; protected override object </span>PrepareCellForEdit(<span style="color: #2b91af">FrameworkElement </span>editingElement,             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: #2b91af">RoutedEventArgs </span>editingEventArgs)             
&#160;&#160;&#160; {             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; editingElement.MoveFocus(<span style="color: blue">new </span><span style="color: #2b91af">TraversalRequest</span>(<span style="color: #2b91af">FocusNavigationDirection</span>.First));             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">return base</span>.PrepareCellForEdit(editingElement, editingEventArgs);             
&#160;&#160;&#160; }</font></font> <a href="http://11011.net/software/vspaste"></a>        
<a href="http://11011.net/software/vspaste"></a>
9.  The DatePicker uses the Enter key to highlight the date within its embedded TextBox. This is annoying when used in a DataGridTemplateColumn since the standard behaviour for the Enter key is to commit edits and move down a row. Commenting out this functionality in the DatePicker (within ProcessDatePickerKey) does the trick.&#160;   
        
<font size="1" face="Verdana">&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">private bool </span>ProcessDatePickerKey(<span style="color: #2b91af">KeyEventArgs </span>e)          
&#160;&#160;&#160;&#160;&#160;&#160;&#160; {          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">switch </span>(e.Key)          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; {          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">case </span><span style="color: #2b91af">Key</span>.System:          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; {          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">switch </span>(e.SystemKey)          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; {          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">case </span><span style="color: #2b91af">Key</span>.Down: { <span style="color: blue">if </span>((<span style="color: #2b91af">Keyboard</span>.Modifiers &amp; <span style="color: #2b91af">ModifierKeys</span>.Alt) == <span style="color: #2b91af">ModifierKeys</span>.Alt)          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; {          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; TogglePopUp();          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">return true</span>;          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; }          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">break</span>;          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; }          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; }          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">break</span>;          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; }          
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font><font size="1"><font face="Verdana"><span style="background: #f9fff9; color: green">// Removing this functionality makes the control work better in the grid - ENTER goes to next row.             
</span>&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="background: #f9fff9; color: green">//case Key.Enter:             
</span>&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="background: #f9fff9; color: green">//{             
</span>&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="background: #f9fff9; color: green">//&#160;&#160;&#160; SetSelectedDate();             
</span>&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="background: #f9fff9; color: green">//&#160;&#160;&#160; return true;             
</span>&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font></font><font size="1"><font face="Verdana"><span style="background: #f9fff9; color: green">//}             
</span>&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; }            
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <span style="color: blue">return false</span>;            
&#160;&#160;&#160;&#160;&#160;&#160;&#160; }</font></font>  

### Othe
r Resources

  

*   Tips and Tricks for DataGrid and DatePicker: [http://wpf.codeplex.com/Wiki/View.aspx?title=Tips%20%26%20Tricks](http://wpf.codeplex.com/Wiki/View.aspx?title=Tips%20%26%20Tricks)
*   Customizing the DataGrid (Jaime Rodriguez): [http://blogs.msdn.com/jaimer/archive/2008/08/13/dabbling-around-the-new-wpf-datagrid-part-1.aspx](http://blogs.msdn.com/jaimer/archive/2008/08/13/dabbling-around-the-new-wpf-datagrid-part-1.aspx)
*   DataGrid (Vincent Sibal): [http://blogs.msdn.com/vinsibal/archive/2008/08/11/net-3-5-sp1-and-wpf-datagrid-ctp-is-out-now.aspx](http://blogs.msdn.com/vinsibal/archive/2008/08/11/net-3-5-sp1-and-wpf-datagrid-ctp-is-out-now.aspx) and [http://blogs.msdn.com/vinsibal/archive/2008/09/16/wpf-datagrid-styling-rows-and-columns-based-on-header-conditions-and-other-properties.aspx](http://blogs.msdn.com/vinsibal/archive/2008/09/16/wpf-datagrid-styling-rows-and-columns-based-on-header-conditions-and-other-properties.aspx)
*   Detecting column, cell and row for click event (Colin Eberhardt): [http://www.scottlogic.co.uk/blog/wpf/2008/12/wpf-datagrid-detecting-clicked-cell-and-row/](http://www.scottlogic.co.uk/blog/wpf/2008/12/wpf-datagrid-detecting-clicked-cell-and-row/)

