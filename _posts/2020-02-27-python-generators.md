---
layout: post
title: Python Generators - My Notes
date: 2020-02-27 10:00
---

Generators facilitate lazy evaluation in Python.
They can be used to reduce computaional stress as well as memeory requirements.
Python sucks a little bit less because of them.

### What is a `generator`

Lazy evaluation.
That's it, don't think too deep into it.
They're not like Clojure generators that are used to create _test_ data.
In Python, like Rust, generators are used to create resumable functions that resemble syntactic closures.
The scope of a generator is limited to the scope of the function itself.

### What do they look like?

Let's start with what they replace.

```python
def fib(n):
    a, b = 0, 1
    result = []
    for i in range(n):
        result.append(b)
        a, b = b, a+b
    return result
```

Run this with a big number and watch your terminal lock up.
As the loop continues on, the available memory on your computer decreases since all of the values are being eagerly stored in that list.
This won't work, but you still need those values one at a time somewhere else.
Use a generator...

```python
def fib():
    a, b = 0, 1
    while 1:
        yield b
        a, b = b, a+b
```

Now, you can pull these values, one at a time, without locking up your terminal.

### Better example

Now that we've made it past the idea of lazy evaluation, let's look at something useful.
We're going to pull data from a very large (10 million rows) database table.
I'm tired and don't feel like explaining so if you have a question, email me.

```python
def TickDataGenerator(cursor, batch_size=None):
    batch_size = 1000 if batch_size is None else batch_size

    while True:
        res = cursor.fetchmany(batch_size)
        if not res:
            break
        else:
            for r in res:
                yield r
```

Congratulations, you've invented pagination in database queries.
That's practically fire to programmers.


### Other stuff

Tired, lazy, look at this if you want to.

- [PEP 255 - Generators][1]
- [Python Wiki on Generators][2]
- [Google it][3]


Have fun

-----
[0]: https://www.python.org/dev/peps/pep-0255/#motivation
[1]: https://www.python.org/dev/peps/pep-0255/
[2]: https://wiki.python.org/moin/Generators
[3]: https://duckduckgo.com/?q=python+generators&t=ffab&atb=v192-1&ia=web
