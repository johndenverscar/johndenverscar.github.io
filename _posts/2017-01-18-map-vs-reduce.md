---
layout: post
title: An Introduction to Map, Filter, Reduce
date: 2017-01-18 22:23
---

Higher order functions are a powerful tool for functinoal programmers.
The idea of a higher order function has given rise to three especially useful functions.
Enter `map`, `filter`, and `reduce`.

## Map: Transform Data
- Input: Colletion and unary function
- Output: A collection, equal in size to the original collection
- Uses: One-to-one transformation of a collection

In Clojure, `map` is used to transform each element in a collection.
Map takes a unary (one argument) function and a collection as input.


Given a unary function, `f`, and a collection `C = [v0, v1, ..., vN]`, 
`(map f C)` will return `[(f v0), (f v1), ..., (f vN)]`.

For instance, lets pretend we have to apply a curve to exam grades,

```clojure
(def grades [72 68 52 70 81 63])

(defn curve
  [grades extra-points]
  (map #(+ extra-points %) grades))

(curve grades 10)
;;=> [82 78 62 80 91 73]

; Awful color scheme
```

## Filter: uh... Filter Data
- Input: Collection and predicate function
- Output: A subset of the original collection
- Uses: To remove unwanted elements of a collection

Filtering a collection is a common operation in any kind of programming.
Clojure (any many other languages) provide to you a function, `filter`, to do just this.
As inputs, you provide a predicate function and a collection.
The return will always be a subset of the original collection.

Lets say we have a list of names but we only want names starting with "S",

```clojure
(def names ["Andy" "Jimmy" "Sarah" "Sam" "Zach"])

(def s-names
  (filter #(clojure.string/starts-with? names "S")))
  
(prn names)
;;=> ["Sarah" "Sam"]
```
Filter is very straight forward in what it's intended use is and how it's used.
As long as you understand the idea of higher order functions, filter comes easy.

## Reduce: Accumulate Data
- Input: Colelction and combining function
- Output: Recombination of original data
- Uses: To Aggregate, accumulate, or otherwise transform data

`Reduce`, also known as `fold`, is primarily used to recursively recombine a collection of data.
`Reduce` is the most flexible of of the three functions in this article.
In fact, `map` and `filter` can be defined in terms of `reduce`.

```clojure
(def my-map
  [f col]
  (reduce (fn [acc v]
            (conj acc v))
    []
    col))
    
(def my-filter
  [p col]
  (reduce (fn [acc v]
            (if (f v)
              (conj acc v)))
    []
    col))
```
If you have a keen eye for laziness, you'll notice that something is off.
While `map` and `filter` are both known to be lazy, `reduce` isn't.
Our new `my-map` and `my-filter` functions are both eager.
This isn't necessarily a bad thing in all problem domains.
However, since `map` and `filter` are typically used on __large__ data sets, laziness is good.
I'll explain why in a later post.

__But what do we do with this thing?__

```clojure
(defn my-sum
  [nums]
  (reduce + nums))

(def costs [100.10 45.70 255.45])

(my-sum costs)
;;=> 401.25
```

Fun fact, this is how `+` is defined in Clojure
This is actually how many mathematical, collection related functions are defined.
(i.e. `+`, `-`, `*`, `max`, `min`, etc...)
Not quite this simplistic, but kind of close.


## Review

`Map`, `filter`, and `reduce` are staple, higher order functions.
`Reduce` can be used to define a wide variety of data related functions.
`Map` excels at surjective (onto) transformations.
`Filter` is a very useful when you need to refine a set of data based on a predicate like `odd?`.
