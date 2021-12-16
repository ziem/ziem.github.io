---
layout: post
title: Web intent resolution in Android 12
---

It's just a small reminder regarding web intent resolution in Android 12. As it's described in the [Behavior changes: all apps](https://developer.android.com/about/versions/12/behavior-changes-all) section for Android 12 and in the [Create Deep Links to App Content](https://developer.android.com/training/app-links/deep-linking) guide, you need to have your domain linked with your app.

> **Note:** Starting in Android 12 (API level 31), a generic web intent resolves to an activity in your app only if your app is approved for the specific domain contained in that web intent. If your app isn't approved for the domain, the web intent resolves to the user's default browser app instead.

All steps to make it work are clearly described in the [Behavior changes: all apps](https://developer.android.com/about/versions/12/behavior-changes-all) section for Android 12:

> * Verify the domain using [Android App Links](https://developer.android.com/training/app-links/verify-site-associations).<br/><br/>
> On apps that target Android 12 or higher, the system changes how it [automatically verifies](https://developer.android.com/training/app-links/verify-site-associations#auto-verification) your app's Android App Links. In your app's [intent filters](https://developer.android.com/training/app-links/verify-site-associations#add-intent-filters), check that you include the `BROWSABLE` category and support the `https` scheme.<br/><br/>
> On Android 12 or higher, you can [manually verify](https://developer.android.com/training/app-links/verify-site-associations#manual-verification) your app's Android App Links, to test how this updated logic affects your app.<br/><br/>
> * [Request the user to associate your app with the domain](https://developer.android.com/training/app-links/verify-site-associations#request-user-associate-app-with-domain) in system settings.


