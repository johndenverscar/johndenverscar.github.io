---
layout: post
title: Map, Filter, Reduce
date: 2017-01-18 22:23
---

Higher order functions are a powerful tool for functinoal programmers.
The idea of a higher order function has given rise to three especially useful functions.
Enter `map`, `filter`, and `reduce`.

# Map: Transform Data
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

# Filter: uh... Filter Data
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

# Reduce: Accumulate Data

The grandfather of higher order functions.
Just about any data related operation can be expressed in terms of `reduce`.
It's incredibly powerful and flexible.
This comes at the cost of complexity.










