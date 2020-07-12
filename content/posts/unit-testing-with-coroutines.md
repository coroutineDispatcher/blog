---
title: 'Unit testing with coroutines'
date: 2019-10-21T15:19:00.000+02:00
draft: false
aliases: [ "/2019/10/unit-testing-with-coroutines.html" ]
tags : [Kotlin Coroutines, Kotlin, Kotlin Coroutines Testing, Android, Unit testing]
author: "Stavro Xhardha"
---

[![](https://1.bp.blogspot.com/-7_lhiR915Ek/Xa2jYhwlyRI/AAAAAAAAQAc/8bJT8qOl7WA7TUdM4B20VPQOZ6p2TNuigCLcBGAsYHQ/s1600/adi-goldstein-B2sNfHkjagM-unsplash.jpg)](https://1.bp.blogspot.com/-7_lhiR915Ek/Xa2jYhwlyRI/AAAAAAAAQAc/8bJT8qOl7WA7TUdM4B20VPQOZ6p2TNuigCLcBGAsYHQ/s1600/adi-goldstein-B2sNfHkjagM-unsplash.jpg)

  
The coroutines API has already brought some innovation in the Android and Kotlin world. I always loved the idea of keeping it as simple as we all can. There is a saying around here that "Whoever talks to much, makes too much mistakes" and I see this a little bit related to Java's verbosity and also in the world of concurrency. It's said over and over again that concurrency is not simple and I couldn't agree more: You have to care about context, jobs running in parallel, cancelation, returning values etc.  
  
I hope I gave my best in one [of my previous articles](https://medium.com/@stavro96/kotlin-coroutines-the-non-confusing-one-5a47ca799578) explaining Kotlin Coroutines, therefore I will cover the testing tool of them today.  
  
As usual, some might still fear testing, but I really find it so entertaining. But there is no new concept to add to software testing when talking about coroutines, except just defining a "default" TestCoroutineDispatcher which is only a CoroutineDispatcher which runs immediate and lazy code the same way. In other words, a Test Double for CoroutineDispatcher (not sure if it is a Fake).  
  
So let's test. As explained we should be able to do it, once we have the dependency in our module:  
  
After that we only need to add some small configurations for our testing to start, as described above:  
  
_Note: The coroutines testing API is still experimental,  so in order not to annotate all variables and methods with @ExperimentalCoroutinesApi just annotate the class under test (should save you time)._  
  
After that, the only new thing around here is the runBlockingTest clause (i like to call lambdas this way but it's a method) which of course is an imitation of  the runBlocking. You should be able to pass the testDispatcher as an argument but it's not mandatory:  
  
And that's it.  
  
_Note: If you don't trust experimental API's yet, you are free to test with runBlocking even if it is not very much recommended that way (it could be a little redundant for this case). _  
**Conclusion**  
  
Since I already mentioned that concurrency is hard, and coroutines made it simple, there should be a nice tool to make testing them simple also. And the coroutines testing API is the answer. More about the dependency in the [documentation page](https://github.com/Kotlin/kotlinx.coroutines/tree/master/kotlinx-coroutines-test).  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } Stavro Xhardha.