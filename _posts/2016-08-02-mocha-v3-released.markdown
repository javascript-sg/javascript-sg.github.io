---
published: true
title: Mocha v3 released
layout: post
---
This is excited -- a major update of a longstanding testing tool.

Significant changes:

* Due to the increasing difficulty of applying security patches made within its dependency tree, as well as looming incompatibilities with Node.js v7.0, Mocha no longer supports Node.js v0.8.
* `.only()` is no longer "fuzzy", can be used multiple times, and generally just works like you think it should. 
* The dot reporter now uses more visually distinctive characters when indicating "pending" and "failed" tests.

See the [changelog here](https://github.com/mochajs/mocha/blob/master/CHANGELOG.md)

I've upgraded two repositories at work to Mocha v3. It is working right with the latest Karma and Chai.