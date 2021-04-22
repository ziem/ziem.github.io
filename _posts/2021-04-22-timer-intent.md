---
layout: post
title: Timer Intent
---

Recently, I started using [KptnCook](https://www.kptncook.com/), and it's a great cooking app with recipes changing daily.

What's funny, it's that it made me realize we have timer `Intent` in Android. When we go to the cooking steps screen, it's embedded inside steps description:

![KptnCook](/assets/images/timer-intent/kptncook.png)
After clicking "20 min", the Clock app is opened, and the timer starts.

Creating timer `Intent` is simple! We need to use [`AlarmClock.ACTION_SET_TIMER`](https://developer.android.com/reference/android/provider/AlarmClock#ACTION_SET_TIMER) action and provide timer details:
- [`AlarmClock.EXTRA_LENGTH`](https://developer.android.com/reference/android/provider/AlarmClock#EXTRA_LENGTH) -- timer's length in seconds;
- [`AlarmClock.EXTRA_MESSAGE`](https://developer.android.com/reference/android/provider/AlarmClock#EXTRA_MESSAGE) -- timer's description;
- [`AlarmClock.EXTRA_SKIP_UI`](https://developer.android.com/reference/android/provider/AlarmClock#EXTRA_SKIP_UI) -- setting this extra to `true` bypasses any confirmation UI and starts the timer automatically.

To recreate KptnCook's timer behavior we need:

```kotlin
val timerIntent = Intent(AlarmClock.ACTION_SET_TIMER).apply {
  putExtra(AlarmClock.EXTRA_LENGTH, 20 * 60) // 20 minutes
  putExtra(AlarmClock.EXTRA_SKIP_UI, true) // start timer automatically
}
```
Then we call `startActivity(timerIntent)`, and that's it.

Wait! There is one more thing... We need to add:
```xml
<uses-permission android:name="com.android.alarm.permission.SET_ALARM" />
```
to the AndroidManifest.xml.

If you would like to learn more about commonly used `Intent`s, visit [Common Intents](https://developer.android.com/guide/components/intents-common) guide.
