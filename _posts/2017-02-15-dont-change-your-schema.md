---
layout: post
date: 2017-02-15 22:00
title: Don't Change your Schema
category: programming
---

Relational databases are fickle creatures.
They are pretty alright at what they do as long as you just let them do it.

Stuart Halloway made a ton of great points in [The Ten Rules of Schema Growth][1].
Although this is specifically about Datomic schemas, it applies to others pretty well, I think.
The ideas of it at least.
The points I want to talk about are 2 and 5.

> 2. Grow your schema, and never break it
> 5. Never remove a name

Recently, I've experienced the fallout of both.
In both a testing environment __and__ a production environment.

Your data is the foundation of your application.
Without your data, you have nothing to display.
If data is the foundation, then your schema is the corner stone.
Crack the corner stone, and the building crumbles.
How fast depends on the size of the crack.

Recently, our schema was changed ever so slightly.
We had a column name changed.
Literally 2 letters.
Those two letters cost one of our developers about 30 hours.
Think about that for a second.

Changing the name of a column is pretty simple if you disregard the related code.
Good code bases with a proper unit test suite will catch these changes automatically.
Bad code bases, without proper automated tests will not.

We don't have unit tests for a specific portion of our code base, just psudo tests.
And I'm the only one that does them.
Or runs them.
Ever.

When this change was made, our web service integration API was broken.
One of our sequencers no longer worked because it was trying to work with a non-existent column.
This break caused issues for dozens of product integrations for a number of clients.
A bunch of the occurances of the column were fixed at first, except for the integration layers.
This break has been a thorn in our side for almost 4 months now.

Moral of the story:
1. You need unit tests
2. If you have unit tests, make more
3. Don't change your schema

[1]: http://blog.datomic.com/2017/01/the-ten-rules-of-schema-growth.html

