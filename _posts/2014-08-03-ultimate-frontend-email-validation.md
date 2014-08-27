---
layout: post
title: Your ultimate frontend email validation
summary: Leverage native APIs, otherwise check only for @ and go grab another beer.
categories:
  - frontend
  - javascript
---

It is common practice to validate emails using complex regular expressions,
but chances are they are all wrong. Frontend validation in general is great, but
if it happens at the cost of rejecting valid emails, then it sucks, because
surely you are logging all validation errors and auditing if they are legit.
Right.

Well, here is my attempt:

{% highlight javascript %}
(function () {
  'use strict';

  var api = {};

  // element needs to have type="email"
  api.isValidEmail = function (element) {

    // leverage the Constraint Validation API when it is available
    if (typeof element.checkValidity === 'function') {
      return element.checkValidity();
    }

    // otherwise, this is about the only regex
    // that doesn't reject any valid email addresses
    return (/@/).test(element.value);
  };
}());
{% endhighlight %}

Please note that the Constraint Validation API has a
willful violation of RFC 5322 with the following explanation:

> [RFC 5322] defines a syntax for e-mail addresses that is simultaneously
> too strict (before the "@" character), too vague (after the "@" character),
> and too lax (allowing comments, whitespace characters, and quoted strings
> in manners unfamiliar to most users) to be of practical use here.

Source: [W3C E-mail state](http://www.w3.org/TR/html5/forms.html#valid-e-mail-address)

That link also provides a regular `expression` that implements the
current specification in case you really want to go down that road:

{% highlight javascript %}
    /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$/
{% endhighlight %}
