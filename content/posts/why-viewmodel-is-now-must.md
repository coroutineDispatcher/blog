---
title: 'Why the ViewModel is now a must'
date: 2019-06-25T12:53:00.000+02:00
draft: false
aliases: [ "/2019/06/why-viewmodel-is-now-must.html" ]
tags : [Coroutines, Android Architecture Components, ViewModel, Android Development]
author: "Stavro Xhardha"
---

[![](https://1.bp.blogspot.com/-_ig9ZjauT1Q/XRH9RpHZiCI/AAAAAAAAOI0/56-SUoVpyNowwud40mJGvAaAsdDm9EbMwCLcBGAs/s1600/mel-elias-0RanOcSEpAo-unsplash.jpg)](https://1.bp.blogspot.com/-_ig9ZjauT1Q/XRH9RpHZiCI/AAAAAAAAOI0/56-SUoVpyNowwud40mJGvAaAsdDm9EbMwCLcBGAs/s1600/mel-elias-0RanOcSEpAo-unsplash.jpg)

  
Inspired by Lyla Fujiwars latest post, [ViewModels with Saved State, Jetpack Navigation, Data Binding and Coroutines](https://medium.com/androiddevelopers/viewmodels-with-saved-state-jetpack-navigation-data-binding-and-coroutines-df476b78144e), I decided to list some of the reasons why the ViewModel is strongly suggested to be used on the Android app.  
  
  
**1\. Views are independent**  
The ViewModel does fully respect the Single Responsibility Principle, leaving the Activity/Fragmentdo its thing. When the ViewModel is included,  views are passive and just wait for changes without actually doing anything. This is done through LiveData or RxJava, which provide observable fields:   
  
  
The MutableLiveData is the variable we are prepared that it might take changes of the value asigned during the flow. And giving its value to the live data means that whatever happens that variable value LiveData is going to observe it.   
After that, the Fragment/Activity just waits for that variable:   
  
If you notice, the view does nothing except handles the child views.  
  
**2\. Presenter destroyed**  
I don't want to be rude with people using the MVP architecture, but I think the ViewModelremoves lots of boilerplate code as well as the view reference from the presenter. MVP done wrong could just lead us to complex architecture apps. However, the Android team strongly suggests using the presenter behind the ViewModel if you have lots of business logic to deal with and infinite number of if and else blocks.   
  
**3\. onSavedInstanceState is not a pain anymore**  
If you check the article on the introduction, the latest version of Android Architecture components has brought a nice support over the onSavedInstanceState. You will now deal with it on the ViewModel, and only instantiating it on the Activity/Fragment  
  
_Note, below gists not original, brought from the article above_  
  
And now a simple case for restoring some userId:  
  
That's simply because the ViewModeldeserved more to deal with the onSavedInstanceState than the views, plus it has a different lifecycle which makes it survive the configuration change.  
  
**4\. Can be used to share data between fragments**  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } And I love it lot. ViewModels can be easy integrated to share data between fragments, removing once and for all the broken telephone way of EventBuses, perhaps static variables or some unnecessary SharedPreferences calls.  
  
**5\. Easy integration with Coroutines**  
  
And my favorite part. The Android team has introduced us with an extension function for the ViewModelcalled viewModelScope. What is does is basically gives a CoroutineContext that handles lifecycle without need to override the onCleared method to cancel the job. That way, you don't have to be scared of memory leaks and perform coroutine actions easily. For more information, please head over to my other post about Usage of the [ViewModelScope](https://proandroiddev.com/usage-of-the-viewmodelscope-f28703467b31) .  
  
**Conclusion**  
In my own opinion, the ViewModel has been the missing peace which corrected lots of admitted mistakes by the Android team. Android apps without a ViewModel don't suck, they are all awesome and each developer needs to be congratulated for building apps, but the ViewModel makes life easier. There are lots of other reasons, and really important ones. For example the Databinding, but their logic can be achieved even without ViewModel also, that's why I haven't included it in the post.