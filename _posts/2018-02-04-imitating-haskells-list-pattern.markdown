---
layout: post
title:  "Imitating Haskell's List Pattern"
date:   2018-02-04 13:43:00 +0100
categories: programming haskell swift
---
## Note
Before reading, note that prior knowledge of Haskell or functional programming is not required.

## Article
Recently, I’ve been experimenting with Haskell and am particularly fond of one feature - the (x:xs) in the pattern matching of lists, where x is the current element, the so-called ‘head’, and xs the rest of the list, the so-called ‘tail’.
The following is an example that incorporates the mentioned feature, a length method for lists:

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

That being said, I started wondering whether implementing this feature in another language would be useful or not. The next code snippet is an attempt of "imitating" it in Swift:

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

As you can see, these two methods are extensions of the type “Array”.

The first one, which I call - because I couldn’t come up with a better name - recurse, returns a tuple made of an optional Element called x, and the rest of the array called xs. X is optional because there might be no _actual_ Element left at some point. Its body is rather self-explanatory: self.first is the current head and self.dropFirst will return the “tail”. However, the return type of dropFirst is an ArraySlice and not an Array, hence the map-method.

Now with Haskell’s feature imitated to some extent, its time to implement length.
The general scenario is to return 1 plus what results in using length on the tail of the current “iteration”, see example 2, l. 9. The two lines above it are the cases in which the list is empty and returns 0, or the head element isn’t existent, meaning it equals null and returns just 1.

This implementation works fine and does appeal to me in some way, though it isn't as elegant as the original in Haskell. Myself, I don’t really see an advantage of approaching recursion in a language like Swift this way, neither do some of my friends who know Swift. It is, however, a design-pattern that might appeal to some.
