---
layout: post
title: 'How to fix java.lang.NoSuchMethodError: java.nio.ByteBuffer.clear()Ljava/nio/ByteBuffer; when using Ktor on Heroku?'
---

I'm playing with [Ktor](https://ktor.io/), and yesterday I tried to serve some static content.
According to [the documentation](https://ktor.io/docs/serving-static-content.html#embedded-application-resources), to publish files from the `resources` directory, you have to do the following:
```kotlin
routing {
    // /static is a remote path under which files from the assets folder will be accessible
    static("/static") {
        // assets is a folder in the resources directory in your project
        resources("assets")
    }
}
```

It worked locally, but when I pushed my changes to Heroku, I got this error:
>java.lang.NoSuchMethodError: java.nio.ByteBuffer.clear()Ljava/nio/ByteBuffer;

I'm using Ktor with Kotlin 1.6.10 without any special config or dependencies.
The issue happens due to some incompatibilities between JDK 1.8 and JDK 1.8+.
To fix the issue, I decided to set `jvmTarget` and runtime to Java 11.

To do so add the following code to your `build.gradle.kts` file:
```kotlin
tasks.withType<org.jetbrains.kotlin.gradle.tasks.KotlinCompile> {
    kotlinOptions.jvmTarget = JavaVersion.VERSION_11.toString()
}
```

To specify the Java version on Heroku, you need to create the `system.properties` file with the following content:
```
java.runtime.version=11
```
Ref.: <https://devcenter.heroku.com/articles/java-support#specifying-a-java-version>

Happy building!
