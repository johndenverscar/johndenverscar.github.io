---
layout: post
title: Information Overload: Starting out
date: 2017-03-23 10:50
categories: [programming clojure noobs]
---

A little more than a month ago, I wrote a post called 
[Information Overload](http://localhost:4000/information-overload/)
about how I was starting to feel overwhelmed with tech.
Since then, I've vastly improved my work situation and am glad to announce the beginning 
of a new series, __Information Overload__.
My biggest problem when it comes to software development is that there is too much information.
Processing it is difficult and leaves me exhausted sometimes.
So I'm going to make it very simple.

During the Information Overload series I'm going to walk you through a small coding project.
We're going to be building a small web application to hold onto writing samples.
My vision is to create an online notepad.

### What will we need

- data model
- UI mock up
- service to create/read/update/delete writings
- secure user sign on
- easy deployments

### How We're going to do it

- Clojure on the back end
- ClojureScript on the front
- Reagent + re-frame
- Monger + Ring + Friend
- Mongo DB
- AWS? :O

### Quick Plan

1. Establish the data model
2. Build out the back end service
3. Build out the authentication
4. Build out the UI
5. Build our easy button

### Next Steps

In the next installment of Information Overload, we're going to learn how to build a data model
and how Clojure and Mongo DB interact.
Next, we'll build a simple REST service to interact with our database.
After that we establish our project foundation, we're going to secure our user's information using
the [Friend](https://github.com/cemerick/friend) library.
Then we'll mock up our UI using raw HTML and CSS and then translate it into Reagent components.
To finish it all off, we're going to work on creating an easy way to deploy our application.
We might be diving into AWS with this one depending on how well my certification course goes!
