---
layout: post
title: Evolving GraphQL Resolvers
date: 2017-12-15 12:00
---

GraphQL and Lacinia.
Voodoo, magic, or engineering briliance.
You decide!
No matter what though, you'll probably wind up using it one day.

## Warning

I'm not teaching you anything about GraphQL.
I'm not teaching you much about Lacinia.
Here's some stuff if you want to learn though! :)

1. [GraphQL Documentation](http://graphql.org/learn/)
2. [GraphQL Technical Specs](http://facebook.github.io/graphql/October2016/)
3. [Lacinia Tutorial (Work in progress)](http://lacinia.readthedocs.io/en/latest/tutorial/index.html)
4. [Lacinia Documentation](http://lacinia.readthedocs.io/en/latest/overview.html)
5. [Lacinia Pedestal Documentation](https://github.com/walmartlabs/lacinia-pedestal)

## Step 1 - Dumb Resolvers

My initial thought with creating the resolvers was, I'm going to make this as small as possible.
I did literally nothing to validate input or output.
I did literally nothing to handler errors.
I did literally nothing to coerce data.
What came from it was a dumb resolver.

```clojure
(def resolve-thing
  [context args _value] ;; args I don't use in the fn start with an underscore
  (db/get-thing args))
```

That was it.
If all of my args and DB data we're good, then it worked.

But if it wasn't...

`Internal Service Error:`

What?

Yeah, that's all you get...
Time to dig through stack traces to find out that a query was returning nothing because I screwed up seed data.

__DOPE.__

## Step 2 - Less Dumb but Still Pretty Dumb Resolvers

In come union types because of a lack of understanding of Lacinia.

```
{:objects
 ...
 :Error
 {:fields {:message {:type String}}}

 :unions
 {:Thing_union
  {:members [:Thing :Error]}}

 ...}
```

Yeah, I made my own error object because I _really_ didn't understand Lacinia.
It's okay though, because at least I have the flexibility to work with errors!
This is a solid improvement from 'Internal Server Error' I think, no matter how un-idiomatic the code.

Now, let's fix up the resolver.

```clojure
(require '[com.walmartlabs.lacinia.resolve :refer [tag-with-type]])

;; I created this to keep the error maps uniform
;; Just in case I wanted to add to it in the future
(defn ->error [message]
  {:message message})

(defn resolve-thing
  [context args _value]
  (let [thing (db/thing args)]
    (if (empty? thing)
      (tag-with-type (->error {:message "Thing doesn't exist"}) :Error)
      thing)))
```

Alright!
Now, if the data doesn't exist, we don't get internal server errors or any of this random `null` nonsense.

However much of an improvement this is, I still don't like the union types.
It seems like extra stuff because of ignorance.
Let's see what we can do...

## Step 3 - Using Lacinia!

I don't know what I'd do without @hlship and @guy on the #graphql channel.
Literal life savers.

<3

One day, Howard dropped this code snippet on my life.

```
(require '[com.walmartlabs.lacinia.resolve :refer [resolve-as]])

(...
  (let [thing (db/thing args)]
    (if (empty? thing)
      (resolve-as nil {:message "Thing does not exist!"})
      thing)))
```

You know what that does.
IT PROPOGATES ERRORS TO THE CONSUMER VIA LACINIA! REAL ERROR MESSAGES!
It's happening! It's all happening!!!

What did this give me?
Happiness.

I got to remove all of my union types, all of the `tag-with-types`, and reduce the error code to,

```
(defn resolve-error [message]
  (resolve-as nil {:message message}))
```

If you haven't noticed, my errors only contain a message.
That's a personal thing though.

Now here's a scary thought, what if my data isn't valid?
Crap...

## Step 4 - Spec Everything.

I had no data validation so time to implement some.
What better to use than good ol' `clojure.spec.alpha`?

```clojure
(require '[clojure.spec.alpha :as s])

(s/def :thing/a string?)
(s/def :thing/b number?)
(s/def ::thing (s/keys [:thing/a :thing/b]))

(defn resolve-thing
  [context args _value]
  (let [thing (db/thing args)]
    (cond
      (empty? thing)
      (resolve-as nil {:message "Thing DNE"})

      (= (s/conform ::thing thing) :clojure.spec.alpha/invalid)
      (resolve-as nil {:message (s/explain-str ::thing thing)})

      :default
      thing)))
```

Oh yeah, that's it...
That's the stuff...

This thing is bullet proof-ish.

Maybe not bullet proof, the args could use validation too, but it's pretty damn good!

Let's do a quick step-by-step resolver comparison.

|                 | Step 1 | Step 2 | Step 3 | Step 4 |
|-----------------|--------|--------|--------|--------|
| resolves        | mostly |   yes  |   yes  |   yes  |
| reports errors  |  none  |  some  |  some  |   yes  |
| check empty?    |   no   |   yes  |   yes  |   yes  |
| validate otuput |   no   |   no   |   no   |   yes  |
| validate input  |   no   |   no   |   no   |   no   |

In step 1, we had a resolver that could only resolve if the input and DB data are both perfect.
That's pretty fragile.
If something is wrong, you wind up with all sorts of nulls and cryptic errors sending you to stack traces.
You never want to have to check a stacktrace for a production application.

Our improvements from steps 2 and 3 led us siginificantly better resolvers by utilizing error objects.
Whether a union type or a Lacinia error, it was a major improvement because it's less time digging through stack traces.
That's a little more solid than before.

Once we get to step 4, we have a fairly strong resolver.
Perfectly bulletproof?
Absolutely not.
There's still a lot of room for improvement.
However, we are now utilizing spec and Lacinia errors to validate output and propogate readable errors to the consumer.
This. This is good.

## Next steps

First and foremost, I have to make these things more easily testable.
That will probably require some function break out and the creation of some execution pattern.
Give me some time in the hammock and I'll get back to you.

After that, more specs!
I need to validate input.
That's just as important as validating output.

Then, as a final move, expanding specs.
Using plain predicates is a good start, but for things like email addresses, you want more detail.
I'll be adding that in last though.
For now, we'll be fine.

#### Gripes?

__TOO DAMN BAD!!!__

Nah, I'm just kidding...
@amday on the Clojure slack.
I'm online a lot and float around #graphql and #beginners.
Hit me up!
