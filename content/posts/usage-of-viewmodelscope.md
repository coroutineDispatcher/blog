---
title: 'Usage of the ViewModelScope'
date: 2019-06-19T10:37:00.000+02:00
draft: false
aliases: [ "/2019/06/usage-of-viewmodelscope.html" ]
author: "Stavro Xhardha"
---

Usage of the ViewModelScope \* { font-family: Georgia, Cambria, "Times New Roman", Times, serif; } html, body { margin: 0; padding: 0; } h1 { font-size: 50px; margin-bottom: 17px; color: #333; } h2 { font-size: 24px; line-height: 1.6; margin: 30px 0 0 0; margin-bottom: 18px; margin-top: 33px; color: #333; } h3 { font-size: 30px; margin: 10px 0 20px 0; color: #333; } header { width: 640px; margin: auto; } section { width: 640px; margin: auto; } section p { margin-bottom: 27px; font-size: 20px; line-height: 1.6; color: #333; } section img { max-width: 640px; } footer { padding: 0 20px; margin: 50px 0; text-align: center; font-size: 12px; } .aspectRatioPlaceholder { max-width: auto !important; max-height: auto !important; } .aspectRatioPlaceholder-fill { padding-bottom: 0 !important; } header, section\[data-field=subtitle\], section\[data-field=description\] { display: none; }

Based on my last blog post about easy implementation on Kotlin Coroutines,we were also introduced with the CoroutineContext .That was not…

* * *

![](https://cdn-images-1.medium.com/max/800/1*GH0V5XrkLOxZVlbDN2IS0Q.jpeg)

Based on my last [post](https://medium.com/@stavro96/kotlin-coroutines-the-non-confusing-one-5a47ca799578) about easy implementation on [Kotlin Coroutines](https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html), we were also introduced with the `CoroutineContext` .That was not too hard to understand, but I think there is a lot of code inside a `ViewModel` . First you needed a `Job()` , than you needed a `CoroutineScope` and in the instantiation you needed to specify one default `Dispatcher` . After that, you needed to keep track of the `CoroutineContext` corresponding to the `ViewModel`/`Activity`/`Fragment` lifecycle.

> Note: If you are not using a ViewModel skip this post entirely.

**Current code:**

This is not very long code. However, think of every ViewModel you will be having inside your project. So, you will need some refactoring,or not.  
This approach looks nice but has some unnecessary code.

Luckily there is another way of dealing with the `CoroutineContext` , thanks to Google. There is a lifecycle dependency which **provides and keeps track of** the `CoroutineContext` without declaring the `Job()` , the `CoroutineScope` and canceling on the `onCleared()` method.

**Introducing the ViewModelScope:**

Add this in your `build.gradle`(module app):

Now check how the ViewModel looks:

Of course, the `Dispatchers` need to be specified. Another thing we **must** be careful is that when we fire a coroutine tracked by the `viewModelScope` and we do not specify the thread it is going to run on the main thread. That’s because the `viewModelScope` runs on the `Dispatchers.Main` by default.

Happy ViewModelScoping.

By [Stavro Xhardha](https://medium.com/@stavro96) on [May 14, 2019](https://medium.com/p/f28703467b31).

[Canonical link](https://medium.com/@stavro96/usage-of-the-viewmodelscope-f28703467b31)

Exported from [Medium](https://medium.com) on June 19, 2019.