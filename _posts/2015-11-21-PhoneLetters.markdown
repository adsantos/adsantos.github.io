---
layout: post
title:  "Possible letters from a phone keyboard input"
date:   2015-11-21 10:01:00
categories: jekyll update
---

A phone keyboard has 12 digits, where the numbers between 2 and 9 have associated letters. When we type the numbers, a set of possible combined letters will come out. If we type '2', we get 'A', 'B' and 'C'. If we type '23', we get 'AD', 'AE', 'AF', 'BD', 'BE', 'BF', 'CD', 'CE' and 'CF'. 

Let's write a method that returns all the possible combined letters for a given number typed on a phone keyboard.

The first step is to write a method that returns the possible letters for a single number. For simplicity, let's ignore the space.

{% highlight objc %}
- (NSArray *)lettersForNumber:(NSNumber *)number {
    switch ([number intValue]) {
        case 2:
            return @[@"A",@"B",@"C"];
        case 3:
            return @[@"D",@"E",@"F"];
        case 4:
            return @[@"G",@"H",@"I"];
        case 5:
            return @[@"J",@"K",@"L"];
        case 6:
            return @[@"M",@"N",@"O"];
        case 7:
            return @[@"P",@"Q",@"R",@"S"];
        case 8:
            return @[@"T",@"U",@"V"];
        case 9:
            return @[@"W",@"X",@"Y",@"Z"];
        default:
            return nil;
    }
}
{% endhighlight %}

Then let's implement the main method, which will do the following: for each input number, get the list of letters. For each letter, build a new string combining the existing strings already built and the letter. At first, there are no strings discovered, so the first strings will be just the letter itself.

{% highlight objc %}
- (NSArray *)lettersForNumbers:(NSArray *)numbers {
    NSMutableArray *letters = [[NSMutableArray alloc] initWithObjects:@"", nil];
    
    for (NSNumber *number in numbers) {
        NSArray *lettersNumber = [self lettersForNumber:number];
        
        //Ignore numbers that don't have associated letters
        if (lettersNumber.count == 0) {
            continue;
        }
        
        NSMutableArray *currentLetters = [letters mutableCopy];
        letters = [[NSMutableArray alloc] init];
        
        for (NSString *letter in lettersNumber) {
            
            for (NSString *letterInResult in currentLetters) {
                
                NSString *newString = [NSString stringWithFormat:@"%@%@", letterInResult, letter];
                [letters addObject:newString];
            }
        }
    }
    
    return letters;
}
{% endhighlight %}

Let's now calculate the complexity of the method:
- n is the total input numbers.
- m is the total number of letters, which can be 3*n or 4*n - the numbers `7` and `9` have 4 possible letters.
- The `currentLetters` are O(3^n) or O(4^n).
- The complexity of the `lettersForNumbers` method is therefore O(n * 4n * 4^n).