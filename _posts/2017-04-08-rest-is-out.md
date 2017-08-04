---
layout: post
date: 2017-08-04 11:00
title: Rest is out, GraphQL is in!
categories: [clojure]
---

First there was SOAP, and SOAP was terrible.
Then there was REST, which wasn't as bad.
Now there's [GraphQL][1], and everything is alright.

Web services exist to provide users (computers/humans) with data.
SOAP did that but it was ugly, slow, and rigid.
REST made it a little bit better by speeding it up, but thats it.
What good is a data delivery service if the service decides what data you get?

### Flexibility

GraphQL is flexible.
Rather than asking end points for everything they have, a GraphQL service is more of a data store.
You can ask the service for whatever data you want and none of what you don't.

### Lightweight

Say you have a REST service to retrieve information about users.
If you make a request to that end point, you get back megabytes of information.
That's a lot of detail, but you only wanted their age and name...
With GraphQL, since you only get the data you ask for, you cut megabytes down to kilobytes.
This is great for systems with low memory (I'm looking at you AWS).

### Explicit

One problem with REST services is that you never really know what data is available.
Not until you request it of course.
If you're looking for specific data, 
you might not be able to find it until you make requests to every endpoint.
With GraphQL comes this game changing tool, GraphIQL.
GraphIQL provides a schema browser, a mock request workspace, and autocompletion!
_It's like an IDE for your API_!
