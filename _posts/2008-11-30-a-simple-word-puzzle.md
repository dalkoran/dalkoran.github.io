---
layout: post
title: "A Simple Word Puzzle"
date: 2008-11-30 12:37
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


A few weeks back my young son showed me some work that he’d been doing at school – constructing a simple word puzzle by hiding a few known words within a grid of letters. His approach to this was very simplistic – but got me thinking that writing an application to do the same would be trivial and thus something that could keep me busy on the bus trips to and from work.
  

One night to get started and then a few bus trips later and I’ve got the following:
  

![Word Puzzle 1](/images/Word%20Puzzle%201_1.png "Word Puzzle 1") 
  

The puzzle board itself is actually two ItemsControls layered on top of each other. The lower control has its items laid out using a UniformGrid and each item represents a letter tile. The topmost control superimposes the “solved” words on top of the lower grid. I originally tried doing this just be altering the style of the individual tiles – but this didn’t achieve the effect I was after and got overly complex when a single letter was used by multiple words.
  

<font size="1"><font face="Verdana"><span style="color: rgb(163,21,21)">       </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Viewbox</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Grid</span><span style="color: rgb(255,0,0)"> VerticalAlignment</span><span style="color: rgb(0,0,255)">=&quot;Center&quot;</span><span style="color: rgb(255,0,0)"> HorizontalAlignment</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;Center&quot;&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ItemsControl</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">=&quot;letterItemsControl&quot;</span>
<span style="color: rgb(255,0,0)"> ItemsSource</span><span style="color: rgb(0,0,255)">=&quot;{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> LetterStream</span><span style="color: rgb(0,0,255)">}&quot;</span>
<span style="color: rgb(255,0,0)"> MouseDown</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;letterItemsControl_MouseDown&quot;
</span>                             <span style="color: rgb(255,0,0)"> MouseMove</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;letterItemsControl_MouseMove&quot;
</span>                             <span style="color: rgb(255,0,0)"> MouseUp</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;letterItemsControl_MouseUp&quot;&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ItemsControl.ItemsPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ItemsPanelTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">UniformGrid</span><span style="color: rgb(255,0,0)"> Rows</span><span style="color: rgb(0,0,255)">=&quot;{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Height</span><span style="color: rgb(0,0,255)">}&quot;</span><span style="color: rgb(255,0,0)"> Columns</span><span style="color: rgb(0,0,255)">=&quot;{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Width</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">}&quot;/&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ItemsPanelTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ItemsControl.ItemsPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ItemsControl</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ItemsControl</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">=&quot;wordItemsControl&quot;</span><span style="color: rgb(255,0,0)"> ItemsSource</span><span style="color: rgb(0,0,255)">=&quot;{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Words</span><span style="color: rgb(0,0,255)">}&quot;</span>
<span style="color: rgb(255,0,0)"> IsHitTestVisible</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">=&quot;False&quot;
</span>                             <span style="color: rgb(255,0,0)"> ItemTemplate</span><span style="color: rgb(0,0,255)">=&quot;{</span><span style="color: rgb(163,21,21)">StaticResource</span><span style="color: rgb(255,0,0)"> WordSelection</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">}&quot;&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ItemsControl.ItemsPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ItemsPanelTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                            </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Canvas</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">/&gt;
</span><span style="color: rgb(163,21,21)">                        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ItemsPanelTemplate</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ItemsControl.ItemsPanel</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ItemsControl</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Grid</span></font></font><font size="1"><font face="Verdana"><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Viewbox</span><span style="color: rgb(0,0,255)">&gt;</span></font></font>

<a href="http://11011.net/software/vspaste"></a>


Of course it was pointed out to me that having each letter tile rendered as a set of controls wouldn’t scale well at all. Sure enough, even at 20x20 things start to slow down. Rendering a grid at 100x100 is painfully slow – although to be fare you can’t read the letter tiles at that size either – and it wouldn’t be much fun looking for words on a 10000 letter grid. Whilst a better implementation probably lies down the path of creating a BoardControl that renders all the tiles as one geometry group, I just liked the idea of being able to easily apply styles to the letter tiles, word selection etc.



As it stands this isn’t really finished – but its probably as close as I’ll get. I showed it to my son – he was bored of it after about 30 seconds ![](http://blog.spencen.com/emoticons/sad.png).



I’ve posted an XBAP version <a href="http://www.spencen.com/Alphabet/AlphabetXBap.xbap" target="_blank">here</a>.


