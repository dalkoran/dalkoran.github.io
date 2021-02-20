---
title: "Thumbnail Images"
date: 2008-01-09 15:42
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---


When I wrote my little [PhotoPlay](http://blog.spencen.com/2007/08/15/photoplay-v01--winforms.aspx) applet I quickly realised that loading large numbers of images into memory was slow and enormously expensive in terms of memory use. I remember the first time I pointed PhotoPlay at my year 2004 photo folder - I think I managed to kill the task at around 3Gb of virtual memory use.
 

So I did a quick search and found a great bit of code from Kourosh Derakshan that allows you to use the thumbnail images that are embedded into photos by almost all digital cameras. The images are actually stored as metadata against the real image along with all the other EXIF metadata tags such as camera model, flash type, focal length etc.
 

Using this code I was able to load 100s of photos in several seconds, as opposed to minutes, and the memory usage was amazingly small. Of course the gotcha is that if the image doesn't have a thumbnail this method won't help you. If the image didn't come from a camera then chances are it won't have a thumbnail - so really this is for digital photos only. [The main reason the thumbnail image is stored is because this is the image that the cameras use themselves when you browse photos on the camera.]
 

The key to this code is the System.Drawing.Image.FromStream() method. The overload used has a parameter "validateImageData" that if set to false means the image isn't loaded into memory. Not obvious and certainly not apparent from reading the documentation! You end up with an Image instance that still allows you access the metadata properties without having any overhead of processing the image itself.


    <span style="font-size: 8pt; font-family: verdana"><font color="#004000"><span>// Written by Kourosh Derakshan - minor mods by Nigel Spencer.
</span>        </font><span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> </span><span style="color: rgb(128,128,128)">&lt;summary&gt;
</span>        <span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> Gets the thumbnail from the image metadata. Returns null if no thumbnail
</span>        <span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> is stored in the image metadata
</span>        <span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> </span><span style="color: rgb(128,128,128)">&lt;/summary&gt;
</span>        <span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> </span><span style="color: rgb(128,128,128)">&lt;param name="path"&gt;</span><span style="color: rgb(0,128,0)">The full path to the image.</span><span style="color: rgb(128,128,128)">&lt;/param&gt;
</span>        <span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> </span><span style="color: rgb(128,128,128)">&lt;returns&gt;</span><span style="color: rgb(0,128,0)">The thumbnail image contained within the metadata (EXIF) of the image, or <br>        </span></span><span style="font-size: 8pt; font-family: verdana"><span style="color: rgb(128,128,128)">/// &lt;see langword="null"/&gt;</span><span style="color: rgb(0,128,0)"> if no thumbnail data is present.</span><span style="color: rgb(128,128,128)">&lt;/returns&gt;
</span>        <span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> </span><span style="color: rgb(128,128,128)">&lt;remarks&gt;
</span>        <span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> The ExifTag metadata is copied from the original image to the thumbnail that is returned.
</span>        <span style="color: rgb(128,128,128)">///</span><span style="color: rgb(0,128,0)"> </span><span style="color: rgb(128,128,128)">&lt;/remarks&gt;
</span>        <span style="color: rgb(0,0,255)">public</span> <span style="color: rgb(0,0,255)">static</span> <span style="color: rgb(43,145,175)">Image</span> GetThumbnail (<span style="color: rgb(0,0,255)">string</span> path)
{
<span style="color: rgb(43,145,175)">FileStream</span> fileStream = <span style="color: rgb(0,0,255)">null</span>;
<span style="color: rgb(0,0,255)">try
</span>            {
fileStream = <span style="color: rgb(43,145,175)">File</span>.OpenRead(path);
<span style="color: rgb(0,128,0)">// Last parameter tells GDI+ not the load the actual image data
</span>                <span style="color: rgb(43,145,175)">Image</span> originalImage = <span style="color: rgb(43,145,175)">Image</span>.FromStream(fileStream, <span style="color: rgb(0,0,255)">false</span>, <span style="color: rgb(0,0,255)">false</span>);
<span style="color: rgb(0,128,0)">// GDI+ throws an error if we try to read a property when the image
</span>                <span style="color: rgb(0,128,0)">// doesn't have that property. Check to make sure the thumbnail property
</span>                <span style="color: rgb(0,128,0)">// item exists.
</span>                <span style="color: rgb(0,0,255)">bool</span> propertyFound = <span style="color: rgb(0,0,255)">false</span>;
<span style="color: rgb(0,0,255)">for</span> (<span style="color: rgb(0,0,255)">int</span> i = 0; i &lt; originalImage.PropertyIdList.Length; i++)
<span style="color: rgb(0,0,255)">if</span> (originalImage.PropertyIdList[i] == (<span style="color: rgb(0,0,255)">int</span>)<span style="color: rgb(43,145,175)">ExifTags</span>.ThumbnailData)
{
propertyFound = <span style="color: rgb(0,0,255)">true</span>;
<span style="color: rgb(0,0,255)">break</span>;
}
<span style="color: rgb(0,0,255)">if</span> (!propertyFound)
<span style="color: rgb(0,0,255)">return</span> <span style="color: rgb(0,0,255)">null</span>;
<span style="color: rgb(43,145,175)">PropertyItem</span> thumbnailPropertyItem = originalImage.GetPropertyItem((<span style="color: rgb(0,0,255)">int</span>)<span style="color: rgb(43,145,175)">ExifTags</span>.ThumbnailData);
<span style="color: rgb(0,128,0)">// The image data is in the form of a byte array. Write all
</span>                <span style="color: rgb(0,128,0)">// the bytes to a stream and create a new image from that stream
</span>                <span style="color: rgb(0,0,255)">byte</span>[] imageBytes = thumbnailPropertyItem.Value;
<span style="color: rgb(43,145,175)">MemoryStream</span> stream = <span style="color: rgb(0,0,255)">new</span> <span style="color: rgb(43,145,175)">MemoryStream</span>(imageBytes.Length);
stream.Write(imageBytes, 0, imageBytes.Length);
<span style="color: rgb(43,145,175)">Image</span> thumbnailImage = <span style="color: rgb(43,145,175)">Image</span>.FromStream(stream);
<span style="color: rgb(0,128,0)">// Copy all the original properties to the thumbnail.
</span>                <span style="color: rgb(0,0,255)">for</span> (<span style="color: rgb(0,0,255)">int</span> i = 0; i &lt; originalImage.PropertyIdList.Length; i++)
{
<span style="color: rgb(43,145,175)">PropertyItem</span> itemToCopy = originalImage.GetPropertyItem(originalImage.PropertyIdList[i]);
thumbnailImage.SetPropertyItem(itemToCopy);
}
originalImage.Dispose();
<span style="color: rgb(0,0,255)">return</span> thumbnailImage;
}
<span style="color: rgb(0,0,255)">finally
</span>            {
<span style="color: rgb(0,0,255)">if</span> (fileStream != <span style="color: rgb(0,0,255)">null</span>)
{
fileStream.Dispose();
}
}
}
<a href="http://11011.net/software/vspaste"></span></a>


So this method has been very useful to me, but in the last week I've been trying to do something similar in WPF. My first attempt was to just jump straight in and bind a ListBox to some images. This was incredibly slow to load - I mean horrendous! I wanted to use the GetThumbnail method but of course WPF doesn't understand System.Drawing.Image - I need to use System.Windows.Media.ImageSource instead. I've messed briefly with DecodePixelWidth and Height but these only seem to control the size of the image that is held in memory (which is good) but it still takes forever to produce the thumbnail from the full image. I also noticed that the BitmapFrame class, not the BitmapSource seems to be more helpful because it allows Thumbnail and metadata access. 



Well - its late - so it'll have to be a job for tomorrow. Hopefully I'll find an equivalent way of doing this in WPF that works and be able to blog the solution. Assuming I can get it working in WPF I'll also include some performance comparisons against the method shown here.


