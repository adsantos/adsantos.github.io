---
layout: post
title:  "Right-to-left alignment"
date:   2015-11-13 17:17:00
categories: jekyll update
---

In the [apple documentation] to support right-to-left languages, it says right at the beginning:

``
If you use base internationalization and Auto Layout, most of the user interface will appear mirrored automatically for you.
```

I didn't bother continue reading, so I thought that would do all the magic. However, a few more steps were required. The complete list is the following:
1 - Ensure all the views use auto-layout.
2 - In the leading auto-layout constraints, the first item has to respect the language direction (it's the default).

![Respect language direction](/assets/respectLanguageDirection.png)

3 - All the labels have to be naturally aligned.
{% highlight objc %}
self.label.textAlignment = NSTextAlignmentNatural;
{% endhighlight %}

4 - The disclosure indicator is pointing in the wrong direction, in iPhone iOS9+. Since iOS9, the iPhone navigates from right to left instead of left to right, if the device's language is right-to-left. A possible approach to solve this is to make the disclosure indicator an image view. Then we need 3 checks: check if the selected language is right to left, what's the current iOS version and if we are on an iPhone. The following methods can be added to categories:

{% highlight objc %}
+ (BOOL)isLanguageLayoutDirectionRightToLeft
{
    return [UIApplication sharedApplication].userInterfaceLayoutDirection == UIUserInterfaceLayoutDirectionRightToLeft;
}

+ (BOOL)isCurrentIOSVersionGreaterThanOrEqualToVersion:(NSString *)iOSVersion
{
    return [[[UIDevice currentDevice] systemVersion] compare:iOSVersion options:NSNumericSearch] != NSOrderedAscending;
}


+ (BOOL)isPhone
{
    return UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPhone;
}
{% endhighlight %}

Finally, we rotate the disclosure indicator image view, if necessary:

{% highlight objc %}
if ([UIApplication isLanguageLayoutDirectionRightToLeft] && [UIDevice isCurrentIOSVersionGreaterThanOrEqualToVersion:@"9.0"] && [UIDevice isPhone])
{
    self.disclosureIndicatorImageView.transform = CGAffineTransformMakeRotation(M_PI);
}
 {% endhighlight %}

 Those were all the steps I had to follow to support right-to-left alignment.

[apple documentation]: https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html
