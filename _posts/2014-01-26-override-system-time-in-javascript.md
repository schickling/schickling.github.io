---
layout: post
title: Override system time in javascript
---

I recently wrote a time-dependent part of a business logic, which I needed to unit test. (In javascript I run all my unit tests with [Karma](http://karma-runner.github.io/) and [Jasmine](http://pivotal.github.io/jasmine/)). The logic I had to test depends on the current system time. So it behaves (and should behave) differently if its 10pm or 8am. My first rough workaround was altering the system time of my operating system. Obviously this was a kinda bad solution so I needed to find another way.

--

One solution would have been to wrap the javascript `Date` function and mock it while testing, but this approach kinda felt like overkill. There had to be a simpler solution.

So I ended up writing **[timemachine](https://github.com/schickling/timemachine)**. It lets you **test your time-dependent app by monkey patching the Date function and overriding your system time**. Over the time I added a lot of fancy features.

Let's say you want to simulate the following 24th of December 1980, 11:01pm in Greenwich Mean Time. It's as simple as that:

```js
timemachine.config({
	dateString: '24. Dec 1980, 11:01 GMT'
});
console.log(new Date()); // Wed, 24 Dec 1980 11:01:00 GMT
```

A more complex scenario would be if you want the same date but the time should continue to elapse. So at the time of using the timemachine it should be 11:01:00 and 5 seconds later it should be 11:01:05. This can be achieved by the `tick` parameter. Here is the example.

```js
timemachine.config({
	dateString: '24. Dec 1980, 11:01 GMT',
	tick: true
});
console.log(new Date()); // Wed, 24 Dec 1980 11:01:00 GMT
// tick tack - let's waste 5 seconds
console.log(new Date()); // Wed, 24 Dec 1980 11:01:05 GMT
```

### Conclusion
I tried to implement a simple and easy way to override the javascript system clock. I think this approach should fit to most of the testing purposes. I even consider to use this package to **sync client side javascript applications with backend servers**.