---
layout: post
title: nohup vs &
date: 2019-10-30 10:00
---

I've gotten a lot of questions in my time about firing off a long running process in an ssh session and having them stop after logging out.
This is because every command you run in a bash sessions (via your local terminal or ssh) has an associated hangup signal.
When you log out of the sessions (i.e. close your terminal or kill an ssh session), the shell will terminate all of the sub-commands associated.
This can be problematic for people who want to log in, run a script to start a server, and log out.

For those who need a more robust way of doing this, I'd strongly suggest creating a systemd service unit.
With this, you'll be able to start, stop, restart, and monitor the process via `systemctl` rather than parsing through `ps` output.
However, if you're looking for a quick and dirty way to just run something, we have `&` and `nohup`.

### `&`

The ampersand, `&`, is used to run a command in the background of a shell.
It's useful in situations where you're starting a long running process in a shell and don't need to follow any output.
The problem most people run into with this is that they run a command using `&`, log out of the shell, and then their process is all of a sudden gone.
This is because running a command with the `&` doesn't prevent hangup, it just hides the process in the background.
When you do something like `./start_server.sh &` and then log out of the shell, that command is killed as a sub-command of the current shell session.

### `nohup`

`nohup` on the other hand, is used to run a command without hanging up at the end of the shell session.
It's more ideal for those who want to do something like start a server and then leave.
It's not foolproof, however,
Some processes may reconnect the hangup signal and wind up killing the process later on.
Think of `nohup` as a temporary fix in a rush as the `pid` can still be re-added to the shell's job list.

### `disown`

To be really sure that your process wont be interrupted, you'll have to `disown` the process.

```
./run_the_thing &
disown $(echo $!)
```

This will remove the process from the job list, making it safe to exit the shell.

### Systemd

If you're building something to be truly robust, I'd strongly suggest using Systemd, Upstart, or something similar.
I prefer Systemd, but that's just because it was taught to me first.
Use what you prefer, but this is a truly reliable way to fire something off and forget about it.
