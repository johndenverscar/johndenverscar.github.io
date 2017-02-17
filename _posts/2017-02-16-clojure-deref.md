---
layout: post
date: 2017-02-16 21:22
title: Clojure @
categories: [programming clojure]
---

Of all the weird symbols in the clojure language, one of my favorites is `@`.
Good ol' `deref`.
It's necessary, but for reasons that weren't intuitive to me.

The Clojure STM system is a pretty neat thing.
Most Clojure developers will wind up using atoms or refs at some point.
We're going to use one.

```clojure
(def hobbit (atom {}))

(swap! hobbit assoc :name "Bilbo")

hobbit
;;=> #atom[{:name "Bilbo"} 0xe6bf29c]

(:name hobbit)
;;=> nil
```

Oh... 

What's happening here?
We've set the value of this atom `hobbit` to be a map containing the key `:name`.
We should be able to access that like a map, right?
We should also be able to see the map as a whole by just calling `hobbit` in the REPL.
Wrong.
`hobbit` __is not__ a map. 
It's an `atom`.
That's a reference type.
We have to dereference it to see it's value.

```clojure
@hobbit
;;=> {:name "Bilbo"}

(:name hobbit)
;;=> "Bilbo"
```

OH!

Yeah.
This goes for refs, atoms, futures, promises, vars, agents, and delays as well.

Happy dereferencing!
