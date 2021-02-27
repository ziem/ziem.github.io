---
layout: post
title: Html class - basic usage
summary: Basic usage
---

From time to time, there is a need to display some styled text in an Android application. You can always use <a href="https://flavienlaurent.com/blog/2014/01/31/spans/" type="Spans">Spans</a> to achieve your goals, but if you know HTML, you could consider using <code><a href="https://developer.android.com/reference/android/text/Html.html" title="Html class">Html</a></code> class.

The `Html` class with <code><a href="https://developer.android.com/reference/android/text/Html.ImageGetter.html" title="ImageGetter interface">ImageGetter</a></code> and <code><a href="https://developer.android.com/reference/android/text/Html.TagHandler.html" title="TagHandler interface">TagHandler</a></code> interfaces is used to convert HTML strings into styled text. At the time of writing `Html` class contains four static methods:

```java
static String escapeHtml(CharSequence text);
static Spanned fromHtml(String source);
static Spanned fromHtml(String source, Html.ImageGetter imageGetter, Html.TagHandler tagHandler);
static String toHtml(Spanned text);
```

The first method allows us to get an HTML-escaped representation of given plain text. The fourth we can use when we want to get an HTML representation of the provided `Spanned` text. `fromHtml` methods return styled text (`Spanned`) from provided HTML string. You may wonder, how does it work? As you can find at [https://grepcode.com][html_java] `Html` class parses HTML strings using [TagSoup][tag_soup] which is a SAX-compliant parser written in Java.

In this post, I will focus on the second method because, in my opinion, it is used most of the time. Let's assume that we would like to display the following text:

> <b>Lorem ipsum dolor</b> sit <u>amet</u>, consectetur <font color="green">adipiscing</font> elit. <i>Cras ut magna eget erat fermentum dignissim.</i> Aenean interdum ultrices sapien<sup>1</sup>. <font color="red">Vestibulum feugiat ultrices neque, vel pretium massa. Etiam tempor luctus dui eget aliquam.</font> In nec metus sit amet orci facilisis pretium.

Creating `Span`s manually can be cumbersome so let's use a `fromHtml` method.

First of all, we have to create an HTML representation of our text:

```html
<b>Lorem ipsum dolor</b> sit <u>amet</u>, consectetur <font color="green">adipiscing</font> elit. <i>Cras ut magna eget erat fermentum dignissim.</i> Aenean interdum ultrices sapien<sup>1</sup>. <font color="red">Vestibulum feugiat ultrices neque, vel pretium massa. Etiam tempor luctus dui eget aliquam.</font> In nec metus sit amet orci facilisis pretium.
```

next, we put it into a `string.xml` file:
```xml
<string name="lorem"><![CDATA[<b>Lorem ipsum dolor</b> sit <u>amet</u>, consectetur <font color="green">adipiscing</font> elit. <i>Cras ut magna eget erat fermentum dignissim.</i> Aenean interdum ultrices sapien<sup>1</sup>. <font color="red">Vestibulum feugiat ultrices neque, vel pretium massa. Etiam tempor luctus dui eget aliquam.</font> In nec metus sit amet orci facilisis pretium.]]></string>
```

in the `*Activity.java` file we use the `Html` class:

```java
TextView textView = ...;
String string = getString(R.string.lorem);
Spanned spanned = Html.fromHtml(string);
textView.setText(spanned);
```

and voilà:

![Screenshot of the result](/assets/images/html-class-basic-usage/result-framed.png)

If you would like to know what other methods return, I'm attaching snippets.

`escapedHtml` method returns:

```html
&lt;b&gt;Lorem ipsum dolor&lt;/b&gt; sit &lt;u&gt;amet&lt;/u&gt;, consectetur &lt;font color=green&gt;adipiscing&lt;/font&gt; elit. &lt;i&gt;Cras ut magna eget erat fermentum dignissim.&lt;/i&gt; Aenean interdum ultrices sapien&lt;sup&gt;1&lt;/sup&gt;. &lt;font color=red&gt;Vestibulum feugiat ultrices neque, vel pretium massa. Etiam tempor luctus dui eget aliquam.&lt;/font&gt; In nec metus sit amet orci facilisis pretium.
```

and `toHtml` method returns:

```html
<p dir="ltr"><b>Lorem ipsum dolor</b> sit <u>amet</u>, consectetur <font color ="#00ff00">adipiscing</font> elit. <i>Cras ut magna eget erat fermentum dignissim.</i> Aenean interdum ultrices sapien<sup>1</sup>. <font color ="#ff0000">Vestibulum feugiat ultrices neque, vel pretium massa. Etiam tempor luctus dui eget aliquam.</font> In nec metus sit amet orci facilisis pretium.</p>
```

Be aware that the Html class doesn't support all HTML tags. Unfortunately, the documentation doesn't say which are supported: [Issue 8640: Documentation which tags are supported by Html.fromHtml()][issue_8640]. Thanks to Mark Murphy, you can find a list of supported tags in Android 2.1 on his blog: [HTML Tags Supported By TextView][html_tags_supported_by_textview]. Besides, using [GrepCode][html_java] you can always check it by yourself.

`fromHtml` method parses provided string so I advise not to invoke it on the UI thread when displaying longer text. Additionally, you should consider caching returned `Spanned` object (e.g. in lists).

In my next posts, I will explain how to use images and handle unsupported tags with the `Html` class.

[html]: https://developer.android.com/reference/android/text/Html.html  "Html class"
[html_taghandler]: https://developer.android.com/reference/android/text/Html.TagHandler.html "TagHandler interface"
[html_imagegetter]: https://developer.android.com/reference/android/text/Html.ImageGetter.html "ImageGetter interface"
[spans_a_powerful_concept]: https://flavienlaurent.com/blog/2014/01/31/spans/ "Spans, a Powerful Concept"
[issue_8640]: https://code.google.com/p/android/issues/detail?id=8640 "Issue 8640: Documentation which tags are supported by Html.fromHtml()"
[html_tags_supported_by_textview]: https://commonsware.com/blog/Android/2010/05/26/html-tags-supported-by-textview.html "HTML Tags Supported By TextView"
[html_java]: https://grepcode.com/file/repository.grepcode.com/java/ext/com.google.android/android/5.1.1_r1/android/text/Html.java#Html "Html java"
[tag_soup]: https://home.ccil.org/~cowan/tagsoup/ "TagSoup"
