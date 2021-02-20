---
title: "When is a WPF DataGrid read-only CheckBox not read-only?"
date: 2009-04-19 13:50
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---

I found the answer to this riddle when I decided to style the **DataGridCheckBoxColumn** of Microsoft’s WPF DataGrid. By default the CheckBox displayed by the column template is not centered horizontally or vertically. I thought this looked at little tacky so I decided to apply a custom Style to the **ElementStyle** (and **EditElementStyle**) property.
  

<font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">Style  </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span><span style="color: blue">=&quot;CheckBoxStyle&quot; </span><span style="color: red">TargetType</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">CheckBox</span><span style="color: blue">}&quot; </span><span style="color: red">BasedOn</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: blue">{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">CheckBox</span></font></font><span style="color: blue"><font size="1" face="Verdana">}}&quot;&gt;
</font></span><font size="1"><font face="Verdana"><span style="color: blue">    &lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;HorizontalAlignment&quot; </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Center&quot;/&gt;   
    </span></font></font><font size="1"><font face="Verdana"><span style="color: blue">    &lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;Margin&quot; </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;0,2,0,0&quot;/&gt;
&lt;/</span><span style="color: #a31515">Style</span><span style="color: blue">&gt;</span></font></font></pre>
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridCheckBoxColumn </span><span style="color: red">Binding</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">IsPensioner</span><span style="color: blue">}&quot; </span><span style="color: red">  
                                                      Header</span><span style="color: blue">=&quot;Pensioner?&quot;   
                                                      </span><span style="color: red">ElementStyle</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">CheckBoxStyle</span><span style="color: blue">}&quot;   
                                                      </span><span style="color: red">EditingElementStyle</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">CheckBoxStyle</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;/&gt;
&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridCheckBoxColumn </span><span style="color: red">Binding</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">IsEditing</span><span style="color: blue">,</span><span style="color: red">Mode</span><span style="color: blue">=OneWay}&quot;   
                                                      <font size="1"><font face="Verdana"><span style="color: red">Header</span></font></font><span style="color: blue"><font size="1" face="Verdana">=&quot;Is Editing&quot;</font></span>  
                                                      </span><span style="color: red">ElementStyle</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">CheckBoxStyle</span><span style="color: blue">}&quot;   
      
                                                      </span><span style="color: red">IsReadOnly</span><span style="color: blue">=&quot;True&quot;</span></font></font><span style="color: blue"><font size="1" face="Verdana">/&gt;</font></span></pre>
<a href="http://11011.net/software/vspaste"></a><a href="http://11011.net/software/vspaste"></a>

    
    What I later discovered is that applying this style has somehow made my read-only CheckBox editable. But only via mouse clicks! Using the keyboard to focus to the cell and pressing Space didn’t cause the CheckBox to toggle, but left clicking on the CheckBox did. What’s going on?
    

    
    My **guess** is that the default style applied to the **DataGridCheckBoxColumn**’s CheckBox in non-edit mode (ElementStyle) sets the **IsHitTestVisible** property to **False** to disable clicking on the cell. The keyboard events are swallowed by the DataGrid using Preview events – so no styling is required to prevent keyboard access.
    

    
    The rule would therefore appear to be that if you set **ElementStyle** on the **DataGridCheckBoxColumn** you must include **IsHitTestVisible=”False”** to prevent it from “seeming” that the control allows edits.
    
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">Style  </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span><span style="color: blue">=&quot;CheckBoxStyle&quot; </span><span style="color: red">TargetType</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">CheckBox</span><span style="color: blue">}&quot; </span><span style="color: red">BasedOn</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: blue">{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">CheckBox</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}}&quot;&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;HorizontalAlignment&quot; </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Center&quot;/&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;Margin&quot; </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;0,2,0,0&quot;/&gt;
&lt;/</span><span style="color: #a31515">Style</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">&gt;
&lt;</span><span style="color: #a31515">Style  </span><span style="color: red">x</span><span style="color: blue">:</span><span style="color: red">Key</span><span style="color: blue">=&quot;ReadOnlyCheckBoxStyle&quot; </span><span style="color: red">TargetType</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">x</span><span style="color: blue">:</span><span style="color: #a31515">Type </span><span style="color: red">CheckBox</span><span style="color: blue">}&quot; </span><span style="color: red">BasedOn</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">CheckBoxStyle</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;&gt;
&lt;</span><span style="color: #a31515">Setter </span><span style="color: red">Property</span><span style="color: blue">=&quot;IsHitTestVisible&quot; </span><span style="color: red">Value</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;False&quot;/&gt;
&lt;/</span><span style="color: #a31515">Style</span><span style="color: blue">&gt;</span></font></font></pre>
<pre class="code"><font size="1"><font face="Verdana"><span style="color: blue">&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridCheckBoxColumn </span><span style="color: red">Binding</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">IsPensioner</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;
</span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Pensioner?&quot;
</span><span style="color: red">ElementStyle</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">ReadOnlyCheckBoxStyle</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;
</span><span style="color: red">EditingElementStyle</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">CheckBoxStyle</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;/&gt;
&lt;</span><span style="color: #a31515">toolkit</span><span style="color: blue">:</span><span style="color: #a31515">DataGridCheckBoxColumn </span><span style="color: red">Binding</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">Binding </span><span style="color: red">IsEditing</span><span style="color: blue">,</span><span style="color: red">Mode</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=OneWay}&quot;
</span><span style="color: red">Header</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">=&quot;Is Editing&quot;
</span><span style="color: red">ElementStyle</span><span style="color: blue">=&quot;{</span><span style="color: #a31515">StaticResource </span><span style="color: red">ReadOnlyCheckBoxStyle</span></font></font><font size="1"><font face="Verdana"><span style="color: blue">}&quot;
</span><span style="color: red">IsReadOnly</span><span style="color: blue">=&quot;True&quot;/&gt;</span></font></font>

<a href="http://11011.net/software/vspaste"></a><a href="http://11011.net/software/vspaste"></a><a href="http://11011.net/software/vspaste"></a>

