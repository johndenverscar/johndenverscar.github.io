---
layout: post
title: GraphQL: The Right Way
date: 2017-08-14 21:00
categories: [clojure]
---

GraphQL is a great tool, but there are a ton of 'gotchas' you'll find.
I'm going to do my best to help you to avoid them.

### Lacinia. Not Alumbra

There are 3 libraries in Clojure used for GraphQL.

1. Lacinia
2. Alumbra
3. Graphql-clj

[Graphql-clj][1] was the first one out, I think.
It gets the job done, for sure.
I just don't understand, well enough, the thought process behind the design.
The design and my brain clashing make me terribly unproductive.
If you like it, go for it!
It's not for me though.

[Alumbra][2] was the first library that I used for GraphQL.
It's based off of the Claro library, built and maintained by the same guy who made Alumbra.
There is a lot of good stuff in this library, error messages and explicitness aren't in that list.
I've run into a bunch of bugs using it because of the 'magic' that happens behind the scenes.
It's become a bit of a sticking point for me.

[Lacinia][3] came bursting onto the scene when Walmart Labs decided to take it public March 2017.
It's a great library, but not completely without faults.
Lacinia's schemas are written in edn, which is great.
However, most GraphQL schemas are written in the standard JSON-like format as a `.schema` file.
Starting with Lacinia is awesome, but they don't have a tool yet to translate schemas to edn yet.
No big deal, we're starting from scratch anyways!

### Start with the Schema

The name of the game here is simplicity.
Down to the very name of the queries we create.
Think of GraphQL as a data store rather than an API.

We need something to build an API around.
I'm looking for a new car now and I need some help.
I have a database of car data and a rating system.

```sql
create table cars (
  id serial primary key,
  make varchar,
  model varchar,
  body_type varchar,
  fuel_eff varchar,
  crash_rating real,
  miles int,
  price int
);
```

This is a fairly simple table, and we're going to be building a fairly simple schema around it.

```edn
{:enums {}
 :objects {}
 :queries {}
 :mutations {}}
```







[1]: https://github.com/tendant/graphql-clj
[2]: https://github.com/alumbra/alumbra
[3]: https://github.com/walmartlabs/lacinia
