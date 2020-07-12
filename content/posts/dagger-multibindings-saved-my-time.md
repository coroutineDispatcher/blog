---
title: 'Dagger Multibindings saved my time'
date: 2019-08-08T14:03:00.000+02:00
draft: false
aliases: [ "/2019/08/dagger-multibindings-saved-my-time.html" ]
tags : [Dagger Multibindings, Dagger 2, Android Architecture Components, Kotlin, Android Development]
author: "Stavro Xhardha"
---

[![](https://1.bp.blogspot.com/-SBLyu47iAJ4/XUwNvy21LDI/AAAAAAAAOwQ/l8KVGYZMkiAVyLUgKHYy0h81nHVEjPECACLcBGAs/s1600/capturing-the-human-heart-TrhLCn1abMU-unsplash.jpg)](https://1.bp.blogspot.com/-SBLyu47iAJ4/XUwNvy21LDI/AAAAAAAAOwQ/l8KVGYZMkiAVyLUgKHYy0h81nHVEjPECACLcBGAs/s1600/capturing-the-human-heart-TrhLCn1abMU-unsplash.jpg)

  

  
While the Android architecture made development simpler, we should always be aware of the Lifecycle and with that, the right way to provide ViewModels. Creating one ViewModel factory for each Fragment and ViewModel you have is really expensive, bad architecture, and too much unnecessary code.  Not to mention the fact that you should care about scopes, dependencies, components modules when using Dagger for DI.  
  
You can have a simple "generic" ViewModel factory for all your app as a Singleton, with the help of Kotlin Reflection and Daggers Multibinding.  
  
What is provided here is a class, which has a Map in the constructor and extending the ViewModelProvider.Factory. On the only method it has, we are just going to provide our own instance of the ViewModel we need, which is the key of the map, and when that dependency is available we are going to require its particular dependencies which are "stored" in the value of that map. We can place as many ViewModels as we need, and this will be achieved by Dagger Multibinding. So it's just a Map<SomeViewModels, AndSomeViewModeldependencies>. The dependencies cannot just appear directly, so with the help of Provider<T> they arrive on the moment the ViewModel has just started. The @JvmSuppressWildcards is just making sure that the explicit Provider<ViewModel> will be on the Java generated files.  
  
To put what we need on that map, Dagger has a nice way of doing it. First, you need to create an annotation which will pass the key to the map:  
  
For example: To access HomeViewModel dependencies, we need a key, which happens to be the class itself. After that, what is needed is just to "put" those ViewModels in the map:  
  
Remember, we are in @Singleton scope (although in the example using a custom annotation, personal preference). So, where are the values (dependencies) for each key provided? They just appear when the ViewModel is instantiated. And that's amazing about how Dagger solves DI problem in the Java/Kotlin/Android world:  
  
And when injecting a ViewModel factory:  
  
Remember, all this work is not done just for a Repository, each ViewModel implementation might have different dependencies.  
  
**Conclusion:**  
  
Having tons of ViewModels in your Android app might make it too hard and expensive to produce them. When using Dagger Multibindings (if you are using dagger at all) it's a lot easier and prevents from writing bad code. Furthermore, if you want to add another Fragment and ViewModel, it's just like a game.  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; }