---
layout: post
title: Android N Developer Preview is here!
summary: Android N Developer Preview
---

Today Google released Android N Developer Preview. If you would like to find more information, please visit [developer.android.com](https://developer.android.com/preview/index.html).

There you will find six sections:

 * [Program Overview](https://developer.android.com/preview/overview.html)
 * [Android N for Developers](https://developer.android.com/preview/api-overview.html)
 * [Behavior Changes](https://developer.android.com/preview/behavior-changes.html)
 * [Set Up the Preview](https://developer.android.com/preview/setup-sdk.html)
 * [Samples](https://developer.android.com/preview/samples.html)
 * [Support and Release Notes](https://developer.android.com/preview/support.html)

In the **Program Overview** section, you will find information about the timeline and updates of preview releases.

**Android N for Developer** describes changes in Android SDK which include:

 * [Multi-window support](https://developer.android.com/preview/features/multi-window.html)

<img src="/assets/images/android-n-developer-preview-is-here/multi-window-support-framed.png" alt="multi-window-support" style="width: 270px;"/>

 * [Notifications](https://developer.android.com/preview/features/notification-updates.html)

<img src="/assets/images/android-n-developer-preview-is-here/notification-replay-framed.png" alt="notification-replay" style="width: 240px; float:left;"/><img src="/assets/images/android-n-developer-preview-is-here/notification-group-framed.png" alt="notification-group" style="width: 240px; float:left;"/><img src="/assets/images/android-n-developer-preview-is-here/notification-group-expanded-framed.png" alt="notification-group-expanded" style="width: 240px; float:left;"/>

<div style="clear:both;"/>

 * [Quick Settings Tile API](https://developer.android.com/preview/api-overview.html#tile_api)
 * [Data Saver](https://developer.android.com/preview/features/data-saver.html)
 * [and more](https://developer.android.com/preview/api-overview.html)

**Behavior Changes** section tells you about:

 * Performance Improvements -- Doze & Project Svelte
 * Permissions Changes -- Android N introduces new permission: `ACTION_OPEN_EXTERNAL_DIRECTORY` also `GET_ACCOUNTS` is now deprecated
 * Accessibility Improvements -- Screen Zoom enables a user to magnify or shrink all elements on the screen
 * Platform Migration toward OpenJDK 8, NDK Apps Linking to Platform Libraries, and Android for Work

**Set Up the Preview** guides you on how to get Android Studio 2.1, N Preview SDK, Java 8 JDK, and set up your development environment.

Thanks to [Jack](https://source.android.com/source/jack.html) you can now use [Java 8 features](https://developer.android.com/preview/j8-jack.html) in Android back to Gingerbread. The following Java 8 features are available when targeting the Android N Preview:

 * [Default and static interface methods](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)
 * [Lambda expressions](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
```java
fab.setOnClickListener(v -> {
    Toast.makeText(getActivity(), "Hello Lambda", Toast.LENGTH_SHORT)
        .show();
});
```
 * [Repeatable annotations](https://docs.oracle.com/javase/tutorial/java/annotations/repeating.html).

You can enable Java 8 in Android Studio by setting the JDK Location field to the location of the Java 8 JDK and updating your `build.gradle` file:

```groovy
android {
    compileSdkVersion 'android-N'
    buildToolsVersion 24.0.0

    ...

    defaultConfig {
        minSdkVersion 'N'
        targetSdkVersion 'N'

        ...
    }

    ...

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```
Be aware that the Instant Run does not currently work with Jack and will be disabled.

The **Samples** section contains 5 sample apps that demonstrate new Android N features including:

 * [Multi-Window Playground](https://github.com/googlesamples/android-MultiWindowPlayground)
 * [Active Notifications](https://github.com/googlesamples/android-ActiveNotifications)
 * [Messaging Service](https://github.com/googlesamples/android-MessagingService)
 * [Direct boot](https://github.com/googlesamples/android-DirectBoot)
 * [Scoped Directory Access](https://github.com/googlesamples/android-ScopedDirectoryAccess)

In the **Support and Release Notes** section will find notes, known issues, and general advisories.

If you would like to read more about the new developer preview release, please visit:

 * [First Preview of Android N: Developer APIs & Tools](https://android-developers.blogspot.com/2016/03/first-preview-of-android-n-developer.html)
 * [Random Musings on the N Developer Preview](https://commonsware.com/blog/2016/03/09/random-musings-n-developer-preview.html)
 * [Changelog for N Support Libraries](https://michaelevans.org/blog/2016/03/09/changelog-for-n-support-libraries/)
 * [What’s New in Android N](https://www.youtube.com/watch?v=CsulIu3UaUM)
