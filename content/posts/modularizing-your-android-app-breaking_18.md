---
title: 'Modularizing your Android app, breaking the monolith (Part 3)'
date: 2019-11-18T08:57:00.000+01:00
draft: false
aliases: [ "/2019/11/modularizing-your-android-app-breaking_18.html" ]
tags : [Android Module, Micro Frontend, Dagger 2, Android Architecture Components, Android Multi Module]
author: "Stavro Xhardha"
---

[![](https://1.bp.blogspot.com/-GBV568I-KFA/XcwRO6JVtHI/AAAAAAAAQVI/nYR476_JLPMxX7GSTrd_GguoZKi2snKxQCLcBGAsYHQ/s1600/florian-van-duyn-zE6ivbPzPGU-unsplash.jpg)](https://1.bp.blogspot.com/-GBV568I-KFA/XcwRO6JVtHI/AAAAAAAAQVI/nYR476_JLPMxX7GSTrd_GguoZKi2snKxQCLcBGAsYHQ/s1600/florian-van-duyn-zE6ivbPzPGU-unsplash.jpg)

  
On our [latest article](https://www.coroutinedispatcher.com/2019/11/modularizing-your-android-app-breaking_11.html), what we did was creating a Dagger component about almost all of our features and provided a ViewModel(Factory) for every Fragment we had inside a module. As said, this is a little too much work for Dagger, the programmer and each feature. It's a total overkill of using Dagger actually, we could have stuck to a manual DI instead, but I required you to be patient.  
  
Let me remind you of the current state of the app:  
  

[![](https://1.bp.blogspot.com/-a-sO59Lwxuo/XcwZC1uh7RI/AAAAAAAAQVg/mp8LCb606mwT452LTpcEsgB7jmhTUP-3wCLcBGAsYHQ/s1600/Part3Uncrashed%2B%25281%2529.jpg)](https://1.bp.blogspot.com/-a-sO59Lwxuo/XcwZC1uh7RI/AAAAAAAAQVg/mp8LCb606mwT452LTpcEsgB7jmhTUP-3wCLcBGAsYHQ/s1600/Part3Uncrashed%2B%25281%2529.jpg)

  
_For simplicity I'm not saying in the picture that each component brings a ViewModel(Factory) to the component, please check the previous article if you are confused._  
  
**The problem:**  
  
I am using a database only in 2 modules, :feature\_2 and :feature\_4. The rest of the app doesn't care about the database at all. Furthermore, :feature\_2 has 0 relations to my :feature\_4 entities, while this last one has 2 tables related to each other ( a one to many relationship). But this is not the problem yet. The problem is related to the database being exposed for all my modules. That's really unnecessary.  
  
**An optional solution:**  
  
We could create a :db\_module and install it as a module inside the features that actually need it:  
  

[![](https://1.bp.blogspot.com/-0Nc6MuocZOk/XcwZt_g2EnI/AAAAAAAAQVo/aDQOaa9QRdYjJtImzVW-Uk3z2xo5cgGgwCLcBGAsYHQ/s1600/Part3Uncrashed%2B%25282%2529.jpg)](https://1.bp.blogspot.com/-0Nc6MuocZOk/XcwZt_g2EnI/AAAAAAAAQVo/aDQOaa9QRdYjJtImzVW-Uk3z2xo5cgGgwCLcBGAsYHQ/s1600/Part3Uncrashed%2B%25282%2529.jpg)

  
This solution would be great for modules whose entities have relations to each other. In my case it's too much. IMO, it's better to separate databases completely. I'll keep one database for my :feature\_4 which has 2 related entities and another database for my :feature\_2 which has only one entity. Plus, it requires less work in terms of time (even if we are creating 2 databases, isn't that cool😋 )  
  
**The solution:**  
  
Completely separate databases:  
  

[![](https://1.bp.blogspot.com/-O4FfVxF50Sc/XcwbedWRPVI/AAAAAAAAQV0/fZg-mKVAXlsi_haxQSKLBg_RAe_zEk_ogCLcBGAsYHQ/s1600/Part3Uncrashed%2B%25283%2529.jpg)](https://1.bp.blogspot.com/-O4FfVxF50Sc/XcwbedWRPVI/AAAAAAAAQV0/fZg-mKVAXlsi_haxQSKLBg_RAe_zEk_ogCLcBGAsYHQ/s1600/Part3Uncrashed%2B%25283%2529.jpg)

  
In this way, the :feature\_4 and :feature\_2 not only are separated from all the other modules but they are also separated from each other.  
  
**Some code:**  
We still have to do only dependency configuration only. This is good news:  
  
This way I'm able to get the Application instance from my :core\_module and provide it for my local database. And then I can do:  
  
Now, the same rule should apply for the other module which needs the database configuration.  
  
**_Can we do more?_**  
  
Sure! The API calls are still centralized. What I mean is that methods of every API call are still exposed to all the modules. Furthermore, I am not using the same base URL. So I guess we should apply what we did to our Retrofit Interface too. Instead of having one interface with a lot of methods, we can have a lot of interfaces with the respective methods for the API call. I won't stop in the implementation for this case, but I guess the below image would give the message:  
  

[![](https://1.bp.blogspot.com/-ev0m_pg21sA/XcwjqsTF5II/AAAAAAAAQWA/pUBvMtKpb2QfsR2XZpJ6SZMn890ZfvgwwCLcBGAsYHQ/s1600/Part3Uncrashed%2B%25284%2529.jpg)](https://1.bp.blogspot.com/-ev0m_pg21sA/XcwjqsTF5II/AAAAAAAAQWA/pUBvMtKpb2QfsR2XZpJ6SZMn890ZfvgwwCLcBGAsYHQ/s1600/Part3Uncrashed%2B%25284%2529.jpg)

  
  
The same logic as the database configuration appeals to the retrofit modularization in code also:  
  
  
**Conclusion**  
There you go, now your modules are totally independent from each-other and our multi module app is 90% ready. The last part will be covering some short topics about my background-things (AlarmManager, WorkManager) and mostly resources, layouts and values.  
  
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css'); @import url('https://fonts.googleapis.com/css?family=Open+Sans'); body { margin: 20px; font: 16px 'Open Sans', sans-serif; } Stavro Xhardha