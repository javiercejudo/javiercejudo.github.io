---
layout: post
title: AngularJS filter to convert a time (hh:mm:ss) to seconds
summary: Split, reverse, cast, and the power of 'reduce'.
categories:
  - frontend
  - javascript
  - angularjs
---

The following AngularJS filter converts something in the form of `hh:mm:ss`
or `mm:ss` to seconds.

{% highlight javascript %}
'use strict';

angular.module('myApp.filters.time', [])
  .filter('timeToSeconds', function () {
    return function (time) {
      return time.split(':')
        .reverse()
        .map(Number)
        .reduce(function (pUnit, cUnit, index) {
          return pUnit + cUnit * Math.pow(60, index);
        });
    };
  });
{% endhighlight %}
