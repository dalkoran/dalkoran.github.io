---title: "Windows 7 RC and Visual Studio 2010 Beta – Hands on"
date: 2009-05-23 14:31
author: spencen
comments: true
categories: [.NET, Development, WPF]
tags: []
---
A while back I posted that I had [installed the Windows 7 64bit Beta](http://blog.spencen.com/2009/01/28/windows-7-beta-experience.aspx) on my main development machine as a dual boot with Vista and that all looked good. A couple of weeks after that post my machine inexplicably stopped booting into Vista despite my attempts to perform a repair. So I’ve been running Windows 7 now for quite a while.
  

This week, along with everyone else I re-paved the machine with Windows 7 RC. On top of that I installed Visual Studio 2010 Beta and also jumped through the numerous hoops to install VS2010 Team Foundation Server on a virtual Windows 2008 Server instance [running on my WHS box](http://blog.spencen.com/2008/06/26/windows-home-server-2008.aspx).
  

So far:
  

*   The Windows 7 taskbar is improved by the additional key/mouse combos, e.g. Ctrl-LeftClick to quickly cycle through instances of an application.
*   Windows 7 install is still the best Windows installation ever – which is good because for a while there Windows was getting consistently worse.
*   Team Foundation Server is still a long installation process, but mainly now due just to the pre-requisites, e.g. Windows Server, IIS, SharePoint, SQL Server/Reporting Services. The install documentation is good – but it would pay to read through most of it before you install – which of course no-one (myself included) will ever do.
*   Visual Studio 2010 is looking pretty good. If the new WPF text editor is anything to go by then the new font improvements in WPF 4/[DirectWrite](http://channel9.msdn.com/pdc2008/PC18/) have worked well.
*   There are typical Beta quirks in VS 2010 – it crashes occasionally, when you zoom in the editor the scroll bars also zoom (which looks most odd).
*   Visual Studio start page is so easy to customize now. This [article](http://blogs.msdn.com/vsxteam/archive/2009/05/20/visual-studio-2010-beta-1-start-page-customization.aspx) shows how you simple toggle an option and then build whatever XAML you like for the startpage. Either just extending what’s already there or completely re-writing or re-skinning. Very cool!
*   A number of Visual Studio extensions are beginning to popup. Including editor extensions for creating Regex’s with intellisense, adding images inline with source code as well as some project templates, e.g. WPF application with tray icon.

