---

title: "Thumbnail Solutions"
date: 2008-01-10 12:34
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


So based on [last nights performance troubles loading thumbnail images](http://blog.spencen.com/2008/01/10/thumbnail-images.aspx) I spent a couple of minutes "googling" and discovered the following useful links:
 

[http://blogs.msdn.com/dditweb/archive/2007/08/22/speeding-up-image-loading-in-wpf-using-thumbnails.aspx](http://blogs.msdn.com/dditweb/archive/2007/08/22/speeding-up-image-loading-in-wpf-using-thumbnails.aspx)  

[http://msdn2.microsoft.com/en-us/library/system.windows.media.imaging.bitmapframe_members.aspx](http://msdn2.microsoft.com/en-us/library/system.windows.media.imaging.bitmapframe_members.aspx)  

I tried the solution in the first link which involved creating a converter to use so that binding to a URI can result in a BitmapSource that uses the DecodePixelWidth/Height property.

<span style="font-size: 8pt; font-family: verdana">    <span rgb(0,0,255)? color:>public</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">UriToBitmapConverter</span> : <span style="color: rgb(43,145,175)">IValueConverter
</span>    {
<span style="color: rgb(0,0,255)">public</span> UriToBitmapConverter()
{
DecodeResolution = 100;
}
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">int</span> DecodeResolution { <span style="color: rgb(0,0,255)">get</span>; <span style="color: rgb(0,0,255)">set</span>; }
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">object</span> Convert(<span style="color: rgb(0,0,255)">object</span> value, <span style="color: rgb(43,145,175)">Type</span> targetType, <span style="color: rgb(0,0,255)">object</span> parameter, <span style="color: rgb(43,145,175)">CultureInfo</span> culture)
{
<span style="color: rgb(43,145,175)">BitmapImage</span> bi = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">BitmapImage</span>();
bi.BeginInit();
bi.DecodePixelWidth = DecodeResolution;
bi.CacheOption = <span style="color: rgb(43,145,175)">BitmapCacheOption</span>.OnLoad;
bi.UriSource = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Uri</span>( value.ToString() );
bi.EndInit();
<span style="color: rgb(0,0,255)">return</span> bi;
}
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">object</span> ConvertBack(<span style="color: rgb(0,0,255)">object</span> value, <span style="color: rgb(43,145,175)">Type</span> targetType, <span style="color: rgb(0,0,255)">object</span> parameter, <span style="color: rgb(43,145,175)">CultureInfo</span> culture)
{
<span style="color: rgb(0,0,255)">throw</span> <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Exception</span>(<span style="color: rgb(163,21,21)">"The method or operation is not implemented."</span>);
}
}</pre></span><a href="http://11011.net/software/vspaste"></a>

    
    This converter is then hooked up in the XAML something like as follows.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Window</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Class</span><span style="color: rgb(0,0,255)">="ThumbnailLoading.Window1"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
</span>   <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">x</span><span style="color: rgb(0,0,255)">="http://schemas.microsoft.com/winfx/2006/xaml"
</span>       <span style="color: rgb(255,0,0)"> xmlns</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">local</span><span style="color: rgb(0,0,255)">="clr-namespace:ThumbnailLoading"
</span>   <span style="color: rgb(255,0,0)"> Title</span><span style="color: rgb(0,0,255)">="Window1"</span><span style="color: rgb(255,0,0)"> Height</span><span style="color: rgb(0,0,255)">="300"</span><span style="color: rgb(255,0,0)"> Width</span><span style="color: rgb(0,0,255)">="300"&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Window.Resources</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">local</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">PhotoCollection</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Key</span><span style="color: rgb(0,0,255)">="photos"</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Name</span><span style="color: rgb(0,0,255)">="photos"/&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">local</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">UriToBitmapConverter</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Key</span><span style="color: rgb(0,0,255)">="uriToBitmapConverter"</span><span style="color: rgb(255,0,0)"> DecodeResolution</span><span style="color: rgb(0,0,255)">="60"/&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">Window.Resources</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">StackPanel</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ListView</span><span style="color: rgb(255,0,0)"> ItemsSource</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">StaticResource</span><span style="color: rgb(255,0,0)"> photos</span><span style="color: rgb(0,0,255)">}"&gt;
</span><span style="color: rgb(163,21,21)">             </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">ListView.ItemTemplate</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">DataTemplate</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">                    </span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Image</span><span style="color: rgb(255,0,0)"> Width</span><span style="color: rgb(0,0,255)">="60"</span><span style="color: rgb(255,0,0)"> <br>                                Source</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=Uri,</span><span style="color: rgb(255,0,0)"> Converter</span><span style="color: rgb(0,0,255)">={</span><span style="color: rgb(163,21,21)">StaticResource</span><span style="color: rgb(255,0,0)"> uriToBitmapConverter</span><span style="color: rgb(0,0,255)">}}"/&gt;
</span><span style="color: rgb(163,21,21)">                </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">DataTemplate</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">            </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ListView.ItemTemplate</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">        </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">ListView</span><span style="color: rgb(0,0,255)">&gt;
</span><span style="color: rgb(163,21,21)">    </span><span style="color: rgb(0,0,255)">&lt;/</span><span style="color: rgb(163,21,21)">StackPanel</span><span style="color: rgb(0,0,255)">&gt;
&lt;/</span><span style="color: rgb(163,21,21)">Window</span><span style="color: rgb(0,0,255)">&gt;</span></span></pre><a href="http://11011.net/software/vspaste"></a>

    
    Whilst this is an improvement as I suggested in the previous post from the folks at DDITDev - it is still too slow. 
    

    
    So I spent some time reading the MSDN documentation. The BitmapFrame class seemed to have a lot of cool functionality - including the ability to access image metadata. They even have properties for the commonly used properties such as Camera Model and Rating - very nice! However, of most interest was the Thumbnail property which suggested that it would return the thumbnail stored with the image. I used the Binding converter idea and created my own converter as follows.
    <pre class="code"><span style="font-size: 8pt; font-family: verdana">    <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">class</span> <span style="color: rgb(43,145,175)">UriToThumbnailConverter</span> : <span style="color: rgb(43,145,175)">IValueConverter
</span>    {
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">object</span> Convert(<span style="color: rgb(0,0,255)">object</span> value, <span style="color: rgb(43,145,175)">Type</span> targetType, <span style="color: rgb(0,0,255)">object</span> parameter, <span style="color: rgb(43,145,175)">CultureInfo</span> culture)
{
<span style="color: rgb(43,145,175)">BitmapFrame</span> bi = <span style="color: rgb(43,145,175)">BitmapFrame</span>.Create(<span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Uri</span>(value.ToString()), <br>                                                                      <span style="color: rgb(43,145,175)">BitmapCreateOptions</span>.DelayCreation, <br>                                                                      <span style="color: rgb(43,145,175)">BitmapCacheOption</span>.OnDemand);
<span style="color: rgb(0,0,255)">return</span> bi.Thumbnail;
}
<span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">object</span> ConvertBack(<span style="color: rgb(0,0,255)">object</span> value, <span style="color: rgb(43,145,175)">Type</span> targetType, <span style="color: rgb(0,0,255)">object</span> parameter, <span style="color: rgb(43,145,175)">CultureInfo</span> culture)
{
<span style="color: rgb(0,0,255)">throw</span> <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">Exception</span>(<span style="color: rgb(163,21,21)">"The method or operation is not implemented."</span>);
}
}</span><font face="Verdana"> </font>



Then made the following changes to the XAML to declare and use the new binding converter.




<a href="http://11011.net/software/vspaste"><a href="http://11011.net/software/vspaste"></a>


>
<p style="font-size: 8pt; font-family: verdana"><span style="color: rgb(163,21,21)"></span><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">local</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(163,21,21)">UriToThumbnailConverter</span><span style="color: rgb(255,0,0)"> x</span><span style="color: rgb(0,0,255)">:</span><span style="color: rgb(255,0,0)">Key</span><span style="color: rgb(0,0,255)">="uriToThumbnailConverter"</span><span style="color: rgb(0,0,255)">/&gt; </span>





>
<p style="font-size: 8pt; font-family: verdana"><span style="color: rgb(0,0,255)">&lt;</span><span style="color: rgb(163,21,21)">Image</span><span style="color: rgb(255,0,0)"> Width</span><span style="color: rgb(0,0,255)">="60"</span><span style="color: rgb(255,0,0)"> Source</span><span style="color: rgb(0,0,255)">="{</span><span style="color: rgb(163,21,21)">Binding</span><span style="color: rgb(255,0,0)"> Path</span><span style="color: rgb(0,0,255)">=Uri,</span><span style="color: rgb(255,0,0)"> Converter</span><span style="color: rgb(0,0,255)">={</span><span style="color: rgb(163,21,21)">StaticResource</span><span style="color: rgb(255,0,0)"> uriToThumbnailConverter</span><span style="color: rgb(0,0,255)">}}"/&gt; </span>





This actually seemed to do the trick. I provided some statistics below which show the performance benefits. Of course in reality - as with PhotoPlay - any expensive image loading would normally happen on a background thread so maybe the binding converter isn't really the best way to manage this.

<table cellspacing="0" cellpadding="2" width="615" border="1">
<tbody>
<tr>
<td valign="top" width="185">&nbsp;</td>
<td valign="top" width="117">*Bind direct to Uri*</td>
<td valign="top" width="159">*Bind to Uri using DecodePixelWidth*</td>
<td valign="top" width="152">*Bind to Uri using embedded Thumbnail*</td></tr>
<tr>
<td valign="top" width="185">Loading 301 approx 280kb (1.1 megapixel) images</td>
<td valign="top" width="117"><font color="#ff0000">**1 minutes<br>1.2 Gb**</font></td>
<td valign="top" width="161"><font color="#ff8040">**30 seconds<br>120 Mb**</font></td>
<td valign="top" width="152"><font color="#008000">**4 seconds<br>180 Mb**</font></td></tr>
<tr>
<td valign="top" width="185">Loading 1102 approx 2Mb (5 megapixel) images</td>
<td valign="top" width="117"><font color="#ff0000">**Yeah right!**</font></td>
<td valign="top" width="163"><font color="#ff8040">**4 minutes 35 seconds<br>201 Mb**</font></td>
<td valign="top" width="152"><font color="#008000">**14 seconds<br>310 Mb**</font></td></tr></tbody></table>


Note that the DecodePixel width method using the UriToBitmapConverter actually uses less memory because the embedded Thumbnails are actually bigger that the decode width of 60 pixels that I specified.



I also found this Microsoft sample application [WPF Photo Viewer Demo](http://msdn2.microsoft.com/en-us/library/ms771331.aspx) which shows how Thumbnails and metadata can be extracted from digital photos.


