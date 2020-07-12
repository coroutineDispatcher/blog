---
title: 'Getting hands dirty with Flutter'
date: 2019-09-23T09:00:00.000+02:00
draft: false
aliases: [ "/2019/09/getting-hands-dirty-with-flutter.html" ]
tags : [Swift, Flutter Development, iOS, Cross-Platform, Javascript, Kotlin, Native vs Cross-Platform, Flutter for web., Dart, Android Development, Flutter]
author: "Stavro Xhardha"
---

_An Android Developer point of view._  
  

[![](https://1.bp.blogspot.com/-4o0osNnBTQM/XYeAmuWNzPI/AAAAAAAAPZM/u2BLbDoj8r8gWxdOUWFMEkqlImgaeBj5gCLcBGAsYHQ/s1600/flutter_app_design.jpg)](https://1.bp.blogspot.com/-4o0osNnBTQM/XYeAmuWNzPI/AAAAAAAAPZM/u2BLbDoj8r8gWxdOUWFMEkqlImgaeBj5gCLcBGAsYHQ/s1600/flutter_app_design.jpg)

  
  
I have never been a fan of cross-platforms until Flutter came by. Actually, it was my first experience with cross-platforms. Never have I ever tried React Native, Ionic or whatever. But what was intriguing about Flutter? Well, since I am a Google fan and a Native Android Developer, i thought I should give it a try.  
  
Jumping into Flutter was pretty easy. They have simple documentation comparing to the [Android Developers documentation](https://developer.android.com/guide), and I think it has a lot more details and better best-practices. Also, Android Studio supports flutter development by just installing [Dart and Flutter plugins](https://flutter.dev/docs/get-started/editor). So the only real configuration I made was telling Android Studio where my [Flutter SDK](https://flutter.dev/docs/get-started/install) location was.  
  
In order not to transfer this article to a tutorial, I'm just going to point out, what I think Flutter development is comparing to Android.  
  
**Honestly, Flutter is simpler.**  
Even though I was an inexperienced programmer when I started Android, I remember a lot of struggles when having to face a bug or implement something new in Android. I expected to face more trouble with the State, since should have been a new concept but I got into it pretty quick.  
  
**Dart > XML for design**  
The easiest thing I'm going to point out here, is a BottomNavigationView, or a BottomNavigationBar (flutterly speaking)**.** While in native code you have to care a lot about what happens when you press one element of the Navigation, in Flutter it's pretty simple:  
  
However, I still think as a Flutter beginner that the syntax is pretty strange comparing to the heavy Java and Kotlin structure (and Java's verbosity of course).  
  
**There are a lot of Widgets**  
Looks like I had to google a lot about implementing the right widget. I admit that I should have read the docs more, but still, widgets in Flutter are too many comparing to the Android Development views.  
   
**I loved lists.**  
A pain in the Android world is RecyclerViews and Adapters. They are not hard, but every time I have to create a list and display some data, I have this immediate reactions: "Oh great. Another adapter". In Flutter ListViews or GridViews were just too good.  
  
**Futures were great.**  
While Android has made a lot of improvements in Futures (or Deferred responses to be exact), still, I loved the way dart manages asynchronous methods:  
  
Adding some [FutureBuilder](https://api.flutter.dev/flutter/widgets/FutureBuilder-class.html) support, Flutter Futures are just sweet.  
  
_Note, one thing I learned in Flutter, is that if I'm overthinking how to solve the case, perhaps I'm doing it all wrong or know nothing about the problem._  
  
**Kotlin > Dart**  
Yea, Kotlin is a better language to program comparing to dart. They do look the same in some cases but my experience with Kotlin has always been very easy.  
  
**Flutter lags a little**  
Perhaps I didn't google enough, and my loading images had a high quality, but I did experienced lags and shakes especially on the web (even though we know web build it's still not ready for production yet).   
**I still don't know why can't I build a debug version for the web**  
The localhost:WHATEVER doesn't load a thing (The page was always blank. I didn't find much in Google, if you can help please share.), while building a release was pretty easy_**.**_ One thing to keep in mind is that you must prepare the project to support web. [Overall it's just 4 commands on the terminal](https://flutter.dev/docs/get-started/web).  
  
**TypeScript > KotlinJS > Dart.js on the web-perspective.**  
Well, even though I'm not experienced in the web development at all, I have tried Angular and I still think that no language is able to compile to JavaScript better then TypeScript.  
  
  
_Project overview: Just retreive some data from some API and make some List Rendering, usual things_  
  
**[The web version](https://coroutinedispatcher.github.io/seivom_web/#/): (recently changed to new design)**  
  

[![](https://1.bp.blogspot.com/-AhNpnrw3KM4/XYeQXXity-I/AAAAAAAAPZY/Xt8mCw1eFOoGKh82WeJ_Kjj5dh0SCNmcQCLcBGAsYHQ/s1600/Screenshot%2B2019-09-22%2Bat%2B17.15.55.png)](https://1.bp.blogspot.com/-AhNpnrw3KM4/XYeQXXity-I/AAAAAAAAPZY/Xt8mCw1eFOoGKh82WeJ_Kjj5dh0SCNmcQCLcBGAsYHQ/s1600/Screenshot%2B2019-09-22%2Bat%2B17.15.55.png)

  
**IOS:**  
  

[![](https://1.bp.blogspot.com/-HuiY-8ejZOY/XYeQhojytSI/AAAAAAAAPZc/rHO_4TzIcj0RSKONyaG9zcppo8UDvYzdQCLcBGAsYHQ/s1600/Screenshot%2B2019-09-22%2Bat%2B15.52.37.png)](https://1.bp.blogspot.com/-HuiY-8ejZOY/XYeQhojytSI/AAAAAAAAPZc/rHO_4TzIcj0RSKONyaG9zcppo8UDvYzdQCLcBGAsYHQ/s1600/Screenshot%2B2019-09-22%2Bat%2B15.52.37.png)

Since I own an Android device, I'm using XCode's Emulator

  
**Android:**  
  

[![](https://1.bp.blogspot.com/-geLDxokSo64/XYeQ1IVHqyI/AAAAAAAAPZo/iKHj2aFgh3Q-hkIli0kEOJMgqDKOokaggCLcBGAsYHQ/s400/viber_image_2019-09-22_17-17-42.jpg)](https://1.bp.blogspot.com/-geLDxokSo64/XYeQ1IVHqyI/AAAAAAAAPZo/iKHj2aFgh3Q-hkIli0kEOJMgqDKOokaggCLcBGAsYHQ/s1600/viber_image_2019-09-22_17-17-42.jpg)

Since I own an Android device, this is just a screenshot from it.

  
The performance is nearly the same.  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } Best of luck.  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; }