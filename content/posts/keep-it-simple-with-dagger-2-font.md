---
title: 'Keep it simple with Dagger 2'
date: 2019-06-19T10:16:00.001+02:00
draft: false
aliases: [ "/2019/06/keep-it-simple-with-dagger-2-font.html" ]
author: "Stavro Xhardha"
---

Keep it simple with Dagger 2 \* { font-family: Georgia, Cambria, "Times New Roman", Times, serif; } html, body { margin: 0; padding: 0; } h1 { font-size: 50px; margin-bottom: 17px; color: #333; } h2 { font-size: 24px; line-height: 1.6; margin: 30px 0 0 0; margin-bottom: 18px; margin-top: 33px; color: #333; } h3 { font-size: 30px; margin: 10px 0 20px 0; color: #333; } header { width: 640px; margin: auto; } section { width: 640px; margin: auto; } section p { margin-bottom: 27px; font-size: 20px; line-height: 1.6; color: #333; } section img { max-width: 640px; } footer { padding: 0 20px; margin: 50px 0; text-align: center; font-size: 12px; } .aspectRatioPlaceholder { max-width: auto !important; max-height: auto !important; } .aspectRatioPlaceholder-fill { padding-bottom: 0 !important; } header, section\[data-field=subtitle\], section\[data-field=description\] { display: none; }

Keep it simple with Dagger 2
============================

Dagger is a Dependency Injection tool for Android (and other kinds of Java/Kotlin) projects. What this means is less code, more structured…

* * *

### Keep it simple with Dagger 2

![](https://cdn-images-1.medium.com/max/800/1*PPT1TgUf3Fxp1ZB6Bv6eJw.jpeg)

Dagger is a [Dependency Injection](https://www.vogella.com/tutorials/DependencyInjection/article.html) tool for Android (and other kinds of Java/Kotlin) projects. What this means is less code, more structured project .

Apart from being cool, dagger is hard. I believe anyone might have had a hard time starting with Dagger, no matter how much it lasted. So, I decided to make an article about a basic setup of Dagger 2 .

> Note: I find hard to use Dagger for Android, so I am using the standard library.

> My project is in Kotlin.

Implementing the dependency library and the annotation processor:

```
implementation **'com.google.dagger:dagger:2.22'  
**kapt **'com.google.dagger:dagger-compiler:2.22'**
```

There are 3 basic things in dagger 2 :

1- Modules -> This is where you declare what dependencies need .

2- Components -> This is the connection between 3 and 1

3- Injecting -> You start requesting these dependencies in order to use them.How do you do that ? Go backwards to step 2 and than you will find dependencies in the step 1.

**Scoping**  
Now I didn’t include scoping in the basic things , but scoping is also important. What you achieve in scoping is you create an hierarchy of your dependencies .For example : A `Retrofit` instance is needed in the whole application only once so you don’t need an instance of that for each activity/fragment you create . But we will jump to scoping a little later .

**Setting up**   
Define a simple Application class . In this way , you can declare what dependencies are in for the whole application.First of all , we need to know , what dependencies will our application needs . In my case (till now) , my app doesn’t need a local database , so I am not using `Room` or `Realm`, or anything.   
The use case : I will have basically an app that connects to the internet and returns a bunch of things from a certain API.   
Don’t forget to include your class in the `AndroidManifest` .

Then , what do I need ? I need `Retrofit` and together with that I need to check what’s happening while I am doing the request , so , I’m going to need also the `OKHttpclient` (which is needed to be set in the `Retrofit` builder). But the client without a logging interceptor has no value , so my `OkHttpClient` also needs the `HttpLoggingInterceptor` . I am also going to need some `CoroutineDispatchers` in order to deal with threading context when I deal with some requests in the `ViewModel` later . I found it in this approach from [Chris Banes](https://medium.com/u/9303277cb6db) [here](https://medium.com/androiddevelopers/rxjava-to-kotlin-coroutines-1204c896a700) . Now I would start with what I need most : Providing `Retrofit `.

**Modules  
**I will call the class `NetworkModule` :

If you check , classes annotated with `@Module` usually tell Dagger : _Hey , I have some dependencies for you to be provided somewhere_ . This class has 2 methods , one for providing my `Retrofit` instance , and the other to provide my `TreasureApi` . What happens is dagger goes to the first method , checks the parameters (which is ones dependency) and than provides with what I say it to return .

Notice the `@ApplicationScope` annotation. As I mentioned early , we have jumped to scoping . Scopes are just fields to define hierarchy for different dependencies . If you mark the method (or the component) with a scope , you basically are telling dagger that you have some hierarchy on your dependencies and Dagger will figure it out for you. The `Singleton` scope is the highest hierarchy scope that you can use . In this use case , I could have used the `@Singleton` annotation , but I did it because I want to have my own , in this case , they are the same .If the method “sees” the `@Singleton` annotation, it will know that it will create only an instance per app .And why is that ? Because I have market the app level components and dependencies with `@Singleton` , or in my case , the `@ApplicationScope` .

This is my application scope. If you notice , there is no much difference between it and the `Singleton` annotation class.

If you see the `provideRetrofit` method , you will notice that it has a parameter for that `client`. Dagger is confused now , because it is looking inside this module for an instance of that client , but he finds not .But the module is basically saying : _Hey , check what I include in this line of code :_  
`includes = [InterceptorModule::class]` . Then , Dagger finds out that it needs to check in the other module for that dependency . And here it is :

Nothing new here .

Now I need a way to give this methods to someone that will be requesting them . The way for doing it , is between components .

**Components**

The component is nothing more than a bridge between classes who request the dependencies and classes that provide them (modules) .

I would like to call the dependencies like this , but there is also another way , which I will be talking later .

Notice the `@ApplicationScope` I told you about . The components needs also to be scoped ,otherwise Dagger wont know what anything means . If you hit the `Build` button in your IDE , Dagger will start looking now for dependencies . It will go to `getTreasureApi(): TreasureApi` and will ask : _Is there anything that provides this class here?_ And than component will tell it : _Yea , I have the_ `_NetworkModule_` _which has a setup for that , in this line of code :_ `modules = [NetworkModule::class, SchedulersModule::class]`

> Note : I am skipping the `SchedulersModule` because it has the same logic for providing dependencies .

After a successful build , you can now Inject what is created (don’t confuse with `@Inject` annotation )

After the build , you can easily type Dagger and the rest will be provided by the IDE . I also need a getter in order to provide the dependencies I just created in the whole application .

**The fun part**

![](https://cdn-images-1.medium.com/max/800/1*qd2dwKNf8Xf7MpaC6AlotQ.jpeg)

Now what ?

Now , jump to your Activity/Fragment and start working .

In my case , I am using a single activity app , since I got inspired from [Ian Lake](https://medium.com/u/51a4f24f5367) video : [Single Activity: Why, When, and How (Android Dev Summit ‘18)](https://www.youtube.com/watch?v=2k8x8V77CrU)  
with Navigation Components , so jumped directly to one of the fragments that I have less work , in order to provide a simple use case for Dagger . The fragment is just showing a list of names from [Prayer Times Api](https://aladhan.com/asma-al-husna-api), but if I want to set up with the [MVVM](https://proandroiddev.com/mvvm-architecture-viewmodel-and-livedata-part-1-604f50cda1) architecture , I might have some dependencies to provide also .

1- This fragment needs an Adapter .

2- This fragment needs a layoutManager for the recyclerview .

3- This fragment needs a ViewModel , which I will help it with a ViewModelProviderFactory, since I have some dependencies for that too .

4- The ViewModelProviderFactory will need an instance of the repository , and my CoroutineDispatchers .

5- The repository on its own needs the `TreasureApi` in order to make calls on the network .

**On with the show**

First i tell dagger about the Scope . Now I’m using dependencies for my `NamesFragment` so I’m going to need another scope .

Then , I’m going to define my modules :

Notice the constructor I have used here . In this case I need to provide a context for that `LinearLayoutManager` instance. I could write it in a different file and than include it here , but I don’t think that is a bad practice as I have used it also . We will jump later on how dagger instantiates that .

Now, I define the component for that fragment :

Notice the `dependencies = [PocketTreasureComponent::class]` line of code . In this way I’m telling dagger that I have dependencies which cannot be found in my module , so you can check the class that I provided you to find the rest of them . If you drop that line of code , you will get an error which says something like :

> TreasureApi cannot be provided without `@Provides` annotation.

Now notice the `inject` method . This is the other way dagger , calls the dependencies . I will be call this method in my fragment and it will be implemented on the “framework” side of dagger.

This will be the `NamesViewModelProviderFactory` which I will be needing to start my ViewModel with some dependencies . The need for a `ViewModelProviderFactory` is because the factory knows that the `ViewModel` has a lifecycle and knows how to start in rather than just call `ViewModel(repository , coroutineDispatcher)` .

Here I am pasting the repository class for this fragment :

Notice the `@Inject` annotation in the constructor .In Dagger you have 3 ways for injection : _Field Injection_, _Constructor Injection_ and _Method Injection_ . Notice I am needing the `TreasureApi` on the moment that the repository has woken . So , the `@Inject` annotation here , just requires that dependency .

The last thing to setup :

In the fragment , I require the dependencies with the field injection usage of the `@Inject` annotation .

> Note : You can’t require the dependencies if you haven’t build your app.

You can notice the `performDI` method where I tell dagger : “Here , I have dependencies for this fragment” . If you check the  
 `.pocketTreasureComponent((activity!!.application as PocketTreasureApplication).getPocketTreasureComponent())` , you will see that I am telling Dagger that I have some application level dependencies .   
Also I promised to talk about the constructor on the `NamesFragmentModule` .   
Now , for modules that have no external dependencies , Dagger knows how to start them by itself . For modules that require dependencies I have to provide the dependencies for them in one way or another , thus I need to specify the context for my `NamesFragmentModule` like this :

```
`namesFragmentModule(NamesFragmentModule(context!!))` 
```

Done !

![](https://cdn-images-1.medium.com/max/800/1*aIIRntdw2stBWukYPGJg8Q.jpeg)

**Conclusion**

Although Dagger 2 has lots of other tools to use , this is the most basic setup for a simple app that you might want to build . Dagger is a little hard , but there is not lack of information about it . However , I have to mention that there are plenty of misguiding tutorials and articles out there . Anyways , want I recommend to learn Dagger 2 are these 3 resources :

#### [The Future of Dependency Injection with Dagger 2](https://www.youtube.com/watch?v=plK0zyRLIP8&t=2065s)

#### [Dagger , that missing guide](https://medium.com/@Zhuinden/that-missing-guide-how-to-use-dagger2-ef116fbea97)

#### [Dagger 2 Android Tutorial](https://www.youtube.com/watch?v=Qwk7ESmaCq0&list=PLuR1PJnGR-Ih-HXnGSpnqjdhdvqcwhfFU)

> Note : please follow this [Reddit thread](https://www.reddit.com/r/androiddev/comments/bjivna/keep_it_simple_with_dagger_2/) in order to clear some questions you might have about providing Adapter and LinearLayoutManager in the module.

Happy Injecting .

By [Stavro Xhardha](https://medium.com/@stavro96) on [May 1, 2019](https://medium.com/p/241d32e14de).

[Canonical link](https://medium.com/@stavro96/keep-it-simple-with-dagger-2-241d32e14de)

Exported from [Medium](https://medium.com) on June 19, 2019.