---
title: 'Annotation processor: Say less, mean more'
date: 2019-06-19T10:33:00.002+02:00
draft: false
aliases: [ "/2019/06/annotation-processor-say-less-mean-more.html" ]
author: "Stavro Xhardha"
---

Annotation processor: Say less, mean more \* { font-family: Georgia, Cambria, "Times New Roman", Times, serif; } html, body { margin: 0; padding: 0; } h1 { font-size: 50px; margin-bottom: 17px; color: #333; } h2 { font-size: 24px; line-height: 1.6; margin: 30px 0 0 0; margin-bottom: 18px; margin-top: 33px; color: #333; } h3 { font-size: 30px; margin: 10px 0 20px 0; color: #333; } header { width: 640px; margin: auto; } section { width: 640px; margin: auto; } section p { margin-bottom: 27px; font-size: 20px; line-height: 1.6; color: #333; } section img { max-width: 640px; } footer { padding: 0 20px; margin: 50px 0; text-align: center; font-size: 12px; } .aspectRatioPlaceholder { max-width: auto !important; max-height: auto !important; } .aspectRatioPlaceholder-fill { padding-bottom: 0 !important; } header, section\[data-field=subtitle\], section\[data-field=description\] { display: none; }

I’ve always been curious on what is behind an annotation. As much as they made my angry, believe me they are so fun. This is my experience…

* * *

![](https://cdn-images-1.medium.com/max/800/1*Jsodd7y2uNxBeWED_ufuHg.jpeg)

I’ve always been curious on what is behind an annotation. As much as they made my angry, believe me they are so fun. This is my experience as a beginner on the Annotation Processing.

I will give my own definition for an annotation:

_An annotation is just a way to mark a class, field, another annotation, method etc. Why? It just tells that the marked component has a special attribute. But how do you handle it?_

The way to handle an `Annotation` is through generating code at compile time. This has always intrigued me. Why? This depends on the use case. Have you ever thought how [Dagger2](https://github.com/google/dagger) knows what dependencies you are using? Or perhaps how [Butterknife](https://jakewharton.github.io/butterknife/) knows how to bind views or set an `onClickListener`? Yep, generating code at compile time. The process of generating code at compile time to handle the annotations is called `Annotation Processing` . I will do a simple use case, explaining all what’s happening.

**The use case:**

I hate to type:

```
activity.getSupportFragmentManager()  
.beginTransaction()  
.replace(R.id.someId,SomeFragment.newInstance())  
.commit()
```

for each fragment that I need to start. Someone here would be right to say that what do we need the annotations for, but this article is for “academical” reasons, not for providing some best practice or refactor.

**Setup:**

First of all, you must know that creating an annotation and handling it, you need separated modules. That’s because we need to tell `gradle` that there is some code that needs to be imported and the other code that works as a compiler. To create a new module in Android Studio go to _File->New->Module_ and select Java Library.

> Note, Android Library is not necessary in this case. Choose Android library when you are creating a custom view class or whatever.

So let’s go :

![](https://cdn-images-1.medium.com/max/800/1*CZm2Gzk1ChAbRFkK_gcPkA.png)

I have created 2 more modules except from my current Android app module. I named my modules Browser since they I will basically navigate through app fragments.

Let’s start with the easy one: Create the desired annotation:

There are 2 important things here: The `@Target` which does the check if I have annotated my desired component, in my case a `CLASS` . If I annotate a method or a filed it will show an error, and the `@Retention` _which determines how an annotation is stored in binary output_ (from Kotlin documentation). There are 3 kinds of `AnnotationRetention` s:

![](https://cdn-images-1.medium.com/max/800/1*k0uhVDP1LrBlQRKevAHVxA.png)

For the moment, I’m just letting it to source because I don’t need any of those other options.

Go to the `browser_compiler` and open its `build.gradle` to import these libraries:

We will explain other dependencies later. For the moment just notice importing the above module in order to handle that Annotation in the compiler module.

> Note: don’t forget to add the `apply plugin: ‘kotlin-kapt’` ,otherwise, forget about a successful build.

Create a class and extend `AbstractProcessor` (overriding the methods of course). Since I am using [kotlin metadata library](https://github.com/Takhion/kotlin-metadata), I am extending the `KotlinAbstractProcessor` but there is no difference between them, except from accessing some fields directly on this case.

This is why we imported the other module here, to access the annotation.

**Work:**

The work will happen in the `process` method. This is where the magic happens:

This is where I am talking to the Kotlin compiler and telling it that if there is any thing annotated with `@BindFragment` annotation apart from a class, show some error message.

Our `elementUtils.getPackageOf(annotatedElement).toString()` will just provide us the annotated class package name, while the `annotatedElement.simpleName().toString()` will just provide us the annotated class name. Now let’s jump to the `startClassGeneration` method. I am using the [KotlinPoet](https://github.com/square/kotlinpoet) library to generate kotlin code on the compile time, but it is not mandatory, you can also type simple strings as long as it is correctly used without and without compile errors. This is the code:

You can read the documentation on what is doing one and what is doing the other.

Details are important:

We **must** annotate this generator class with the `@AutoService(Processor::class)` . It just creates a registration file for this custom processor. Not providing it will force you to include your processor manually in the `META-INF` directory. That way it can be accessed through all the project.

Full processor file:

**The moment of truth:**

Go to your `build.gradle` module app and import both your modules, the `browser` module as an implementation and the `browser_compiler` as the processor using `kapt`. Note to include `kotlin-kapt` here also.

Annotate your Fragments with your created annotation:

Annotate your second fragment, which you will navigate after a button click in the first fragment:

Hit BUILD on your IDE and go to main activity to open the `FirstFragment`

And the `FirstFragment` , on the button click will have this code:

Done!

![](https://cdn-images-1.medium.com/max/800/1*Jp2AG9J0ou_8KarZmlPx1w.gif)

**Conclusion:**

There is plenty to learn on generating code at compile time and this includes me too. I want to start creating a real library, once I feel confident on this.

Happy processing!

By [Stavro Xhardha](https://medium.com/@stavro96) on [May 31, 2019](https://medium.com/p/b0e23dd9a3e2).  
[Canonical link](https://medium.com/@stavro96/annotation-processor-say-less-mean-more-b0e23dd9a3e2)  
Exported from [Medium](https://medium.com/) on June 19, 2019.