---
title: 'Kotlin Coroutines, the non-confusing one '
date: 2019-06-19T10:31:00.003+02:00
draft: false
aliases: [ "/2019/06/kotlin-coroutines-non-confusing-one.html" ]
author: "Stavro Xhardha"
---

Kotlin Coroutines, the non-confusing one . \* { font-family: Georgia, Cambria, "Times New Roman", Times, serif; } html, body { margin: 0; padding: 0; } h1 { font-size: 50px; margin-bottom: 17px; color: #333; } h2 { font-size: 24px; line-height: 1.6; margin: 30px 0 0 0; margin-bottom: 18px; margin-top: 33px; color: #333; } h3 { font-size: 30px; margin: 10px 0 20px 0; color: #333; } header { width: 640px; margin: auto; } section { width: 640px; margin: auto; } section p { margin-bottom: 27px; font-size: 20px; line-height: 1.6; color: #333; } section img { max-width: 640px; } footer { padding: 0 20px; margin: 50px 0; text-align: center; font-size: 12px; } .aspectRatioPlaceholder { max-width: auto !important; max-height: auto !important; } .aspectRatioPlaceholder-fill { padding-bottom: 0 !important; } header, section\[data-field=subtitle\], section\[data-field=description\] { display: none; }

Being inspired from my first lame post about coroutines, this time I decided to get real focus on the coroutines, rather than the setup of…

* * *

Being inspired from my [first lame post about coroutines](https://medium.com/@stavro96/kotlin-coroutines-save-the-day-cf2745a4e30a), this time I decided to get real focus on the coroutines, rather than the setup of Dagger or the ViewModel before the work.

![](https://cdn-images-1.medium.com/max/800/1*waErc-N_YswDkPMCyVcP2Q.png)

**Definition:**

Coroutines are lightweight threads. What this means is that coroutines do not create a real new thread in your processor. These threads are just simple objects which imitate the way of how threads work. Coroutines are created to make asynchronous programming simple.

> Note: This posts use cases are in the Android framework.

**Setup:**

Add the dependency to your `build.gradle` module app file :

```
implementation ‘org.jetbrains.kotlinx:kotlinx-coroutines-core:1.2.1’
```

**Need to know:**

To start a coroutine, you need a `CoroutineContext.`For that, you will need a `Job` and a `CoroutineScope`. A `Job` is nothing more than a way, so you can keep track of what coroutine is doing. Also the job makes working simple because it handles the coroutines according to the `Activity`/`Fragment` lifecycle (or the `ViewModel`).You can cancel this job when `Activity`/`Fragment` has finished.The `CoroutineScope` is a combination between the `Job()`and the `Dispatchers.*`, which is telling the framework what thread the job is going to be executed. If you do not declare a `Job()` the `CoroutineScope` will provide it for you. However, you cannot keep track of that job depending on the lifecycle .

**The use case:**

I need to get some data from the server via Json, the server will give me some values which I will save to the `SharedPreferences`. After that I am going to update the UI with those saved values.

**Details:**

Best practice is to use coroutines inside the ViewModel. But if you don’t what to use it, initialise in the Activity/Fragment class.

The `CoroutineScope` is being used in the IO thread. The IO thread is any thread but the Main thread. I could have started the `CoroutineScope` on the Main thread, but this is not my use case. My primary work is on the network than in the UI, thus there is pretty logical to me to start the coroutine on the IO thread. The plus operator there just defines the threading policy between the scope and the job. Together they make the `CoroutineScope`. The `Dispatchers` class has 4 types.

*   **Default**: _The Default type is used when no_ `_CoroutineContext_` _has been specified._
*   **Main**: _Refers to the main thread, where the UI lies._
*   **Unconfirmed**: _A coroutine dispatcher that is not confined to any specific thread._
*   **IO**: _Designed for offloading blocking IO tasks to share pool of threads._

> Note: You must finish the job when the `Activity`/`Fragment`/`ViewModel` has finished. In my case:

**Work:**

First of all I will need to specify the network call using retrofit:

Nothing new here apart from the `Deferred` type. The `Deferred` type is nothing but a class telling the software that what it provides is going to be delayed. Logic says: don’t run this on the main thread .

> Note: I am not providing the retrofit setup.

> Also note: As for the `Deferred` approach on Retrofit, it won’t be too long in this way. Please refer [this](https://zsmb.co/retrofit-meets-coroutines/) link if you want to know more.

A method inside a coroutine, except from the methods that run on the main thread, must be marked with the `suspend` keyword, or on this case should return a `Deferred` type, if they return something that delays.

**Implementation:**

The `loadPrayerTimes()` is the method where the coroutine starts. If a condition is met, I start making the api call, else I need to tell user something (on the main thread).The `withContext` is basically the way to change the `CoroutineContex`. Now let’s jump to the `makePrayersApiCall()` method. It is marked with a `suspend` keyword as mentioned before. Everything that runs inside this method, is running on the IO thread. Why? Because that is the scope where I started in the initialisation. Basically, I update the UI on the beginning (where I load some progress bar) and in the end (where I show the data or I show error). One last thing we have not yet mentioned is the `.await()` method, which basically makes my request wait, until it gets the response but not by blocking the UI . The execution continues asynchronously.

**More to know:**

If you want to use kotlin coroutines only in the ViewModel, Google provides some implementation described [here](https://medium.com/@stavro96/usage-of-the-viewmodelscope-f28703467b31).

**Conclusion:**

Using the kotlin coroutines is fun, simple and short. I personally found hard time using RxJava (which is awesome, but hard) and also Java’s AsyncTask.   
Coroutines made me understand the asynchronous programming more than I could imagine, and now they are helping me build a full [project](https://github.com/stavro96/pocket_treasure) using them.

**Credits**:

#### [KotlinConf 2018 — Kotlin Coroutines in Practice by Roman Elizarov](https://www.youtube.com/watch?v=a3agLJQ6vt8&t=3s)

#### [Droidcon NYC 2018](https://www.youtube.com/watch?v=lh2Vqt4DpHU&t=1904s) — Coroutines by Example

By [Stavro Xhardha](https://medium.com/@stavro96) on [May 10, 2019](https://medium.com/p/5a47ca799578).

[Canonical link](https://medium.com/@stavro96/kotlin-coroutines-the-non-confusing-one-5a47ca799578)

Exported from [Medium](https://medium.com) on June 19, 2019.