---
layout: post
title:  "Closest x points"
date:   2015-10-18 12:00:00
categories: jekyll update
---

I needed a method that should return the closest x points to the center, given a set of points. The point object is defined as the following:

{% highlight objective-c %}
@interface Point2D : NSObject
@property (nonatomic, strong) NSNumber *x;
@property (nonatomic, strong) NSNumber *y;
@end

@implementation Point2D

- (NSString *)description {
    return [NSString stringWithFormat:@"(%@, %@)", self.x, self.y];
}

@end
{% endhighlight %}

Let's say that I have the points (1,1), (2,4) and (2,1). The 2 closest points to the center are (1,1) and (2,1). A possible approach to solve this problem is to sort the points and then return the first x points from the sorted array.

{% highlight objective-c %}
- (NSArray *)closestToTheCenterKPoints:(int)k inSet:(NSSet *)set {

    if (k == 0) {
        return nil;
    }

    NSArray *pointsArray = [set allObjects];
    pointsArray = [pointsArray sortedArrayUsingComparator:^NSComparisonResult(id obj1, id obj2) {

        float absoluteDistanceFirstObj = fabs([[(Point2D *)obj1 x] floatValue]) + fabs([[(Point2D *)obj1 y] floatValue]);
        float absoluteDistanceSecondObj = fabs([[(Point2D *)obj2 x] floatValue]) + fabs([[(Point2D *)obj2 y] floatValue]);
        if (absoluteDistanceFirstObj < absoluteDistanceSecondObj) {
            return (NSComparisonResult)NSOrderedAscending;
        }
        return (NSComparisonResult)NSOrderedDescending;
    }];

    if ([pointsArray count] < k) {
        k = (int)[pointsArray count];
    }

    return [pointsArray subarrayWithRange:NSMakeRange(0, k)];
}
{% endhighlight %}

The complexity of sorting the array is O(n log(n)), so the complexity of this solution is also O(n log(n)).
