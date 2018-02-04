---
layout: post
title:  "Imitating Haskell's List Pattern"
date:   2018-02-04 13:43:00 +0100
categories: programming haskell swift
---
## Note
Before reading, note that prior knowledge of Haskell or functional programming is not required. 

## Article
Recently, I’ve been looking into Haskell and particularly one feature appealed to me while using it, which is (x:xs) in the pattern matching of lists, where x is the current element, the so called ‘head’, and xs the rest of the list, the so called ‘tail’. Here is an example of writing a length method for lists in Haskell:

{% highlight haskell %}
-- The signature for length’
-- The method/function takes a list of any type and returns an Int (the length)
length’ :: [a] -> Int
-- If the input is an empty list, return 0
length’ [] = 0
-- If the input is one element, return 1
length’ [a] = 1
-- Return 1 and add what results in using length on the rest of the list
length’ (x:xs) = 1 + length’ xs
{% endhighlight %}

I asked myself, whether or not it’s useful to implement a similar “feature” in a programming language like Swift.
Here is what I thought of:

{% highlight swift %}
extension Array {
  func recurse() -> (x: Element?, xs: [Element]) {
    return (self.first, self.dropFirst().map { $0 })
  }

  func length() -> Int {
    if self.isEmpty { return 0 }
    if recurse().x == nil { return 1 }
    return 1 + self.recurse().xs.length()
  }
}
{% endhighlight %}

As you can see, our two methods are extensions on the type “Array”.

The first one, which I call - because I couldn’t come up with a better name - recurse, returns a tuple made of an optional Element called x, and the rest of the array called xs. X is optional, because the point where there is no actual Element left might come. Its body is pretty self explanatory, self.first is the current head, and self.dropFirst will return the “tail”. Note that the return type of dropFirst is an ArraySlice and not an Array, hence the map-method.

Now with Haskell’s feature imitated to some extent, its time implement length. The general scenario is to return 1 and then use length on the tail of the current “iteration”. That’s what the 9th line does. The two lines above it are the cases in which the list is empty and returns 0, or the head element isn’t existent, meaning it equals null and returns just 1.


This implementation works fine and does appeal to me in some way, though not as much as it does in Haskell. Myself, I don’t really see an advantage of approaching recursion in a language like Swift this way, neither do some of my friends who know Swift. It is, however, a design-pattern that might appeal to some.
