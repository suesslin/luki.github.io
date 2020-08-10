---
layout: post
title:  "Function Composition in Swift"
date:   2018-04-05 18:51:00 +0100
categories: math programming
---

A very interesting concept in mathematics is *function composition*, which allows one to composite two functions, just as
the name implicates. In mathematical notation, one might write equations such as the following

f -> x: x * 3

g -> x: x + 4

h -> x: f . g

h(5) = 27

Summed up, that means that the x-value put into the composed function will first server the function to the right, and the
result of that will be used as an argument for the function the left.

Haskell is a programming language that is prominent for taking advantage of the latter, here's an example:

{% highlight haskell %}
multiplyThree n = n * 3
addFour n = n + 4

composite n = multiplyThree . addFour
{% endhighlight %}

So, {% highlight haskell %}composite 3{% endhighlight %} would return 13.

But how would we implement function composition in Swift? Let's have a look using playgrounds!

{% highlight swift %}
// Define precedencegroup Left
precedencegroup Left {
    associativity: left
}

infix operator ° : Left // Declare operator
func °<T, U, V>(f1: @escaping (U) -> V, f2: @escaping (T) -> U) -> (T) -> V {
    return { x in f1(f2(x)) }
}

// Example

func multiplyThree<T: Numeric>(n: T) -> T {
    return n * 3
}

func addFour<T: Numeric>(n: T) -> T {
    return n + 4
}

(multiplyThree ° addFour)(2) // Returns 18
{% endhighlight %}

The precedence group's sole use is to make our following '°' infix operator left associative. The signature of its ... function
is very interesting, let's have a look at it.

We have three generic types - T, U and V.
Our first argument, f1, takes a function that takes U and returns V. The second argument, f2,  however, takes a function
that takes T and returns U. Our returning type is a function itself, which takes T, so the argument of f2, and returns V, 
so the return type of 1. This is true, considering how function composition is explained at the top. 

Since our return type will be a function, we have to be able to insert a value, hence, do we have to work with closures,
which explains the braces after return. The value that will be put in, we call x and then put our functions together:
f1 takes f2, which takes x. 

The following two functions we define so we can test composition, and the last expression shows that it works.

We are done, and doesn't it look elegant?
