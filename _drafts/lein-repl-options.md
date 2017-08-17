---
layout: post
title: Leiningen REPL Options
date: 2018-08-17 09:00
categories: [clojure, leiningen]
---

When it comes to Clojure build tools, Leiningen has been the top dog for a while now.
I've been using it for years, literally years, and have barely gone past `lein new` and `lein run`.
It's an incredible tool that I barely know how to use.
Like using Emacs but only memorizing the key bindings for copy and paste.
Instead of sticking with my vanilla project definitions and working around my 'isms', I'm going to actually learn it.
Maybe you'll find something useful in here too!

### Lein

Leiningen has all sorts of stuff that you can define and work with in the project file.
Most importantly, and I think we can all agreen on this, is the `:dependencies` key.
Without that, Leiningen would be pretty useless.
My project files are usually limited to that and whatever else `lein new <thing>` gives me.
I rarely mess with it because I never really needed to.
Not until I tried to start figwheel...

### Figwheel

For those of you who don't know, [figwheel][1] is the best thing ever.
[Bruce Hauman][2] made it because the [front end development feedback loop sucks][3].
It's an amazing tool.
But I also have literally no idea how it works.
More on that later.

What matters is that you can start a standard Clojure REPL.
Then, from within that, you can use some figwheel functions to start a ClojureScript REPL inside of it.
__Then__, you can start working on stuff.
Which is cool because once you get everything started, you have a really good workflow.
For a while though, I had no idea how to get all that going in an easy way inside of Spacemacs.
I spent a lot of time pressing `C-g` to bail out of the weirdest looking modes...

### Getting Figwheel Going the Easy Way

Paul Gouter wrote a quick article about [figwheel in spacemacs][4] in December 2016 that cleared everything up for me.
Which was great!
He gave the exact steps to start a REPL consistently and the functions to call and all that.
Prefect!
A recipe for success!

I screwed it up a lot.

### In Comes Leiningen's `:repl-options`

I don't like having to keep a bunch of references open and I don't like memorizing things.
I only have so much space in the ol' noggin.
So I went in search of a way to automate this for my ClojureScript projects.
That's when I found this [behemoth of a project.clj][5] that [Phil][6] made.

Do a quick `C-f` search for 'repl' and __BOOM!__ `:repl-options` pops up, eventually.
This looks promising.

### The Guts of `:repl-options`

There are 11 keys in the `:repl-options` of the sample project.clj that I linked above.

1. prompt
2. welcome
3. init-ns
4. init
5. caught
6. skip-default-init
7. host
8. port
9. timeout
10. nrepl-handler
11. nrepl-middleware

All of these are documented a little bit in the sample project.clj.
None of them are really advertised though.
Which is unfortunate because they are all really nice to have.
For the purposes of this article, I'm going to stick to talking about init.

### `:init`

From the docs;
```clojure
;; This expression will run when first opening a REPL, in the
;; namespace from :init-ns or :main if specified.
```

When you open a REPL to start a workflow on any project, there are always the standard namespaces to load.
For instance, if you're building a Clojure service that works with a database, you'll probably want to load `clojure.java.jdbc` every time you open a REPL.
Or maybe you're working on a web application and need a `project-name.server` namespace to start an http server.
This is where that happens.
This is also something that I now will use for every project I work on.

#### Loading in JDBC

For any project where I do stuff with a database, I stick to `clojure.java.jdbc`.
I like to just use bland SQL and all that because I'm boring.
Don't judge my libs.
You can use what you want.
But my all of my DB based projects, you'll find my `[:repl-options :init]` looks like,

```clojure
:init (do (require '[clojure.java.jdbc :as jdbc])
          ;; require all project namespaces with appropriate aliases
          (def repl-context {:db-conn " ... "
                             ... })
          ... )
```

For what it's worth, the process of requiring/defining my REPL tools, utilities, and helpers took up about 15-20 minutes of my time every time I started a REPL because I always forgot something.
I would get halfway through testing something just to get a bunch of errors because I forgot a key in my `repl-context` or I aliased a namespace differently than normal.
I start a REPL about once per day, 5 days per week.
That's 75 - 100 minutes per week.
I can do a lot in that time.
Like watch a video course on AWS.
Or maybe, write a post for my blog on how proper use of Leiningen can save you a ton of time!

#### Starting Figwheel

Watching me get started building a UI nowadays is a thing of beauty.
I know __exactly__ what to do where and how to get everything going.
Now that I have my `:init` down, it looks like I've actually done this before!

```clojure
:init (do (use 'figwheel-sidecar.repl-api)
          (start-figwheel!)
          (cljs-repl))}
```

This is now a default in any project that I use figwheel in.
Without it, starting up figwheel was something like,

1. Start a ClojureScript REPL
2. Realize it's not the correct REPL
3. Start a Clojure REPL
4. Forget to use the repl-api ns
5. Try to start figwheel by running `(figwheel-repl)`
6. Errors
7. Close that REPL and try the same thing in a CLJS REPL
8. Coffee because I'm getting tired
9. Fumble around looking for the article on how to use Figwheel in Spacemacs
10. Realize my mistakes
11. Copy and paste the proper commands into a REPL like a newb
12. Start working

For people watching this, it's painful.

NOT ANY MORE!

Now, with my handy-dandy init thing in my project.clj, I just start a Clojure REPL in Spacemacs and Leiningen does the rest!
This one saves me about 30-45 minutes per Figwheel startup (my company internet is slooooow).
And then another 15 minutes of cooling down after getting so frustrated, I'm only human.
And then anouter 5 minutes from going to get a cup of coffee because I'm tired now.

If you've never read through the sample project.clj in the Leiningen Github repo, I strongly urge you to do so.

[1]: https://github.com/bhauman/lein-figwheel
[2]: https://github.com/bhauman
[3]: https://youtu.be/j-kj2qwJa_E
[4]: https://paultopia.github.io/posts-output/figwheel-emacs/
[5]: https://github.com/technomancy/leiningen/blob/master/sample.project.clj
[6]: https://github.com/technomancy
