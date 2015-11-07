---
layout: post
title:  "Is it a palindrome?"
date:   2015-11-07 17:55:00
categories: jekyll update
---

I need a method that should check if a given string is a
[palindrome][palindrome]. Below is a possible implementation with O(n) complexity.

{% highlight objective-c %}
- (BOOL)isPalindrome:(NSString *)string {
    NSUInteger stringLength = string.length;
    for (int i=0; i<string.length/2; i++) {
        if ([string characterAtIndex:i] != [string characterAtIndex:stringLength-1-i]) {
            return NO;
        }
    }
    return YES;
}
{% endhighlight %}

[palindrome]: https://en.wikipedia.org/wiki/Palindrome
