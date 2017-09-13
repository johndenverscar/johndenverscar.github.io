---
layout: post
title: Bashrc Vs. Bash Profile
date: 2017-09-11 11:00
date: 2017-09-11 11:00
---

Dotfiles are the greatest thing ever.
They are the king of facilitating protable configuration.
Both at start up and runtime.
They can be very confusing if you don't know what each one is for though.

## The Two Kings of the Terminal

There are two key dotfiles that *nix users ought to get a hold of.
The `bashrc` and the `bash_profile`.
They are complimentary but not identical.
Both are bash scripts that are used to establish a shell environment.
When they are exectued varries.
The `bash_profile` is exectued for login shells.
While the `bashrc` is exectued for interactive shells.

### Login vs. Interactive 

For Linux and Unix shells, this is very important.
For Mac, not so much.
I explain why in the last paragraph.

When you login to a *nix box, the `bash_profile` is executed.
This is to configure your shell before the initial command prompt is opened.
Since you are physically logging in, this is considered a login shell.

If you're already logged in (i.e. inside KDE or Gnome) and you open a terminal, your `bashrc` is executed.
This is an interactive shell.

For Mac users, every time you open Terminal.app (iTerm), you're opening a login shell.
This means that both your `bash_profile` and you `bashrc` are executed.
Proceed accordingly.

### But Why Two?

Honestly, I don't know what the original creator of the idea was thinking.
Every year, around the same time, I spend a good 3 or 4 days trying to figure it out.
I almost always give in because I simply don't care enough.
However, I do know a solid use for myself!

Whenever I log into my linux computer at home, I like to run some analytics.
Simple stuff like available disk space, outdated applications, startup time, etc...
I gather all of this information and write it to a file that is timestamped.
After that, I compare the results to the previous days and out put another file that is timestamped.

All of this happens in my `bash_profile`.
It's a fairly long process as far as start up time goes, about a minute, so I don't want to do it too often.
If I had to wait a minute for every terminal I opened, I'd go nuts.
I open a lot of terminals so this distinction is valuable to me.

## What Goes Where?

Most people will tell you to use your `bash_profile` to call your `bashrc`.
I agree.
Put this in your `bash_profile`,

```bash
[[ -s ~/.bashrc ]] && source ~/.bashrc
```

All this does is calls your `bashrc` from your `bash_profile`.
After that, everything I do goes in my `bashrc`.
I do this so that I can set up identical environments whether I am on Linux or Mac without worrying about load times.

For those of you looking for an example of both files, check out some of these.

1. [The Linux Documentation Project][1]
2. [Nate Landau's Sweet Mac OS X bash_profile][2]
3. [The Ubuntu Forum's bashrc Swap][3]

[1]: http://tldp.org/LDP/abs/html/sample-bashrc.html
[2]: https://gist.github.com/natelandau/10654137
[3]: https://ubuntuforums.org/showthread.php?t=679762
