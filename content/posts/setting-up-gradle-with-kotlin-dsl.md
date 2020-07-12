---
title: 'Setting up Gradle with Kotlin DSL, a simple guide'
date: 2019-12-16T09:53:00.000+01:00
draft: false
aliases: [ "/2019/12/setting-up-gradle-with-kotlin-dsl.html" ]
tags : [Kotlin DSL, Gradle, Kotlin Delegation, Kotlin, Groovy, Migration, Android]
author: "Stavro Xhardha"
---

[![](https://static.zerochan.net/Kaiba.Seto.full.2339940.gif)](https://static.zerochan.net/Kaiba.Seto.full.2339940.gif)

  
Kotlin is a very a pretty nice adoptive language and user friendly. It really replaced Java from my everyday programming. However, it was not enough. We all know that groovy runs on JVM. So, why do I even need a new language just for my builds? Can't it be Java? So Java is the basic language for the JVM, Kotlin runs on JVM, Groovy runs on JVM and my build system has a separated language from my business logic system. Why?  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } I was introduced to Gradle with Kotlin accidentally. I never heard of Kotlin DSL in terms of Gradle. I just created a new Spring project and the built file looked kind of strange. After a little Google-ing, everything was clear. Long story short, I removed groovy from my Gradle build tool in my Android project, and replaced it with Kotlin. It shouldn't take more than 15 minutes to do this, but you can struggle with some things in particular.  
  
_Note: Kotlin DSL means Kotlin Domain Specific Language. It's just a notation, the name is self descriptive._  
  
 Here are some things to know:  
  
Start with the simplest thing ever: rename your settings.gradle to settings.gradle.kts. It should have less than 5 lines of code:  
  
is just going to be:  
  
Than stick with build.gradle project level. It's shorter (or it has repetitive things). So instead of having a build.gradle just rename the file to build.gradle.kts. There are 2 important things to note here (at least in my case).  
  
Global variables. If you need a variable which is going to be shared across modules and be kept in project level gradle, the ext.kotlin\_version = '1.3.61' should just be val kotlinVersion by rootProject.extra { "1.3.61" } and then you can access it by doing this: implementation("org.jetbrains.kotlin:kotlin-stdlib-jdk7:${rootProject.extra.get("kotlinVersion")}") in your app module.  
  
The custom _Clean Task,_ transforms from:  
  
  
to:  
  
  
Don't forget that Kotlin has no idea what ' ' are. Use " " instead. Also, the classpaths, are just becoming simple Kotlin methods with String parameters. So here is my full build.gradle.kts:  
  
So now let's jump to build.gradle module app. I could just paste the whole file (which I would do below), but there are some things to note. For example variables. I know that I it sounds a little stupid, but I never bothered writing variables in  groovy. It was better with just copy-pasting the version 🤦‍♂️. With Kotlin, it's a little more natural to write variables.  
  
Now the plugins transform:  
  
  
into:  
  
  
Note that when you have kotlin specific plugins you can just use kotlin() method, otherwise stick to id(). The defaultConfig becomes from:  
  
  
to:  
  
  
The buildTypes clause has also a new thing. Instead of:  
  
  
it becomes:  
  
A now a tricky little thing, KotlinOptions. You would need a getTask()  method to access it. So this:  
  
  
becomes:  
  
The rest is just the same. I'm just pasting the full build.gradle.kts (module app) whole file in case I forgot to mention something. There is also a nice guide [here](https://guides.gradle.org/migrating-build-logic-from-groovy-to-kotlin/) in case you have more unmentioned trouble around.  
  
  
**Conclusion**  
Unfortunately, I am still unable to tell the difference of the build speed because my project is still small. However, I might share it later on [my twitter](https://twitter.com/suspendfunction).  
  
Stavro Xhardha