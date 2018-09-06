---
title: Strong Types vs Flexible Spec
---

With the emergence of Clojure's `spec` library, the argument against strong types now has teeth.
Sharp teeth at that.
The new specification development pattern provides a significant amount of utility to developers.
The question now becomes, is it genuinely useful or is it just more fluff?

### What's are we comparing? [WIP]


Specs and types.

What's a type?
Code

What's a spec?
Data.

What's the difference?
Code can be evaluated.
Data can be evaluated and transformed.

### When are they evaluated? [WIP]


Types?
Compile time.

Specs?
Run time.

### What can they do? [WIP]


Types?
Construct things.

Specs?
Generate data.
Conform data.
Validate data.
Combine without needing inheritance.
Explain bad data.
Construct things.



___

[NOTES]

Types vs Specs

Where do types benefit you?

Development time
    - IDE integration assists developer based on type defs
Compile time
    - Compiler will throw errors for type mismatching
NOT runtime...
NOT test time...

Pros:
    - Catch many type mismatches at compile time
    - Makes code easier to read when using IDE with good integration (VS Code, IntelliJ)
Cons:
    - quiet failures
    - more verbose and complex development patterns


Where do specs benefit you?

Development time
Runtime
    - conform
    - explain
    - validate
Test time
    - generators
NOT compile time
    - wont catch an error solely based on a spec

Pros:
    - Consisten error pattern via explain/conform
    - Simple error propogation to users
    - Built in generators of conformant/consistent test data
    - Built in runtime validation with valid? and conform
Cons:
    - No IDE integration
