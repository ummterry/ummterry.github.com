---
layout: post
title: "Writing an application on Mac OS X with Mono C#"
description: ""
category: Other 
tags: [C#, Mac, Mono, Xamarin]
---
{% include JB/setup %}
I have a windows form application written in C#, and I thought it would be cool if I can migrate it on to Mac OS with [Mono](http://www.mono-project.com/Main_Page), which claims to provide portable C# on both Windows, Linux and Mac OS.

First I installed [Mono SDK](http://www.go-mono.com/mono-downloads/download.html) and [Xamarin Studio](http://xamarin.com/studio), and surprisingly found it able to import the visual studio project directly. Then came the disaster. Strange errors came one after another, and most of them doen’t make sense at all. The errors, as figured out later, are almost possible to fix because of Mono’s poor support for some of the windows form controls such as System.Windows.Forms.WebBrowser and System.Windows.Forms.NotifyIcon.

So I had to give up the UI code based on Windows.Forms and try to find some substitute. [Xamarin.Mac](http://xamarin.com/mac) seem to be a suitable option, with the basis of [MonoMac](http://www.mono-project.com/MonoMac). Xamarin provides a good Hello World example for newcomer to get started. The tutorial walks through creating a basic Xamarin.Mac application, demonstrating the development toolchain, including Xamarin Studio and Xcode, as well as explaining the basic structure of a Xamarin.Mac application (which is so different from the windows form application).

Developing with Xamarin.Mac is such a headache, because you can seldom find any examples except the Xamarin document. However, the application is based on the same UI controls of Cocoa applications, thus it looks as native as the Objective-C applications.

The application cannot run on a naked Mac without the [Mono Runtime](http://www.go-mono.com/mono-downloads/download.html), which makes it less attractive as every endpoint user need to install the Mono Runtime to run the application.