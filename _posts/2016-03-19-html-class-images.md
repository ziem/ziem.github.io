---
layout: post
title: Html class - images
summary: Handling images
---

The `Html` class supports images just like HTML. Let's see how `Html.fromHtml()` method handles them. In this blog post, I will show you how to use `ImageGetter` to display images in the `TextView`.

First, we need to add `<img src="..." />` tag to our HTML string. Next, we see what happens, but we encounter the following problem:

![broken img tag](/assets/images/html-class-images/broken-img-framed.png)

Why was such a square displayed? It's `fromHtml()` default behavior, it uses `com.android.internal.R.drawable.unknown_image` drawable when we don't pass an `ImageGetter` object to this method. What is the `ImageGetter`? It's an interface with one method `getDrawable(String source)` which is used by the `Html` class to retrieve `Drawable`. We implement this interface when we want to support `<img />` tags. `source` argument is equal to `src` attribute value. `ImageGetter`'s `getDrawable(String source)` is triggered by the `Html` class everytime it encounters `<img />` tag.

Now I will show you an example of how to use the `ImageGetter` interface and load drawable based on `src` attribute value.

```java
public class ResourcesImageGetter implements Html.ImageGetter {
    private Context context;

    public ResourcesImageGetter(Context context) {
        this.context = context;
    }

    @Override
    public Drawable getDrawable(String source) {
        String drawableName = source.substring(0, source.lastIndexOf('.'));
        Drawable drawable = getDrawableByName(drawableName);
        if (drawable != null) {
            int width = drawable.getIntrinsicWidth();
            int height = drawable.getIntrinsicHeight();

            drawable.setBounds(0, 0, width, height);
        }

        return drawable;
    }

    private Drawable getDrawableByName(String name) {
        Resources resources = context.getResources();
        String packageName = context.getPackageName();

        final int resourceId = resources.getIdentifier(name, "drawable", packageName);

        return resources.getDrawable(resourceId);
    }
}
```

The code is rather obvious. Based on the image name, I'm loading the application `Drawable` resources (e.g. `ic_launcher.png` will be replaced by `drawable/ic_launcher.png` image).

As the [documentation][get_drawable_method] says, don't forget to call `setBounds()` on the `Drawable` object:

> Make sure you call setBounds() on your Drawable if it doesn't already have its bounds set.

Below we can see the final result:
![working img tag](/assets/images/html-class-images/working-img-framed.png)

[get_drawable_method]: https://developer.android.com/reference/android/text/Html.ImageGetter.html#getDrawable%28java.lang.String%29 "getDrawable method"
