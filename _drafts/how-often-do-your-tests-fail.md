---
layout: post
title: How often do your tests fail?
summary: A bit less often than yes, I have measured it.
categories:
  - testing
---

I have a [few tests][tests] for an [automatic player][autoplayer] of my
[Secretary Problem game][game] that are nondeterministic by nature.
Every now and then [a build will fail][failedbuild] because the expectation for a test
is not met.

In my case, I don't commit all that often, but if you are building your
code thousands of times per day, 99% confidence is probably not enough.

Why not have a little script to run them a number of times and see
what happens? Here is mine:

{% highlight bash %}
#!/bin/bash

SUCCESS=0;
COUNT=0;
ITERATIONS=1;

if [ -n "$1" ]; then
    let ITERATIONS=$1
fi

while [ $COUNT -lt $ITERATIONS ]; do
    let COUNT+=1

    if grunt karma:dev; then
        let SUCCESS+=1
    fi

    echo Success rate: $SUCCESS/$COUNT
done

if hash notify-send 2>/dev/null; then
    notify-send "JavierCejudo.com test loop" "Success rate: $SUCCESS/$ITERATIONS"
fi
{% endhighlight %}

Then I simply use it by running `./test-loop.sh 100` and wait for the
notification. If you are not on Ubuntu, you can replace
[`notify-send`][notifysend] with your notification tool of choice. You
don't need to do this often, but if you find your tests failing here and
there, better measure how often they actually fail.

My test suite takes about ~3 seconds to complete, so I can leave it
running a few hundred times without wanting to die, but for bigger suites
you probably want to avoid nondeterministic tests altogether.

[tests]: https://github.com/javiercejudo/javiercejudo.com/blob/v1.2.7/tests/unit/SecretaryProblemSpec.js#L45-L85
[autoplayer]: https://github.com/javiercejudo/javiercejudo.com/blob/v1.2.7/js/controllers/SecretaryProblemCtrl.js#L343-L396
[game]: http://www.javiercejudo.com/#!/game
[failedbuild]: https://travis-ci.org/javiercejudo/javiercejudo.com/jobs/31416173
[notifysend]: http://manpages.ubuntu.com/manpages/gutsy/man1/notify-send.1.html
