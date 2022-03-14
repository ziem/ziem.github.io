---
layout: post
title: Navigation Component & Fragments
---

## Elements of Navigation Component

Three main building blocks of Navigation Component are:
- navigation graph,
- [`NavHost`][nav_host],
- [`NavController`][nav_controller].

### `navigation_graph.xml`

Navigation graph consists of destinations we can navigate to and actions that define navigation flows.

```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/navigation_graph"
    app:startDestination="@id/router">

    <fragment
        android:id="@+id/article"
        android:name=".ArticleFragment"
        android:label="Article">

        <argument
            android:name="articleId"
            app:argType="string"
            app:nullable="true" />

    </fragment>

    <action
        android:id="@+id/action_global_article"
        app:destination="@id/article" />

   ...

</navigation>
```
// obrazek nawigacji z edytora w AS

### [`NavHost`][nav_host]

[`NavHost`][nav_host] is a container for destinations. Currently, there is only one implementation named [`NavHostFragment`](https://developer.android.com/reference/androidx/navigation/fragment/NavHostFragment). It's recommended to use [`FragmentContainerView`](https://developer.android.com/reference/androidx/fragment/app/FragmentContainerView) which is alternative to `FrameLayout` previously used to host `Fragment`s. In comparison to the `FrameLayout` "it can reliably handle Fragment Transactions, and it also has additional features to coordinate with fragment behavior". You can learn more about it [here](https://developer.android.com/reference/androidx/fragment/app/FragmentContainerView).

```xml
<androidx.fragment.app.FragmentContainerView
    android:id="@+id/nav_host"
    android:name="androidx.navigation.fragment.NavHostFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```


### [`NavController`][nav_controller]

> [`NavController`][nav_controller] manages app navigation
within a [`NavHost`][nav_host].

[`NavController`][nav_controller] is accessible almost everywhere. Just look for the extension functions called `findNavController()`:
```kotlin
Fragment.findNavController()
View.findNavController()
Activity.findNavController(viewId: Int)

val navHost = fragmentManager.findFragmentById(R.id.nav_host)
        as NavHostFragment
val navController = navHostFragment.navController
```

The most important [`NavController`][nav_controller]s method is `navigate(...)` which, as you can guess, it's used to navigate to destinations defined in the `navigation.graph.xml`.

## How to do X with AndroidX Navigation & Fragments?

### Navigation

Navigation, the basic operation, can be invoked by using [`NavController`][nav_controller]:

```kotlin
findNavController()
    .navigate(NavGraphDirections.yourActionToFragmentX())
```

### Back press handling (classic `WebView` example)

In this scenario we want to first navigate back on `WebView`'s history and then close a `Fragment`.
To do it, we need register [`OnBackPressedCallback`](https://developer.android.com/reference/androidx/activity/OnBackPressedCallback) which allows us to intercept back button press event. It has only one method we need to override: `handleOnBackPressed()` which notifies us when back press occurs. We register this callback by using `Activity`'s `onBackPressedDispatcher`.

```kotlin
private val backButtonCallback = object : OnBackPressedCallback(true) {
    override fun handleOnBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack()
        } else {
            remove()
            requireActivity().onBackPressed()
        }
    }
}

override fun onCreate(savedInstanceState: Bundle?) {
    ...

    requireActivity().onBackPressedDispatcher
        .addCallback(this, backButtonCallback)
}
```

### Getting a result from a `Fragment`

Getting a result from a `Fragment` is almost identical to the `startActivityForResult`, `setResult` and `onActivityResult` combo.
First, we need to register [`FragmentResultListener`](https://developer.android.com/reference/androidx/fragment/app/FragmentResultListener) using `setFragmentResultListener` -- it behaves similar to `onActivityResult`. Then we `findNavController().navigate(...)` (`startActivityForResult` "equivalent") to the next `Fragment`. In the second `Fragment`, to return some results `setFragmentResult` (`setResult` equivalent) should be called, and we close it by calling `findNavController().navigateUp()` (`finish()` equivalent). We just need to remember to provide correct keys to both `setFragmentResultListener` and `setFragmentResult`.

// sticky events?

`FragmentA.kt`
```kotlin
setFragmentResultListener("request_key") {
    requestKey: String, bundle: Bundle -> {
        val result = bundle.getString("your_data_key")
        // do something with the result
    }
}

findNavController().navigate(...)
```

`FragmentB.kt`
```kotlin
val result = Bundle().apply {
    putString("your_data_key", "Hello!")
}
setFragmentResult("request_key", result)

findNavController().navigateUp()
```

Other methods:
- scoped `ViewModel`,
- `currentBackStackEntry` & `previousBackStackEntry` & `SavedStateHandle` combo,
- more in <https://stackoverflow.com/q/50702643/759007>.

### Deep links

#### Manually by passing data through `Intent`

#### By using `NavDeepLinkBuilder` which creates `PendingIntent` (explicit deep link)

`NavDeepLinkBuilder` provided by Navigation Component can be used to create `PendingIntent` we can attach to e.g. notifications.
To build `PendingIntent` we need to pass: out main `Activity`, navigation_graph.xml`, target destination, and it's arguments.

```kotlin
val pendingIntent = NavDeepLinkBuilder(context)
    .setComponentName(MainActivity::class.java)
    .setGraph(R.navigation.nav_graph)
    .setDestination(R.id.newsfeed_web)
    .setArguments(...)
    .createPendingIntent()
```

#### By using `Uri` (implicit deep link)

```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android">

    <fragment
        android:id="@+id/article"
        android:name=".ArticleFragment">

        <argument android:name="articleId" />

        <deepLink app:uri="vg://article/{articleId}" />

    ...

</navigation>
```

### SafeArgs

Navigation Component provides SafeArgs Gradle Plugin which generates code for use for type-safe navigation.

For the following navigation graph node:
```xml
<fragment
        android:id="@+id/article"
        android:name="...ArticleFragment"
        android:label="ArticleFragment">

    <argument
            android:name="articleId"
            app:argType="string" />

</fragment>
```
it will generate action and args:

```kotlin
val articleId = "kRmLdL"
val dir = NavGraphDirections.actionGlobalArticle(articleId)

val fragmentArgs = ArticleFragmentArgs(articleId)
val bundle = fragmentArgs.toBundle()

private val args: ArticleFragmentArgs by navArgs()
```

With type-safe `ArticleFragmentArgs` we don't have to maintain our own arguments' keys. Everything is handled for us under the hood.

```kotlin
public data class ArticleFragmentArgs(
  public val articleId: String,
) : NavArgs {
  @Suppress("CAST_NEVER_SUCCEEDS")
  public fun toBundle(): Bundle {
    val result = Bundle()
    result.putString("articleId", this.articleId)
    return result
  }

  ...

}
```

## Testing

### Basic `Fragment` test

```kotlin
@RunWith(AndroidJUnit4::class)
class PodcastFragmentTest {

    @Test
    fun start() {
        val args = PodcastFragmentArgs(Referrer.Test()).toBundle()

        val scenario = launchFragmentInContainer<PodcastFragment>(args)

        onView(withId(...)
            .check(matches(isDisplayed()))
    }
}
```

#### `launchFragmentInContainer`

```kotlin
launchFragmentInContainer<PodcastFragment>(
    Bundle(),
    themeResId = R.style.AppTheme
)
```

#### `FragmentScenario`

```kotlin
fragmentScenario.onFragment { fragment ->
    fragment.recreate()

    fragment.moveToState(Lifecycle.State.CREATED)

    fragment.showTitle("New title")
}
```

#### `TestNavHostController`

```kotlin
val navController = TestNavHostController(
    ApplicationProvider.getApplicationContext()
)
navController.setGraph(R.navigation.navigation_graph)

val fragmentScenario = launchFragmentInContainer<PodcastFragment>(...)

fragmentScenario.onFragment { fragment ->
    Navigation.setViewNavController(fragment.requireView(), navController)
}
```

```kotlin
val fragmentScenario = launchFragmentInContainer<PodcastFragment>(...)

val fragmentScenario = launchFragmentInContainer {
    PodcastFragment(...).also { fragment ->
        fragment.viewLifecycleOwnerLiveData.observeForever {
            viewLifecycleOwner -> if (viewLifecycleOwner != null) {
                Navigation.setViewNavController(
                    fragment.requireView(),
                    navController
                )
            }
        }
    }
}
```

## Challenges

- Nav component destroys Fragment’s view when navigating to the new Fragment
- Leaking RecyclerView’s adapters
- Testing is sometimes tricky
- Not everything is supported


## Quiz

```xml
<fragment
    android:id="@+id/web"
    android:name="com.schibsted.publishing.hermes.web.WebFragment"
    android:label="WebFragment">

    ...

    <argument
        android:name="benefitCardUrl"
        android:defaultValue="null"
        app:argType="string"
        app:nullable="true" />

</fragment>
```

```xml
<fragment
    android:id="@+id/web"
    android:name="com.schibsted.publishing.hermes.web.WebFragment"
    android:label="WebFragment">

    ...

    <argument
        android:name="benefitCardUrl"
        android:defaultValue="@null"
        app:argType="string"
        app:nullable="true" />

</fragment>
```
[nav_host]: https://developer.android.com/reference/androidx/navigation/NavHost
[nav_controller]: https://developer.android.com/reference/androidx/navigation/NavController
