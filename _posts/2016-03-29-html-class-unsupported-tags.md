---
layout: post
title: Html class - unsupported tags
summary: unsupported tags
---

As we know, some HTML tags are not supported by the `Html` class. What can we do about it? Android SDK provides a <code><a href="https://developer.android.com/reference/android/text/Html.TagHandler.html" title="TagHandler interface">TagHandler</a></code> interface that allows us to handle unsupported tags.

Now let's add `<strike />` tag to our lorem string from the previous post.
```xml
<string name="lorem"><![CDATA[<b>Lorem ipsum dolor</b> sit <u>amet</u>, consectetur <font color="green">adipiscing</font> elit. <i>Cras ut magna eget erat fermentum dignissim.</i> Aenean interdum ultrices sapien<sup>1</sup>. <font color="red">Vestibulum feugiat ultrices neque, vel pretium massa. Etiam tempor luctus dui eget aliquam.</font> <strike>In nec metus sit amet orci facilisis pretium.</strike>]]></string>
```

When we use the `fromHtml` method with one parameter and run the code, we can see that it doesn't work as intended.

![broken strike tag](/assets/images/html-class-unsupported-tags/broken-strike-framed.png)

Because `<strike />` is not supported, we have to create `TagHandler` implementation. As `ImageGetter`, this interface also has only one method which notifies us when an unsupported tag is found:

```java
public void handleTag(boolean opening, String tag, Editable output, XMLReader xmlReader);
```

It is the implementation of the `TagHandler` interface with `<strike />` support:
```java
class StrikeTagHandler implements Html.TagHandler {
    @Override
    public void handleTag(boolean opening, String tag, Editable output, XMLReader xmlReader) {
        if (tag.equalsIgnoreCase("strike")) {
            processStrike(opening, output);
        }
    }

    private void processStrike(boolean opening, Editable output) {
        int length = output.length();

        if (opening) {
            startStrike(output, length);
        } else {
            endStrike(output, length);
        }
    }

    private void startStrike(Editable output, int length) {
        output.setSpan(new StrikethroughSpan(), length, length, Spannable.SPAN_MARK_MARK);
    }

    private void endStrike(Editable output, int length) {
        Object obj = getLast(output, StrikethroughSpan.class);
        int where = output.getSpanStart(obj);

        output.removeSpan(obj);

        if (where != length) {
            output.setSpan(new StrikethroughSpan(), where, length, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
        }
    }

    private Object getLast(Editable text, Class kind) {
        Object[] objs = text.getSpans(0, text.length(), kind);

        if (objs.length == 0) {
            return null;
        } else {
            for (int i = objs.length; i > 0; i--) {
                if (text.getSpanFlags(objs[i - 1]) == Spannable.SPAN_MARK_MARK) {
                    return objs[i - 1];
                }
            }

            return null;
        }
    }
}
```

This code is from [Android: How to use the Html.TagHandler?](https://stackoverflow.com/a/4062318/759007) StackOverflow question.

When an unsupported tag is detected, the `handleTag` method is triggered. We use this signal to start and finish processing this tag.
When the `opening` parameter is `true`, we mark a start of a struck-out text by using the `setSpan` method with `Spannable.SPAN_MARK_MARK`.
Next, when the end tag is detected (`opening` is `false`), we search for the last `StrikethroughSpan` with `Spannable.SPAN_MARK_MARK` flag to find start position. Then we use `length` as our end position.
Finally, we apply `StrikethroughSpan` by calling:
```java
output.setSpan(new StrikethroughSpan(), where, length, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
```

It is how we use our `StrikeTagHandler`:
```java
TextView textView = (TextView) findViewById(R.id.lorem);
String string = getString(R.string.lorem);
Spanned spanned = Html.fromHtml(string, null, new StrikeTagHandler());
textView.setText(spanned);
```

Below we can see the result:
![working strike tag](/assets/images/html-class-unsupported-tags/working-strike-framed.png)
