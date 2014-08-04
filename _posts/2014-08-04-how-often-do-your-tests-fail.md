---
layout: post
title: How often do your tests fail?
summary: A bit less often than yes, I have measured it.
---

I have a [few tests][tests] for an [automatic player][autoplayer] of my
[Secretary Problem game][game] that are nondeterministic by nature.
Every now and then [a build will fail][failedbuild] because the expectation for a test
is not met.

In my case, I don't commit all that often, but if you have some tests that
are not guaranteed to behave in the same way on different runs, why not
have a little script to run them a number of times and see what happens?
Here is mine:

{% gist javiercejudo/59b23a3b3a4cd4a805d9 %}

Then I simply use it by running `./test-loop.sh 100` and wait for the
notification. If you are not on Ubuntu, you can replace
[`notify-send`][notifysend] with your notification tool of choice.

My test suite takes about ~3 seconds to complete, so I can leave run it
running few hundred times without wanting to die, but for bigger suites
you probably want to avoid nondeterministic tests altogether.

[tests]: https://github.com/javiercejudo/javiercejudo.com/blob/v1.2.7/tests/unit/SecretaryProblemSpec.js#L45-L85
[autoplayer]: https://github.com/javiercejudo/javiercejudo.com/blob/v1.2.7/js/controllers/SecretaryProblemCtrl.js#L343-L396
[game]: http://www.javiercejudo.com/#!/game
[failedbuild]: https://travis-ci.org/javiercejudo/javiercejudo.com/jobs/31416173
[notifysend]: http://manpages.ubuntu.com/manpages/gutsy/man1/notify-send.1.html
