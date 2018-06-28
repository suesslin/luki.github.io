---
layout: post
title:  "Functor and Applicatives in Haskell - An Introductory Explanation"
date:   2018-06-28 16:45:00 +0100
categories: functor applicative haskell functional-programming
---
Written by Lukas A. Mueller

## 1. Preface
Functors and Applicatives, two names which a lot of people wanting to enter functional programming have envisioned to conceal intimidating concepts. This text shall serve as a simple introduction that, at the very least, allays fears. Some advocates of functional programming may describe what follows as superficial, but that is its entire purpose — to explain in simple and comprehensible terms. Additionally, I’d like to clarify that I am not, by any means, a very experienced functional programmer or user of Haskell, however, I have been trying to include functors and applicatives in my programs. Therefore is this very introduction based on personal experience.  

## 2. Functors
### 2.1. Introduction
In order to introduce functors, it may be interesting to know that they derive from [category theory](https://en.wikipedia.org/wiki/Category_theory). But what only are they useful for? Well, to put it simply, functors allow one to _map_ a function over _hidden values_, e.g. in Haskell, I could add 20 to an integer ‘wrapped’ in a _Maybe_ data type, or 20 to each element in a list, if there exists an instance of the Functor type class for each named data type respectively. 
Instances of Functor requires an implementation of a function called _fmap_, its [type signature](https://www.haskell.org/hoogle/?hoogle=fmap) satisfying what follows.
```haskell
fmap :: Functor f => (a -> b) -> f a -> f b
```
As you can see, you supply any function that transforms a to b, as well as any value a ’contained’ in a data type for which the functor type class is implemented. Your result then is the very same data type as in the second argument, but with the function in the first argument having been applied on its contained value. 
But what is the advantage, when one could _map_ a function over values other ways too, for instance with the function _map_? The answer lays in that functors are more ‘generalized’, which will turn out to be true, if you look it up on hoogle. You’ll see, _map_ would require one to implement it for multiple data types, while fmap works for any data type for which the functor type class has been implemented. Also, for those who favor infix operators, _\<$\>_ can be used in place of _fmap_. Naturally, its [type signature](https://www.haskell.org/hoogle/?hoogle=%3C%24%3E) is the very same as the one of fmap.
### 2.2. Examples
In order for functors to be understandable, I’ll give two examples each for both implementing an instance of Functor for a data type and using it.
#### 2.2.1. Implementation of Instances
Since Functor has been implemented for Maybe already, let’s invent a new data type that resembles Maybe.
```haskell
data Optional a = Some a
                | None
```
An instance of Functor can look like as follows.
```haskell
instance Functor Optional where
  fmap f (Some a) = (Some . f) a
  fmap _ None = None
```
Since None holds no value, we can just ignore the supplied function, the rest is self-explanatory. 
The second example is the Either data type, but again, implemented by us.
```haskell
data Choice b a = This a
                | That b
```
This takes two values, hence do we have to solve this by supposing one value is set (like we did for _Optional_ , and the other is not). Therefore, the implementation looks like this
```haskell
instance Functor (Choice a) where
  fmap f (This a) = (This . f) a
  fmap _ (That b) = That b
```
#### 2.2.2. Usage
First, let’s do something that will be very intuitive, applying a function to a value contained in a Maybe.
```haskell
import Data.Char (intToDigit)
intToDigit <$> Just 9
```
In this example, we map intToDigit - which would usually turn a single digit int to a char - over Just 9, and as a result obtain Maybe containing our result:
```haskell
Just '3' 
```
Moving on, let’s multiply each element in a list of integers with three. That can look like this, 
```haskell
((*) 3) <$> [1,2,3,4,5]
```
with partial application or to make it more readable for some,
```haskell
(\n2 -> (*) 3 n2) <$> [1,2,3,4,5]
```
 using lambda calculus. Our result will be the following.
```haskell
[3, 6, 9, 12, 15]
```
## 3. Applicatives
### 3.1. Introduction
Having had a look at functors, and now knowing that they enable one to map a function over a ‘container’, how can we apply a function in a ‘container’ to a value in a ‘container’ of the same type? That problem is solved by Applicatives. This type class requires two functions, _pure_ with its signature being:
```haskell
pure :: Functor f => a -> f a
```
, and the operator _\<\*\>_, with its signature being:
```haskell
(<*>) :: Functor f => f (a -> b) -> f a -> f b
```
. With this said, _pure_ takes any value and wraps it around a data type, that is an instance of Functor. Furthermore, the infix operator _\<\*\>_ takes a function that transforms an a to a b, that is wrapped in a data type, for which the Functor type class is implemented, a value wrapped in the same 'container' as the function, and returns a value on which the supplied function has been applied on, again, wrapped in the same 'container' as the function. 
### 3.2. Examples
In order to explain Applicatives, I’ll give examples that help understanding it. For one who has got a grip of functors, it shouldn’t be difficult to do the same for Applicatives.
#### 3.2.1. Instances
```haskell
instance Applicative Optional where
  pure = Some
  -- or
  pure a = Some a
  -- or
  pure = \a -> Some a
  (<*>) (Some f) a = f <$> a
  (<*>) None _ = None
```

```haskell
instance (Applicative Choice a) where
  pure = This
  (<*>) (This f) a = f <$> a
  (<*>) _ (That b) = That b
```
#### 3.2.2. Usage
Having imported intToDigit from  _Data.Char_, a function called intToDigitImpure wraps the firstly named function in a Maybe, making it eligible for testing it with the newly introduced operator.
```haskell
intToDigitImpure = Just intToDigit
-- or
intToDigitImpure = pure intToDigit :: Maybe (Int -> Char)
```
Now, it may be interesting to see what happens with an integer that is contained in a Maybe.
```haskell
intToDigitImpure <*> Just 3
```
This will return a result of the following.
```haskell
Just '3'
```
## 4. Conclusion
Hopefully, you do now understand how useful Functors and Applicatives are and how efficient they are when you need to work with data types a lot. If there are any questions or if you’d like to share some feedback, feel free to [send me a mail](mailto:lukasamueller@icloud.com).
