---
layout: post
title:  "Is it a palindrome?"
date:   2015-11-07 17:55:00
categories: jekyll update
---

I needed a method to check if a given string was a
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
