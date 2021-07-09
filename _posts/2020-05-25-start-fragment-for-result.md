---
layout: post
title: How to start a Fragment for a result?
---
Getting a result from a `Fragment` is almost identical to the `startActivityForResult`, `setResult`, and `onActivityResult` combo.

In the [1.3.0-alpha04](https://developer.android.com/jetpack/androidx/releases/fragment#1.3.0-alpha04) version of the AndroidX Fragment library, Google introduced new APIs that allow passing data between `Fragment`s.

> Added support for passing results between two Fragments via new APIs on FragmentManager. This works for hierarchy fragments (parent/child), DialogFragments, and fragments in Navigation and ensures that results are only sent to your Fragment while it is at least STARTED. ([b/149787344](https://issuetracker.google.com/issues/149787344))

Thanks to the Fragment Result API, `FragmentManager` gained two new methods:

- [`FragmentManager#setFragmentResult(String, Bundle)`](https://developer.android.com/reference/androidx/fragment/app/FragmentManager#setFragmentResult(java.lang.String,%20android.os.Bundle)) which you can treat similarly to the existing [`Activity#setResult` ](https://developer.android.com/reference/android/app/Activity#setResult(int));
- [`FragmentManager#setFragmentResultListener(String, LifecycleOwner, FragmentResultListener)`](https://developer.android.com/reference/androidx/fragment/app/FragmentManager#setFragmentResultListener(java.lang.String,%20androidx.lifecycle.LifecycleOwner,%20androidx.fragment.app.FragmentResultListener)) which allows you to listen/observe result changes.

How to use it?

In `FragmentA`, set up `FragmentResultListener` in the `onCreate` method:
```kotlin
setFragmentResultListener("request_key") { requestKey: String, bundle: Bundle ->
    val result = bundle.getString("your_data_key")
    // do something with the result
}
```

In `FragmentB`, add this code to return the result:
```kotlin
val result = Bundle().apply {
    putString("your_data_key", "Hello!")
}
setFragmentResult("request_key", result)
```

Start `FragmentB` e.g. by using:
```kotlin
findNavController().navigate(NavGraphDirections.yourActionToFragmentB())
```

To close / finish `FragmentB` call:
```kotlin
findNavController().navigateUp()
```

Now, your `FragmentResultListener` will be notified, and you will receive your result.

You can use other methods to achieve something similar:
- scoped `ViewModel`,
- `currentBackStackEntry` & `previousBackStackEntry` & `SavedStateHandle` combo,
- more in the [Equivalent of startActivityForResult() with Android Architecture Navigation](https://stackoverflow.com/q/50702643/759007) StackOverflow question.

\* I'm using `fragment-ktx` to simplify the code above.
