---
layout: post
title: Make your own Blog
date: 2020-09-27 10:00
---

It's been 7 months since my last post.

Make your own blog to read from, it'll help you learn to teach the internet.

### Step 1: Jekyll

```
$ sudo apt-get update -y
$ sudo apt-get install ruby-full build-essential -y
$ echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
$ echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
$ source ~/.bashrc
$ gem install jekyll bundler
```

Easy, right?


### Step 2: Make it look cool

Create a Jekyll project.

```
$ jekyll new username.github.io
$ cd username.github.io
$ bundle exec jekyll serve
```

Posts go in the `__posts/` directory. Sick.

### Step 3: Set Up a GitHub Pages Repo

I don't want to take screenshots so you may have to imagine some things here.

First create a GitHub repo under your username called `<username>.github.io`.
Replace `<username>` with your actual username.
My username is `admay` so my repo is called `admay.github.io`.


Now, terminal:

```
$ git remote add origin https://github.com/username/username.github.io
$ git add --all
$ git commit -m "Initial commit"
$ git push -u origin master
```
Go to https://<username>.github.io and be greeted by your code.

### Step 4: Write Stuff

It doesn't matter how scattered your posts are, just write about stuff.
I don't proof any of my posts.
I just toss a thought out there and hope it makes sense.

### /etc

```
[Jekyll]: https://jekyllrb.com/docs/step-by-step/01-setup/
[GitHub Pages]: https://pages.github.com/
[How To Ask Questions]: http://www.catb.org/esr/faqs/smart-questions.html
```
