---
title: 'Retrofit met Coroutines'
date: 2019-06-19T10:42:00.002+02:00
draft: false
aliases: [ "/2019/06/retrofit-met-coroutines.html" ]
author: "Stavro Xhardha"
---

Retrofit met Coroutines \* { font-family: Georgia, Cambria, "Times New Roman", Times, serif; } html, body { margin: 0; padding: 0; } h1 { font-size: 50px; margin-bottom: 17px; color: #333; } h2 { font-size: 24px; line-height: 1.6; margin: 30px 0 0 0; margin-bottom: 18px; margin-top: 33px; color: #333; } h3 { font-size: 30px; margin: 10px 0 20px 0; color: #333; } header { width: 640px; margin: auto; } section { width: 640px; margin: auto; } section p { margin-bottom: 27px; font-size: 20px; line-height: 1.6; color: #333; } section img { max-width: 640px; } footer { padding: 0 20px; margin: 50px 0; text-align: center; font-size: 12px; } .aspectRatioPlaceholder { max-width: auto !important; max-height: auto !important; } .aspectRatioPlaceholder-fill { padding-bottom: 0 !important; } header, section\[data-field=subtitle\], section\[data-field=description\] { display: none; }

Finally the latest version of Retrofit (2.6.0) has got out. While it was already really easy to use and so much fun, Retrofit is now easy…

* * *

![](https://cdn-images-1.medium.com/max/800/1*6zsDPmdnSm_VA0sUPj4Aww.jpeg)

Finally the latest version of `Retrofit` (2.6.0) has got out. While it was already really easy to use and so much fun, `Retrofit` is now easy, fun and shorter to write. Let’s deal with a refactor scenario from the previous version:

**Version 2.5.0:**

Obviously we have to return the same type until we `await()` for that response:

Notice we are returning the `Deferred`type which holds our response. We won’t stop on what a `Deffered` is apart from sayin that it’s just a [Future](https://en.wikipedia.org/wiki/Futures_and_promises).

And when we want to get that response:

Notice we are calling the `await()` method in order to wait until our asynchronous operation has finished.

Well, no need for that anymore!

**Version 2.6.0:**

Drop the `Deferred` type and mark your methods as `suspend` . Again we wont stop on what `suspend` methods are apart from saying that it is a way to tell my program that this method is going to be a little late.

And after that, do the same until you await the response:

Now let’s implement the method:

Notice there is no need to `await()` that method because retrofit does that for you. It will return your defined type.

> Note: You should also drop the `.addCallAdapterFactory(CoroutineCallAdapterFactory())` from your `Retrofit` instance build.

> I am using `GlobalScope` because I am not inside a Activity/Fragment/ViewModel which would have theis own lifecycle. Please avoid the `GlobalScope` . For just an example it’s fine.

**Source:**

[**_Migrate to Retrofit 2.6.0 and built-in suspend support_ by JakeWharton · Pull Request #170 ·…**  
Migrate to Retrofit 2.6.0 and built-in suspend supportgithub.com](https://github.com/JakeWharton/SdkSearch/pull/170/files "https://github.com/JakeWharton/SdkSearch/pull/170/files")[](https://github.com/JakeWharton/SdkSearch/pull/170/files)

Good luck.

By [Stavro Xhardha](https://medium.com/@stavro96) on [June 6, 2019](https://medium.com/p/7bbe7e86825a).

[Canonical link](https://medium.com/@stavro96/retrofit-met-coroutines-7bbe7e86825a)

Exported from [Medium](https://medium.com) on June 19, 2019.