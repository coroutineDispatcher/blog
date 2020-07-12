---
title: 'How to produce the SavedStateHandle in your ViewModel using AssistedInject'
date: 2019-08-27T10:31:00.003+02:00
draft: false
aliases: [ "/2019/08/how-to-produce-savedstatehandle-in-your.html" ]
tags : [Dagger 2, SavedStateHandle, ViewModel Factory, ViewModel, AssistedInject, Kotlin Delegation, Kotlin]
author: "Stavro Xhardha"
---

[![](https://1.bp.blogspot.com/-MtcO8qu_Ato/XWTqNfbpINI/AAAAAAAAPAk/-cKfHokXF-A_Sg2jwDZwHWrsg9gqZGTzgCLcBGAs/s1600/karsten-wurth-karsten-wuerth-0w-uTa0Xz7w-unsplash.jpg)](https://1.bp.blogspot.com/-MtcO8qu_Ato/XWTqNfbpINI/AAAAAAAAPAk/-cKfHokXF-A_Sg2jwDZwHWrsg9gqZGTzgCLcBGAs/s1600/karsten-wurth-karsten-wuerth-0w-uTa0Xz7w-unsplash.jpg)

  
One of my previous article [Dagger Multibinding Saved My Time](https://coroutinedispatcher.blogspot.com/2019/08/dagger-multibindings-saved-my-time.html) "claimed" to have found the right practice for providing ViewModels without producing a ViewModel Factory for each ViewModel. Apparently, [I was wrong](https://www.reddit.com/r/androiddev/comments/cnktpg/dagger_multibindings_saved_my_time/).  There are 2 main problems with that approach:  
  
1 - I might forget to add a ViewModel in the Map graph.  
2 - With the new SavedStateHandle which stays uniqely in each ViewModel I can't use a generic ViewModel Factory.  (you can check the implementation in the article provided above).  
  
So,  what does [AssistedInject](https://github.com/square/AssistedInject) do to help solve this case?  
  
Check this example below:  
  
  
Dagger doesn't know how to create  a SavedStateHandle. And since the SavedStateHandle cannot be used as a Singleton, we want a different state for each of our fragments. In order to achieve this, let's allow the AssistedInject library create a unique Factory for us. It might require a little more work on configurations, but after the set up, it's just kindergarten game for you.  
  
Instead of  annotating constructor with @Inject , we annotate it with @AssistedInject:  
  
  
The constructor parameters which cannot be evaluated by Dagger, should be marked with @Assisted. Now, we should create the Factory. With AssistedInject it's pretty easy. You just define an interface and annotate it with the @AssistedInject.Factory annotation and it is going to have a method with parameters that Dagger doesn't know how to create, and will return the current ViewModel:  
  
  
Perhaps someone would ask why not having different scopes for each ViewModel in order to provide that unknown parameter uniquely in each ViewModel? In that case, you will have a component for each fragment, a module and you should not forget to perform DI in all creation state of each fragment. So the short answer is: Don't do it.  
  
One more step. We need to somehow connect the strings here. How does Dagger provide dependencies that don't know how to create? It will ask the component if there is any of the modules providing the dependencies. But there isn't any module, or is there 👀 ? Yes, the AssistedInject has another small step to conclude:  
  
  
In this way, the our ViewModel Factories will be auto generated. We just have to call them from the Singleton component:  
  
  
That's it.  
  
Since the SavedStateViewModel Factory is now the default ViewModel Factory, to provide a SavedStateHandle, we can't extend our ViewModelProvider.Factory. Instead, we need to extend our AbstractSavedStateViewModelFactory:  
  
  
And with the help of Kotlins' delegation, we achieve something like this in our fragment:  
  
  
And that's it.  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } Credits:  
[Zhuinden](https://twitter.com/Zhuinden/status/1159442420855164929)  
[It's complicated, but it doesn't have to be: a Dagger journey by Fred Porciúncula, Blinkist EN](https://www.youtube.com/watch?v=9fn5s8_CYJI)