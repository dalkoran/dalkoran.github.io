---
layout: post
title: "Validation Template without Binding Errors"
date: 2008-10-10 15:16
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


WPF guru Josh Smith has just put up a great post <a href="http://joshsmithonwpf.wordpress.com/2008/10/08/binding-to-validationerrors0-without-creating-debug-spew/" target="_blank">here</a> about how to access the validation errors on a WPF control without getting lots of binding debug output. Most samples (including MSDN documentation) suggest you should use Validation.Errors[0] which generate debug output for a binding failure whenever there are no errors (since Errors[0] doesn’t exist).
  

The format of the template that I commonly use (which also avoids the Errors[0] issue in almost exactly the same way) is as follows:
  

<font size="1"><font face="Verdana"><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ControlTemplate</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Key</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;DefaultErrorTemplate&quot;&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">DockPanel</span><span style="color: rgb(255,0,0)"> DataContext</span><span style="color: rgb(0,0,255)">=&quot;{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> ElementName</span><span style="color: rgb(0,0,255)">=adorner,</span><span style="color: rgb(255,0,0)">   
                                                                   Path</span><span style="color: rgb(0,0,255)">=AdornedElement.(Validation.Errors)</span>/</font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">ErrorContent}&quot;&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Ellipse</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">=&quot;Ellipse&quot;</span><span style="color: rgb(255,0,0)">   
                                DockPanel.Dock</span><span style="color: rgb(0,0,255)">=&quot;Right&quot;</span><span style="color: rgb(255,0,0)">   
                                Margin</span><span style="color: rgb(0,0,255)">=&quot;2,0,2,0&quot;</span><span style="color: rgb(255,0,0)">   
                                Width</span><span style="color: rgb(0,0,255)">=&quot;14&quot;</span><span style="color: rgb(255,0,0)"> Height</span><span style="color: rgb(0,0,255)">=&quot;14&quot;</span><span style="color: rgb(255,0,0)">   
                                VerticalAlignment</span><span style="color: rgb(0,0,255)">=&quot;Center&quot;</span>
<span style="color: rgb(255,0,0)">        Stroke</span><span style="color: rgb(0,0,255)">=&quot;#40000000&quot;</span><span style="color: rgb(255,0,0)"> StrokeThickness</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;2&quot; </span><span style="color: rgb(255,0,0)">Fill</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;Red&quot;&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Ellipse.ToolTip</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Border</span><span style="color: rgb(255,0,0)"> MaxWidth</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;350&quot;&gt;
</span><span style="color: rgb(163,21,21)">                            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ContentControl</span><span style="color: rgb(255,0,0)"> FontSize</span><span style="color: rgb(0,0,255)">=&quot;14&quot;</span><span style="color: rgb(255,0,0)"> Content</span><span style="color: rgb(0,0,255)">=&quot;{</span><span style="color: rgb(163,21,21)">Binding</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">}&quot;/&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Border</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Ellipse.ToolTip</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Ellipse</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Border</span><span style="color: rgb(255,0,0)"> BorderBrush</span><span style="color: rgb(0,0,255)">=&quot;#40FFAF00&quot;</span><span style="color: rgb(255,0,0)"> BorderThickness</span><span style="color: rgb(0,0,255)">=&quot;2&quot;</span><span style="color: rgb(255,0,0)"> IsHitTestVisible</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;False&quot;&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Border.Background</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">SolidColorBrush</span><span style="color: rgb(255,0,0)"> Color</span><span style="color: rgb(0,0,255)">=&quot;Red&quot;</span><span style="color: rgb(255,0,0)"> Opacity</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;0.2&quot;/&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Border.Background</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">AdornedElementPlaceholder</span><span style="color: rgb(255,0,0)"> Margin</span><span style="color: rgb(0,0,255)">=&quot;-2&quot;</span><span style="color: rgb(255,0,0)"> Name</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;adorner&quot;/&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Border</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">DockPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ControlTemplate</span><span style="color: rgb(0,0,255)">&gt;</span></font></font>

<a href="http://11011.net/software/vspaste"></a>


Normally instead of hard coding the colours I would use a colour converter that maps the severity of the error (through a custom property) to the relevant colour, e.g. error=red, warning=orange etc.



My equivalent of Josh’s sample project is <a href="http://www.spencen.com/Downloads/CleanlyBindToValidationErrorsNS.zip" target="_blank">here</a>.



<a href="http://www.spencen.com/Downloads/CleanlyBindToValidationErrorsNS.zip">![CleanlyBindToValidationErrors](/images/CleanlyBindToValidationErrors_1.png "CleanlyBindToValidationErrors")</a>


