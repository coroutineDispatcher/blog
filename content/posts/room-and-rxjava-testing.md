---
title: 'Room and RxJava testing'
date: 2019-10-07T09:30:00.000+02:00
draft: false
aliases: [ "/2019/10/room-and-rxjava-testing.html" ]
tags : [Android Room, RxJava, Kotlin, Android Architecture Components., Room Persistence]
author: "Stavro Xhardha"
---

[![](https://1.bp.blogspot.com/-r06lu3whUTI/XZo6xhLlVmI/AAAAAAAAPl4/1vngQcfkBwUJksX1ug8hxAT1XjtY6oshgCLcBGAsYHQ/s1600/anand-thakur-l2x4FyIi0tI-unsplash.jpg)](https://1.bp.blogspot.com/-r06lu3whUTI/XZo6xhLlVmI/AAAAAAAAPl4/1vngQcfkBwUJksX1ug8hxAT1XjtY6oshgCLcBGAsYHQ/s1600/anand-thakur-l2x4FyIi0tI-unsplash.jpg)

  
&nbps; When it comes to testing the data layer, we should always have time for that. It's very important. You lose data, you lose users. Since I really found testing + Room pretty amusing, I thought I should share it with you.  
  
Room has already a great support for RxJava or Coroutines. I have used both ways to access the data layer and I was really satisfied with both. So I decided to make a 2-series blog posts with testing in Room with RxJava and testing Room with coroutines.  
  
_This is not a Rx-Coroutines comparison. _  
  
I will start with Rx first. First of all this is the dependency for Rx support:  
  
    androidTestImplementation “android.arch.core:core-testing:1.0.0-alpha5”  
  
 So let's suppose we have this DAO:  
  
I will skip the implementation of TypeConverters, but the important part is to notice that Room supports Date() objects as part of table column. Notice also that the annotation @VisibleForTesting is a pretty cool one. If I use that method outside my testing environment, the IDE will yell at me.  
  
_Some things to keep in mind: Dao methods marked with @Query actually return a value. It could be a void or an integer holding the number of affected rows._  
  
Notice that all my queries are returning some RxJava components. In case of @Insert or @Update or  @Delete you could also use a Single<Long> or a Maybe<Long> (where Long could be the datatype of the affected id, if you marked it as String or Int, put int in there). I just return a Completable. In my case I don't need the id of the affected row.  
  
So, what to test? Well usually, the main parts are insertions, deletions or updating values. The selection queries can be seen as tools to verify the above operations, but that doesn't mean you can't individually test each selection, or change it with another one (must note that should insert some data first anw).  
  
_Note: This kind of tests require a device because they are instrumental tests._  
Now the tests. The first thing to keep in mind is that we don't really want to use our real database because that would be a total disaster. So, Room provides an _in-memory_ database helper for you to run your tests:  
  
Since we are done with the configuration. First let's test the insertion:  
  
Notice that we should add the blockingAwait() method behind the completable execution otherwise the selection might execute before insertion. And that's the only tricky part for testing Room queries with RxJava. Same logic applies to Update, Delete.  
  
Check how RxJava has some awesome support for asserting values only by adding the test() method behind.  
  
And the delete part:  
  
**Conclusion**  
Async operations are pretty hard as a concept. And I really think Rx is one of the best tools that has already given a good problem solving about it.  
The one and only thing I don't really like about Google is that they suddenly dropped support for Rx. They are developing infinitely for coroutines and believe me, that's really great, but for example the viewModelScope should have had a viewModelCompositeDisposable as well (perhaps unrelated to current topic but really, that's what I think and this is my only article where Rxjava is included. I'm more "coroutine-oriented").  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } Stavro Xhardha