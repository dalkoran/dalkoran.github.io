---
layout: post
title: "Validation Panel"
date: 2007-10-11 15:14
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---
<strike></strike> 

The panel I've been working on to display rich validation messages is slowly coming along. I spent quite a bit of effort over the last two nights making it a well behaved "lookless" control.
 

The panel itself is simply a class derived from ItemsControl - has no XAML of its own. I then have a Themes/Generic.xaml file that contains the default rendering for the control as follows.


<font face="Verdana"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ResourceDictionary
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">x</span><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">local</span><span style="color: rgb(0,0,255)">="clr-namespace:Spencen.Windows.Controls.Validation"&gt;
</span><span style="color: rgb(0,0,255)">   &lt;</span><span style="color: rgb(163,21,21)">Style</span><span style="color: rgb(255,0,0)"> TargetType</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Type</span><span style="color: rgb(255,0,0)"> local</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">ValidationPanel</span><span style="color: rgb(0,0,255)">}"&gt;
</span><span style="color: rgb(163,21,21)"></span><span style="color: rgb(0,128,0)">        &lt;!-- Default property values --&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(255,0,0)"> Property</span><span style="color: rgb(0,0,255)">="Background"</span><span style="color: rgb(255,0,0)"> <br>                   </span><span style="color: rgb(255,0,0)">Value</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">DynamicResource</span><span style="color: rgb(0,0,255)"> {</span><span style="color: rgb(163,21,21)">x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Static</span><span style="color: rgb(255,0,0)"> SystemColors</span><span style="color: rgb(0,0,255)">.</span><span style="color: rgb(255,0,0)">InfoBrushKey</span><span style="color: rgb(0,0,255)">}}"/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(255,0,0)"> Property</span><span style="color: rgb(0,0,255)">="BorderBrush"</span><span style="color: rgb(255,0,0)"> <br>                   Value</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">DynamicResource</span><span style="color: rgb(0,0,255)"> {</span><span style="color: rgb(163,21,21)">x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Static</span><span style="color: rgb(255,0,0)"> SystemColors</span><span style="color: rgb(0,0,255)">.</span><span style="color: rgb(255,0,0)">InfoTextBrushKey</span><span style="color: rgb(0,0,255)">}}"/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(255,0,0)"> Property</span><span style="color: rgb(0,0,255)">="BorderThickness"</span><span style="color: rgb(255,0,0)"> Value</span><span style="color: rgb(0,0,255)">="1.5"/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(255,0,0)"> Property</span><span style="color: rgb(0,0,255)">="Margin"</span><span style="color: rgb(255,0,0)"> Value</span><span style="color: rgb(0,0,255)">="2,4"/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(255,0,0)"> Property</span><span style="color: rgb(0,0,255)">="Padding"</span><span style="color: rgb(255,0,0)"> Value</span><span style="color: rgb(0,0,255)">="1"/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(255,0,0)"> Property</span><span style="color: rgb(0,0,255)">="Template"&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter.Value</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ControlTemplate</span><span style="color: rgb(255,0,0)"> TargetType</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">Type</span><span style="color: rgb(255,0,0)"> local</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">ValidationPanel</span><span style="color: rgb(0,0,255)">}"&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Border</span><span style="color: rgb(255,0,0)"> Background</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> RelativeSource</span><span style="color: rgb(0,0,255)">={</span><span style="color: rgb(163,21,21)">RelativeSource</span><span style="color: rgb(255,0,0)"> TemplatedParent</span><span style="color: rgb(0,0,255)">},</span><span style="color: rgb(255,0,0)"> <br>                                                             Path</span><span style="color: rgb(0,0,255)">=Background}"
</span>                              <span style="color: rgb(255,0,0)"> BorderBrush</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">TemplateBinding</span><span style="color: rgb(255,0,0)"> BorderBrush</span><span style="color: rgb(0,0,255)">}"
</span>                              <span style="color: rgb(255,0,0)"> BorderThickness</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">TemplateBinding</span><span style="color: rgb(255,0,0)"> BorderThickness</span><span style="color: rgb(0,0,255)">}"
</span>                              <span style="color: rgb(255,0,0)"> Margin</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">TemplateBinding</span><span style="color: rgb(255,0,0)"> Margin</span><span style="color: rgb(0,0,255)">}"
</span>                              <span style="color: rgb(255,0,0)"> CornerRadius</span><span style="color: rgb(0,0,255)">="2"&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ItemsPresenter</span><span style="color: rgb(255,0,0)"> Margin</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">TemplateBinding</span><span style="color: rgb(255,0,0)"> Padding</span><span style="color: rgb(0,0,255)">}"/&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Border</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ControlTemplate</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Setter.Value</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(255,0,0)"> Property</span><span style="color: rgb(0,0,255)">="ItemTemplate"&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter.Value</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">DataTemplate</span><span style="color: rgb(255,0,0)"> DataType</span><span style="color: rgb(0,0,255)">="validation:ValidationContent"&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Grid</span><span style="color: rgb(255,0,0)"> Margin</span><span style="color: rgb(0,0,255)">="4"&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Grid.Resources</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">local</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">SeverityToImageConverter</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Key</span><span style="color: rgb(0,0,255)">="imageConverter"/&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Grid.Resources</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Grid.ColumnDefinitions</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ColumnDefinition</span><span style="color: rgb(255,0,0)"> Width</span><span style="color: rgb(0,0,255)">="Auto"/&gt;
</span><span style="color: rgb(163,21,21)">                            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ColumnDefinition</span><span style="color: rgb(0,0,255)">/&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Grid.ColumnDefinitions</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Grid.RowDefinitions</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">RowDefinition</span><span style="color: rgb(0,0,255)">/&gt;
</span><span style="color: rgb(163,21,21)">                            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">RowDefinition</span><span style="color: rgb(0,0,255)">/&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Grid.RowDefinitions</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Image</span><span style="color: rgb(255,0,0)"> Source</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=Severity,</span><span style="color: rgb(255,0,0)"> <br>                                                          Converter</span><span style="color: rgb(0,0,255)">={</span><span style="color: rgb(163,21,21)">StaticResource</span><span style="color: rgb(255,0,0)"> imageConverter</span><span style="color: rgb(0,0,255)">}}"</span><span style="color: rgb(255,0,0)"> <br>                                   </span><span style="color: rgb(255,0,0)">Stretch</span><span style="color: rgb(0,0,255)">="None"</span><span style="color: rgb(255,0,0)"> Margin</span><span style="color: rgb(0,0,255)">="0,0,2,0"/&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TextBlock</span><span style="color: rgb(255,0,0)"> Text</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=MessageText}"</span><span style="color: rgb(255,0,0)"> Grid.Column</span><span style="color: rgb(0,0,255)">="1"/&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">TextBlock</span><span style="color: rgb(255,0,0)"> Text</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=MessageDetailText}"</span><span style="color: rgb(255,0,0)"> <br>                                        Foreground</span><span style="color: rgb(0,0,255)">="DarkGray"</span><span style="color: rgb(255,0,0)"> <br>                                        FontStyle</span><span style="color: rgb(0,0,255)">="Italic"</span><span style="color: rgb(255,0,0)"> <br>                                        Grid.Column</span><span style="color: rgb(0,0,255)">="1"</span><span style="color: rgb(255,0,0)"> Grid.Row</span><span style="color: rgb(0,0,255)">="1"</span><span style="color: rgb(255,0,0)"> TextWrapping</span><span style="color: rgb(0,0,255)">="Wrap"/&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Grid</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">DataTemplate</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Setter.Value</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(255,0,0)"> Property</span><span style="color: rgb(0,0,255)">="ItemsPanel"&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Setter.Value</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ItemsPanelTemplate</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">StackPanel</span><span style="color: rgb(0,0,255)">/&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ItemsPanelTemplate</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Setter.Value</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Setter</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Style</span><span style="color: rgb(0,0,255)">&gt;
&lt;/</span><span style="color: rgb(163,21,21)">ResourceDictionary</span><span style="color: rgb(0,0,255)">&gt;</span></font></font>
<a href="http://11011.net/software/vspaste"></a>


This displays the panel as shown below which is fairly standard for a message display area.



![ThemedStatusPanel](/images/ThemedStatusPanel_1.png) 



I then went ahead and created a customised style that changes not just colours but also changes the layout a little. I finally came up with this "vista-ish" look - although its probably a little overdone for real-world use. In this version the secondary text (which in the contrived example is a little redundant) is displayed via a popup invoked by the Help image button.



![StyledStatusPanel](/images/StyledStatusPanel_3.png) 



Now I'm starting on some custom validation rules. One of the things that I want to achieve is to have the rules specify a set of optional commands. These commands will then be used by the Validation Panel to build a context menu (configurable UI of course). Example commands might be *focus to control in error*, *apply default* etc. 



Since the look of each item in the panel is determined by a DataTemplate which itself is bound to a type - I also want to try creating a derived ValidationContent class so that I can try multiple item UIs within the same panel. This might be useful for example in displaying an "acknowledgement" panel that places the form "in error" until the user has acknowledged the message via a CheckBox. Could be used in scenarios like the typical "I acknowledge that I've read your 20 page EULA" message that is often displayed in installers.


