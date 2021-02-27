---
layout: post
title: Google Maps - misc
---

Are you tired fighting `(Support)MapFragment`? There is a simpler solution: use `MapView`.

`MapView` class can be used instead of `[Support]MapFragment`. It's a view which extends `FrameLayout` and displays a map.

If you want to use it you just need to forward these lifecycle methods

* `onCreate(Bundle)`,
* `onResume()`,
* `onPause()`,
* `onDestroy()`,
* `onSaveInstanceState()`,
* `onLowMemory()`,

and voilà.

It has the same callbacks as map fragment:

* `getMap()` that is deprecated and should be used in favour of asynchronous one,
* `getMapAsync()` that is triggered when the map is ready to be used.

---

There is also [a lite mode map](https://developers.google.com/maps/documentation/android/lite):

> **The Google Maps Android API can serve a static image as a 'lite mode' map.**
>
> A lite mode map is a bitmap image of a map at a specified location and zoom level. Lite mode supports all of the map types (normal, hybrid, satellite, terrain) and a subset of the functionality supplied by the full API. Lite mode is useful when you want to provide a number of maps in a stream, or a map that is too small to support meaningful interaction.

You can enable it programmatically:


```java
GoogleMapOptions options = new GoogleMapOptions().liteMode(true);
```

or in your `layout/*.xml` file:

```xml
<fragment xmlns:android="https://schemas.android.com/apk/res/android"
    xmlns:map="https://schemas.android.com/apk/res-auto"
    android:name="com.google.android.gms.maps.MapFragment"
    android:id="@+id/map"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    map:cameraZoom="13"
    map:mapType="normal"
    map:liteMode="true" />
```

In the [documentation](https://developers.google.com/maps/documentation/android/lite#supported_api_features), you can find all the things it supports.

## Snapshot

Yet another feature Google Maps offer are snapshots:

> You can use the Google Maps built-in snapshot method, to capture a preview and display it in an ImageView.

To learn more, I recommend reading: [Android Google Map to show as a picture](https://stackoverflow.com/questions/26946503/android-google-map-to-show-as-picture).

## Utils

There is also a project called [Google Maps Android API utility library](https://github.com/googlemaps/android-maps-utils) which provides extensions to Google Maps components.

It extends Google Maps library functionalities by adding:
- clustering,
- heat maps,
- and more:

<iframe width="560" height="315" src="https://www.youtube.com/embed/nb2X9IjjZpM" frameborder="0" allowfullscreen></iframe>

You can add it to your project by adding this dependency:

```groovy
dependencies {
    compile 'com.google.maps.android:android-maps-utils:0.4.3'
}
```

