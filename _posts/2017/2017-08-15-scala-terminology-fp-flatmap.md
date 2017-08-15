---
title: Scala, Learning FP By Terminology - confused oriented flatMap
permalink: /scala-by-fp-terminology-flatmap/
---

# flatMap

**Confused oriented programming and flatMap**

`flatMap` is one of the most major core concepts and terms in scala and in FP terminology in general.  In addition, flatMap appears to have an amazing tendency to mix developers up.  The reason is obvious, the term flatMap is confusing, when you look at the signature of the function it's not really clear what it's doing and the different from `map`.  In addition the example's provided are mostly complex and confusing by themselves.  In this chapter we are going to discuss flatMap and try to dig in and get to know it from all directions.

`flatMap` reminds me in many ways of a magician pulling out a rabbit out of his hat.  In FP you concatenate or sequence function evaluation one after another.  When you do this you can end up with `Hat[Hat[Hat[Rabbit]]]` or `[Box[Box[Box[Cat]]]]` obviously nor we want that nor the cat or the rabbit.  Therefor just as if the magician pulls out the rabbit out of the hat and does not pulls out a Hat[Rabbit] out of his hat, the magician uses the flatMap trick for that, or he pulls out the flatMap rabbit.

**What I don't like about flatMap explanations**

If there is one thing I dislike about googling results of flatMap explanation is that almost all of them go directly to an explanation like this: "flatMap is like a map with list".  I really don't like this explanation.  I don't like it because it doesn't talk about the essence of flatMap, the concept.  I think the conceptual idea behind flatMap is much larger than this one, and there is also a problem with flatMap signature, not that it should not have it's current signature, i just think there is much more room for a more concise explanation of this term.

If you actually go ahead and checkout the "Programming In Scala" great book you would see it says:

> You see that where map returns a list of lists, flatMap returns a single list in which all element lists are concatenated.

I think this can also misguide you because I think the essence of flatMap is different.

**flatMap-Confused developer's unite!**

First let me tell you this, if you don't find `flatMap` confusing please stop reading this article you already know or think you know `flatMap`.  I'm writing for the confused developer not for the non-confused one.  Frankly, I can tell you that I can take off 2 years off my life now and fill them up with thinking about flatMap together with a beer.  And if that won't be enough i'll extend it with a few years.

So for the `flatMap-Confused` developers, I would like start by checking if we should be embarrassed that we are confused about flatMap or not.

The great and short :) blog post: **[Scala's flatMap is not Haskell's >>=](http://igstan.ro/posts/2012-08-23-scala-s-flatmap-is-not-haskell-s.html)** Tell us a great story, scala's flatMap so claims the author is not haskell's >>=.  Is that important you ask? yes, let's first try to understand what haskll's` >>=` is.

For that purpose we move on and ask our `haskell` oracle (not the db), what the hell is `>>=` in haskell?  The answer from the book **[Learn You A Haskell For A Great Good](https://devatrest.blogspot.com.il/2017/08/book-review-learn-you-haskell-for-great.html)** which is an amazing book and it says, well, first it shows the definition of `>>=` which is:

```haskell
(>>=) :: (Monad m) => m a -> (a -> m b) -> m b
```

A Causion word about "box"


It talks about monad and we didn't explain yet what monad is so for our purpose whenever you hear now monad please think a `box`.  But the book warns against the box analogy.
 
> A word of advice. Many times the box analogy is used to help you get some intuition for how functors work, and later, we'll probably use the same analogy for applicative functors and monads. It's an okay analogy that helps people understand functors at first, just don't take it too literally, because for some functors the box analogy has to be stretched really thin to still hold some truth. A more correct term for what a functor is would be computational context. The context might be that the computation can have a value or it might have failed (Maybe and Either a) or that there might be more values (lists), stuff like that. 
 
 So, the it's about the `box` that contains a value and operations definition.  The book specify that it sometimes better to treat the `box` as a `computation context`, that is the context in which we run our computation, and the monad is that "thing" which stores this context.

Let's decompose it, you don't need to understand it all, just follow me:

1. `>>=` - The name of our function it's going to be `>>=` and may i add `:)`.
1. `=>` - A class constraint meaning, `m` is a bloody `monad`.
1. `m a -> (a -> m b) -> m b` - This is the body of our function
1. `m a` - The argument to the function, we are going to take first an `m a` or a `monad of a`
1. `-> (a -> m b)` - We are going to peak into `m a` into the `a` and then apply a function which will evaluate to `m b`  this is the body of the, what we see here is the second argument the function which converts `a` into `m b`
1. `m b` this is just the output of our `>>=`.

Now the book **[Learn You A Haskell For A Great Good](https://devatrest.blogspot.com.il/2017/08/book-review-learn-you-haskell-for-great.html)** moves on and says:

> If we have a fancy value and a function that takes a normal value but re- turns a fancy value, how do we feed that fancy value into the function? This is the main concern when dealing with monads. We write m a instead of f a, because the m stands for Monad, but monads are just applicative functors that support >>=. The >>= function is called bind.

Let's explain