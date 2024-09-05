---
layout: post
date: 2017-08-03 17:30
title: Proper Web Servers in Clojure
categories: [clojure]
---

Bad web application server setups can waste days of your life over even a short period of time.
Taking some time to set up a proper work flow will improve your work flow and maybe even earn you a long weekend.

### Bad Servers and Why they are Bad

```clojure
(use 'org.httpkit.server)

(def app [request]
  {:status 200
   :headers {"Content-Type" "text/html"}
   :body "<h1>Hello World!</h1>"})

(defonce server (run-server app {:port 8080}))
```

Give that code a shot.
Evaluate all of it and head to `http://localhost:8080` to see your hello.
Congratulations! You just made a Clojure web server!
But it's pretty useless.

One of the most important features of Clojure is the speedy feedback loop.
With proper server set up, that feedback loop can be nearly instantaneous.
With bad set up, like above, your server will lock your repl.
Locking up the REPL pushes you into a _java-like_ workflow which will waste a lot of time.

### A better way

```clojure
(use 'org.httpkit.server)

(def app [request]
  {:status 200
   :headers {"Content-Type" "text/html"}
   :body "<h1>Hello World!</h1>"})
   
(defonce server (atom nil))

(defn start-server []
  (reset! server (run-server #'app {:port 8080})))

(defn stop-server []
  (when-not (nil? @server)
    (@server :timeout 100)
    (reset! server nil)))

(defn restart-server []
  (do
    (start-server)
    (stop-server)))
```

With this, we now have the ability to start, stop, and restart a server via a REPL.
You also no longer have to restart your server whenever you make code changes.
You just have to send the updated code to the REPL and eval `(restart-server)`.

### Time Savings

I put together a __very__ simple [REST service][1] and did a little bit of science.
I timed restart times for code changes.
I used the hand timer on my watch so give about 1 second either way on these times.

First, I created a 'bad' server and timed how long it took to quit the REPL, start a new REPL, and start the server.
Over 25 restarts (excluding the first start) the average time was 17 seconds with a low of 12 seconds and a high of 22 seconds. 20 of the times were between 16 and 19 seconds.
The total time spent restarting was 425 seconds or 7 minutes and 4 seconds (+/- 25 seconds).

My next test was to time the 'good' server.
Again, 25 trials, timed using the same stop watch.
The average time was <1 second.
All of the times were <1 second.
The high and low were both <1 second.
I wasn't fast enough to keep up.
We'll call the total time spend restarting 25 seconds (+0/-24.9 seconds).

If you restart your web server for code changes 25 times per day for 6 months (assuming a 5 day work week), you waste 51,000 seconds.
Thats 850 minutes.
Thats 14 hours, 9 minutes, and 59 seconds.
You lose almost 2 whole work days!
That's unacceptable.

If you're stuck in this kind of a feedback loop, take some time to autimate your process.
Even if it's a as little as 30 seconds, you can save days in the long run.
Take that extra time to go on vacation or work on a side project.

___

[1]: https://github.com/johndenverscar/rest-test
