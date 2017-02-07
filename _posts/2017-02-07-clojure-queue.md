---
layout: post
date: 2017-02-07 17:00
title: Queues in Clojure
---

Without the idea of a first in, first out (FIFO) queue, maintaining order would be very difficult.
Or maybe, it's the converse.
Six to one, half a dozen to the other.
Either way, FIFO is necessary.

###### What is a queue

A queue is a data structue used to store and retrieve data in the first in, first out method.
An example of a queue would be a a line at the store.
The first person to get in the line is the first person to be helped.
The last person to get in line is the last person to be helped.
And when a new person is added to the line, they are added to the back.

Queues are helpful for a number of tasks, the most important of them being keeping tasks in order.
Whew!
Say that ten times fast!
The nature of FIFO is to maintain arrival order.
Like the line at the store, the first data to arrive is the first data to exit.
When computers are expected to run several process in a specific order, they need a reliable queue structure.
When you make a music playlist, you store the songs in the queue.
When your girl friend doesn't feel like moving for a day, she puts all of your chorse in a queue.
See where I'm going with this?

###### How can I work with a queue

`Peek` into the data structure.
`Pop` the first piece of data.
`Conj`oin the original queue with new data.
Check to see if it is `empty`.
Convert it into a `seq`.

Among Clojure's most famous data types are vectors and lists.
Vectors `conj` and `pop` from the tail.
Lists, from the head.
So what can we use?
Obviously, the well documented `clojure.lang.PersistentQueue`!

The Cloure `PersistentQueue` doesn't have literal reader syntax or a wrapper, so you have to create them using a Java call.
Queues, in Clojure, act exactly how you'd expect them to.
Since not having a literal can make this a pain, lets make our own.

```clojure
(defn queue
  ([] (clojure.lang.PersistentQueue/EMPTY))
  ([coll]
    (reduce conj clojure.lang.PersistentQueue/EMPTY coll)))
```
And just like that, we have a clean way of making queues. 
Now lets see how they work.

```clojure
(def numbers-in (queue))

;; We'll use `conj` to add to the structure
;; And then `seq` to look at the queues contents

(seq (conj numbers-in 1 2 3 4 5)
;;=> (1 2 3 4 5)

;; We can use `pop` and `peek` as expected

(peek numbers-in)
;;=> 1

(seq (pop numbers-in))
;;=> (2 3 4 5)

;; And `empty?` can be used to check if the queue is empty.

(empty? numbers-in)
;;=> false

(empty? (queue))
;;=> true
```
You may be wondering why we're using `seq` to look at the queue as a whole.
That's because operations on the queue return a `clojure.lang.PersistentQueue` object, not a `queue` literal.
We created a function to make queues, but we still don't have a reader literal for queues.
In order to view the object's structure in the REPL, you'll have to convert it to a data structure which does have a reader literal.
Hence, the calls to `seq`.

And there we have it, queues in Clojure!

I hope you enjoyed reading. A domani!


