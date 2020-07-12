---
title: 'Room and coroutines testing'
date: 2019-10-14T10:00:00.000+02:00
draft: false
aliases: [ "/2019/10/room-and-coroutines-testing.html" ]
tags : [Coroutines, Android Room, Kotlin Coroutines, Concurrency, Coroutines Testing, RxJava, Kotlin, Suspending Methods, Room Persistence]
author: "Stavro Xhardha"
---

[![](https://1.bp.blogspot.com/-gA4cTTnwpNI/XaLzaqvjzsI/AAAAAAAAPuc/97mlWXrOHGcuQBNagNWrGK3f8hsn3fNqACLcBGAsYHQ/s1600/artur-tumasjan--ebp8HkKtrc-unsplash.jpg)](https://1.bp.blogspot.com/-gA4cTTnwpNI/XaLzaqvjzsI/AAAAAAAAPuc/97mlWXrOHGcuQBNagNWrGK3f8hsn3fNqACLcBGAsYHQ/s1600/artur-tumasjan--ebp8HkKtrc-unsplash.jpg)

  
My [last article](https://www.coroutinedispatcher.com/2019/10/room-and-rxjava-testing.html) covered some simple example about Room and RxJava instrumentation testing code. Coroutines also have [great support](https://github.com/Kotlin/kotlinx.coroutines/tree/master/kotlinx-coroutines-test) in _unit testing_ even though todays topic has nothing to do with it.  What I mean is that we are not going to cover runBlockingTest this time. Android doesn't support that (correct me if I'm wrong please) yet. However, I could schedule a topic about that because it really makes me excited. You can also check [this awesome talk](https://www.droidcon.com/media-detail?video=352671106) from Sean McQuillan, which I found pretty helpful.  
  
Comparing to the last gists, we will try to jump from Rx to coroutines (without implementation) and write test class about it.  Instead of RxJava components, we can just mark methods as suspended:  
  
This looks easier (or better say less confusing).  
  
_Two small notes that might be needed: _  
_Looking the code in the previous article, notice that you won't be needing the _InstantTaskExecutorRule() _and_ **suddenly** _we won't be running the queries on the Main thread. That's because we can't do that if methods are marked as_ suspend_ed._  
Let's start testing:  
  
Notice that we dropped the allowMainThreadQueries().  
  
And now the queries:  
  
Something too familiar? The key ingredient here is just a runBlocking keyword, which makes sure to run your suspending methods. I guess there is no need to add code for the update or delete part of testing. It's just super imperative and there is no secret here which could make you lose your mind (referring to the Rx-Javas blockingawait()) because a coroutine makes sure that the insertion is executed before the code below.  
  
  
**Not a small comparison:**  
  
I love both. But I think that Rx is really redundant when we are inside the Kotlin (especially coroutines) context.  
  
**Conclusion**  
  
Coroutines are being every day more supported by Google Android team and that's really great. New libraries are being written in Kotlin and coroutines are part of them. What I really like about Room and coroutines in testing is that I never _deliberately_ forget to test Daos on my project, I'm always ready to write tests (even though I haven't been around testing in more than 8 months).  
  
Note: If you want to know more about Room + coroutines, [here](https://medium.com/androiddevelopers/room-coroutines-422b786dc4c5) is a nice article from Florina Muntenescu also covering some deep dive and behind the scenes on how Room Coroutine support has been build.  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } Stavro Xhardha