---
title: 'Manage network states with the Paging library'
date: 2019-07-31T16:22:00.000+02:00
draft: false
aliases: [ "/2019/07/manage-network-states-with-paging.html" ]
tags : [Android Paging Library, Android Architecture Components, Kotlin Coroutines, Kotlin, Android Development]
author: "Stavro Xhardha"
---

[![](https://1.bp.blogspot.com/-mJSoCFMzV-8/XUGWcPlTvqI/AAAAAAAAOm8/aJfx9TOsU1sdCTmQh3OOIEEmRmoUSD2gQCLcBGAs/s1600/anastasia-zhenina-DzqkDffa00c-unsplash.jpg)](https://1.bp.blogspot.com/-mJSoCFMzV-8/XUGWcPlTvqI/AAAAAAAAOm8/aJfx9TOsU1sdCTmQh3OOIEEmRmoUSD2gQCLcBGAs/s1600/anastasia-zhenina-DzqkDffa00c-unsplash.jpg)

  
With Room implementation, the Paging Library is not such a big deal. However it needs some small configurations when it comes to using it for API-calls.  
  
One thing to keep in mind, is the initial state, which basically is a different task, which might fail with an Exception. And the same logic is followed by the continues state after the first data are loaded. When using coroutines, we might wanna have an extra problem: managing the CoroutineScope inside a DataSource.  
  
**Implementation**  
With the help of Kotlin data classes, we might be able to manage Paging Library state:  
  
After the basic setup is made, we might want to make some setup on our PagingDataSource, which on my case, is of type PagedKeyDataSource:  
  
Notice the initial loading and the after loading need different fields for same network state of theirs.  
After that, we should be able to produce the DataSourceFactory observing our PagingDataSource with LiveData<NewsDataSource>. The main goal of this is to centralize what the PagingDataSource produces and later observe it in the ViewModel:  
  
What remains is to implement everything on the NewsViewModel, which would bring the code to something like this:  
  
After that, with the help of DataBinding, or just observing three LiveDatas in Fragment, all is set.  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } Helpful resource:  
[Paging with network sample](https://github.com/googlesamples/android-architecture-components/tree/master/PagingWithNetworkSample)