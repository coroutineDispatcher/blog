---
title: 'Modularizing your Android app, breaking the monolith (Part 1)'
date: 2019-11-04T09:30:00.000+01:00
draft: false
aliases: [ "/2019/11/modularizing-your-android-app-breaking.html" ]
tags : [Android Module, Micro Frontend, Android Architecture Components, Android Multi Module]
author: "Stavro Xhardha"
---

[![](https://1.bp.blogspot.com/-t-k6QGxwi4c/XbyQY_hu1FI/AAAAAAAAQK0/5j6A8G0OoXU0CkHTLBtSn_EsVVhhjgixwCLcBGAsYHQ/s1600/kane-reinholdtsen-kNo6H-MDTzA-unsplash.jpg)](https://1.bp.blogspot.com/-t-k6QGxwi4c/XbyQY_hu1FI/AAAAAAAAQK0/5j6A8G0OoXU0CkHTLBtSn_EsVVhhjgixwCLcBGAsYHQ/s1600/kane-reinholdtsen-kNo6H-MDTzA-unsplash.jpg)

  
Inspired by a Martin Fowlers post about [Micro Frontends](https://martinfowler.com/articles/micro-frontends.html), I decided to break my monolithic app into a modular app. I tried to read a little more about breaking monolithic apps in Android, and as far as I got, I felt confident to share my experience with you. This will be some series of blog posts where we actually try to break a simple app into a modularized Android app.  
  
_Note: You should know that I am no expert in this, so if there are false statements or mistakes please feel free to criticize, for the sake of a better development. _  
  
**What do you benefit from this approach:**  

*   Well, people are moving pretty fast nowadays and delivery is required faster and faster. So, in order to achieve this, modularising Android apps is really necessary.
*   You can share features across different apps.
*    Independent teams and less problems per each.
*   Conditional features update.
*   Quicker debugging and fixing.
*   A feature delay doesn't delay the whole app.

As per writing tests, there is not too much difference about being in a monolith or having a modularized Android app. So we will skip tests on this series. Just make sure that each test stands in the right module which corresponds to the chosen feature.  
 Now, there is a small benefit if you are thinking Android specifically:  

*   Significantly reduces APK size. Which means more installs (according to some 🌚)

  
** A small introduction:**  
The app actually is pretty simple. Is built with Architecture Components and has only one Activity. Each fragment has some dependencies but they are Singletons, like database reference, or Picasso and the Retrofit API interface.  
  
So basically, this is my application schema:  
  

[![](https://1.bp.blogspot.com/-EsLKUeP8Bdk/Xb2Z-XmrmeI/AAAAAAAAQLU/9C7pqG6QoxMns3z1ZDxGQDOIzQYeZ9DEACLcBGAsYHQ/s1600/Multi%2BModule%2BRefactoring%2BDiagram.jpg)](https://1.bp.blogspot.com/-EsLKUeP8Bdk/Xb2Z-XmrmeI/AAAAAAAAQLU/9C7pqG6QoxMns3z1ZDxGQDOIzQYeZ9DEACLcBGAsYHQ/s1600/Multi%2BModule%2BRefactoring%2BDiagram.jpg)

  
All my fragments are pulling dependencies from my Application level. Except from one of them. That fragment is totally unrelated to the other part of the app. It just shows some hardcoded values in the RecyclerView. That really looks like a nice way to start.  
  
_Note: I also have an AlarmManager and 2 WorkManagers both pulling dependencies from the application layer, which will be covered later._  
  
But first things first, let's set up gradle for a multi module project. What I mean is that there is no need to reimplement dependencies over and over again when I add a new Android module for some new feature. Instead, we can just put all of our external libraries and dependencies inside the project level gradle and let all the other modules gradle inherit from it. That would bring better management when library updates occur. A smart thing I found [in this YouTube video](https://www.youtube.com/watch?v=TWLkswxjSr0&t=1902s):  
  
This would keep all your build.gradle files super clean. Now you don't have to require a new dependency and manage the updates because you deal with it only once. And this is how you call them after:  
  
  
  
_Note: You must apply plugin : 'kotlin-kapt' when creating a new module._  
  
Basically, this is all what my new module needs. And now let's break something.  
  
Create a new Android Module and just move all your new features classes over there. Don't forget layouts, strings, dimens, drawable and all resources that are unrelated to other fragments. Get them too. For the current case, there will be no errors because I inherit nothing from any of my app components.  
  
All I have to do now, is just apply this feature to my app/build.gradle level.  
  
_Note: The nav graph should break because moving fragment outside the module would bring to unresolved element, but not to worry, we will fix this right now:_  
  
Done. The application schema now would look like this:  
  

[![](https://1.bp.blogspot.com/-SKJ-0_sgYNU/Xb6p2JsjOZI/AAAAAAAAQL4/A8bjTdwbgnsqqgPhk1QqUqlffFFzYsLfACLcBGAsYHQ/s1600/Multi%2BModule%2BRefactoring%2BDiagram%2B%25281%2529.jpg)](https://1.bp.blogspot.com/-SKJ-0_sgYNU/Xb6p2JsjOZI/AAAAAAAAQL4/A8bjTdwbgnsqqgPhk1QqUqlffFFzYsLfACLcBGAsYHQ/s1600/Multi%2BModule%2BRefactoring%2BDiagram%2B%25281%2529.jpg)

  
Now, my feature\_6\_module is installed as an "external library" to my app module.  
  
**Conclusion**  
  
This was part 1 of breaking a monolithic app into a modularized app in Android. Next part will be all about Dagger and core dependencies.  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } Stavro Xhardha