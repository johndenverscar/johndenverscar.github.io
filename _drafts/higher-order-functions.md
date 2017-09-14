---
layout: post
date: 2017-09-14 11:00
title: Higher Order Functions
category: clojure
---

One of the most important concepts of functional programming is higher order functions.
It is essential to the Clojure language and the functional programming paradigm.
This post will be broken into three sections;

1. What?
2. Why?
3. How?

First, we'll look at what higher order functions are.
Next, we'll examine why they might be useful.
In closing, we'll look at some real examples of using higher order functions.

## What?

In Clojure, functions are values.

Functions in Clojure can take other functions as parameters.
They can also return functions as return values.

Higher order functions are indespensible to functional programming.

We'll look at examples of higher order functions in the 'How' section.
First, we'll build our own.

### Functions as arguments

I'm going to build a function that takes two numbers, a and b.
This function will add b to a twice and return another number.

```
(defn add-twice [a b]
  (+ b (+ b a)))

(add-twice 5 10)
;;=> 25
```

Now, I'm going to do the same thing, but with multiplcation instead.

```
(defn multiply-twixe [a b]
  (* b (* a b)))

(multiply-twice 5 10)
;;=> 500
```

This seems like there's a bit of repitition, the only difference is `+` and `*`.
Maybe, we can pass those in as an argument instead.

```
(defn operate-twice [operation a b]
  (operation b (operation b a)))

(operate-twice + 5 10)
;;=> 25

(operate-twice * 5 10)
;;=> 500
```

Now that's better!
We just wrote a higher order function that takes a function as an argument.
This allowed us to make our `twice` functions more generic. 

### Functions Returning Functions

Here, we're going to build functions to greet different species.
First humans, then dogs, than birds.

```
(defn greet-human [name]
  (str "Hello " name))

(defn greet-dog [name]
  (str "Woof woof " name)

(defn greet-bird [name]
  (str "SQWAAAAAAK " name))

(greet-human "Jacob")
;;=> Hello Jacob

(greet-dog "Sparky")
;;=> Woof woof Sparky

(greet-bird "Norman")
;;=> SQWAAAAAAK Norman
```

I'll admit, I don't speak bird.

This seems terribly inefficient though.
Let's see if we can make a function to make our greeting functions for us.

```
(defn make-greeter [greeting]
  (fn [name]
    (str greeting " " name)))

(def greet-human (make-greeter "Hello"))
(greet-human "Jacob")
;;=> Hello Jacob
```

With this `make-greeter` function, we can create all of the old ones while insuring consistency.

## Why?


## How?

1. Filter
2. Map
3. Reduce
4. Partial
5. Middleware
6. Dependency Injection
7. Testing









