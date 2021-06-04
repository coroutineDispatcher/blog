---
title: "Server side kotlin using Ktor"
date: 2021-06-03T18:35:09+02:00
draft: false
tags: ["kotlin", "server-side", "ktor"]
---

![Photo by scienceinhd @ Unsplash](/images/server_side_kotlin.jpg)

Let's be honest, I didn't enjoy Ktor in the beginning. It seemed pretty confusing, different, immature and difficult to start with. Watching it grow from away was a little simpler. However, I returned to ktor since I was interested more in server side Kotlin. Kotlin is doing great, by the way, lots of improvements and new exciting features came up with [1.5.0 release](https://kotlinlang.org/docs/whatsnew15.html), which makes it the perfect time to consider using it not only on Android, but also server side, desktop etc.

# Getting started.

Before starting with Ktor, please note that I have no huge expertise on the platform, therefore, I might be killing flies with Bazukas. But let's see if I get some nice feedback for this, which would be also valuable.

First thing to do is to visit [start.ktor.io](https://start.ktor.io/#) , which is not different from the concept of creating a new spring (boot) project. For anyone not familliar with start.ktor.io, it's a webform that generates your project according to your dependency requirements and technologies you want to use for each layer. As far as I know, you can do it from the IDE as well, but since it's not the first project I create for Ktor, this one works pretty well for me. The screenshot below would help you even more: 

![](/images/ktor_start.png)

After configuring your project, let it download and then open it from IntelliJ IDEA (probably possible with Android Studio as well, but never tried it). Cool advantage is that a Ktor project *by default* now starts with `kts` gradle files instead of `groovy`. Now let's talk about what we are using for this plain example: 

**Graphql as a technology (kgraphql):**

```kotlin
    implementation("com.apurebase:kgraphql:$kgraphql_version")
    implementation("com.apurebase:kgraphql-ktor:$kgraphql_version")
```

**Mongo DB for presistence (kmongo):**

```kotlin
    implementation("org.litote.kmongo:kmongo:$kmongo_version")
```

**Ktor authentication and JWT (since our example will consist in an authentication schema):**


```kotlin
    implementation("io.ktor:ktor-auth:$ktor_version")
    implementation("io.ktor:ktor-auth-jwt:$ktor_version")
```

**Not going much in detail, but Bcrypt for password encryption/decryption:**

```kotlin
implementation("at.favre.lib:bcrypt:$bcrypt_version")
```