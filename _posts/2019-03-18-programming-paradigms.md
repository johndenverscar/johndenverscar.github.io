---
layout: post
title: Usefulness of Programming Paradigms
---

There is no sense in picking a programming paradigm.
Likewise, there's no sense in trying to defend one or the other.

In this article I'll be looking at Java and Clojure closely.
This is not a language specific article though.
I'm just using languages and their semantics as examples of typical b.s.

## Patterns vs Properties

Immutability is often touted as a _property_ of functional programming.
Encapsulation is often touted as a _property_ of object oritented programming.
Neither of these things are unique to a paradigm though.
In Java, you can easily follow the pattern of immutability.
Likewise, you can encapsulate behavior in Clojure.

In Java, the string class is immutable.

```Java
String greeting = new String ( "Hello World!" );
System.out.println(greeting);

greeting.replace("World", "Vietnam");
System.out.println(greeting);

String greeting2 = greeting.replace("World", "Vietnam");
System.out.println(greeting2);

// => Hello World!
// => Hello World!
// => Hello Vietnam!
```

Notice how `greeting` doesn't get mutated?
Crazy, right?
Immutability is a _pattern_ that can be implemented and followed.
You can implement is where you choose to do so.

```Clojure
(defprotocol Describable
    "This is a protocol for describable things"
    (describe [this]))

(deftype Person [first last age]
    Describable
    (describe [this]
        (str first last " is" age " years old")))

(describe (Person. "Bill" "Smith" 30))
;; => "Bill Smith is 30 years old"
```

What?
Encapsulation in Clojure?
I know right.
Crazy...

The point I'm trying to make in a very roundabout kind of way is that OOP and FP are not mutually exclusive.

## Where I use them

When I have programs with a lot of similar objects that differ in things like action implementation, I focus more on objects.
I'm hesitant to call it 'pure' OOP, but object inheritance and class heirearchy comes in to play much more quickly with things like NPCs and retational databases.
A good object structure allows for me to offload state from some central beast of a datastructure to the components individually.
React components are a great example of a domain where object orientation is ideal.
HTML (and other UI) components are almost all definable by the type of container, metadata, contents, and IO.
It allows for components to be individually defined without having to implement shared behavior multiple times.

If I have a program more heavily focused on data, then I'll opt for a more functional approach.
I'll probably have functions to coerce data into a shape that I prefer, but I usually wont build many methods on it.
If I do take an object model approach to data focused applications, I'll opt to build a monad/almost-monad for the data.
My primary focus here is data clarity and introspection.

## Summary

FP and OOP are techniques that some languages encourage more than others.
Neither are perfect but neither are incomplete.
What they affect, more than anything, is long term maintainability.
A code base that restricts assignment to constants only will probably be more predictable.
You don't need to write that code in Haskell to do that though...
Likewise, having a rigid minimum of what your data needs to be properly represented will make code significantly more readable.
You don't need to write that code in Java to do that though...
