---
layout: post
title: Simple Jenkins Jobs
categories:
- blog
date: 2013-10-16 09:00
---

Title: Simple Jenkins Jobs

Jenkins is one of my favorite tools in my toolbox.

Jenkins is awesome in that he can do just about everything.  Things like:

* Get my code from git
* Run my rake/msbuild scripts
* Run my rspecs and cuke my cukes
* Kick off other related builds if successful.

I recently finished a Jenkins migration (from an old crusty machine to a new monster), and noticed something.  There were very clearly 2 categories of jobs:  __Complicated__ and _Simple_

## The Complicated

A complicated job (IMO) means more than 1 build step.  For example:

1. Clean
1. Configure
1. Setup Filesystem
1. Compile & Build
1. Run Unit Tests
1. Run Inspections

Each one of these called into different scripts with different targets and different arguments.

This makes the job hard to understand, and more importantly, more difficult to perform a local build.  As described by [Fowler](http://www.martinfowler.com/articles/continuousIntegration.html) and in [The Book](http://www.amazon.com/Continuous-Integration-Improving-Software-Reducing/dp/0321336380/ref=sr_1_1?s=books&ie=UTF8&qid=1381927324&sr=1-1&keywords=continuous+integration), being able to perform a local build easily is quite important.

In this example, Jenkins is used as a script manager, in addition to a scheduler.

## The Simple

The simple jobs were the jobs that had well structured build automation scripts with single build steps that looked like:

`bundle install && bundle exec rake ci`

Here, we're using Jenkins only as a scheduler.  This makes it really easy to execute a local build.  In addition, I can easily understand what the simple job is trying to do.

## My Advice For Simple Jenkins Jobs

While some builds are inherently complicated, I think it's worth striving to keep them simple.  Here are some tips:

* Treat Jenkins as a scheduler.
* Keep your build steps to a minimum (i.e. _leverage your build system to perform all your tasks via one target, rather than having Jenkins perform each task separately_).
* Use Jenkins to aggregate and display your results.
* Use Jenkins to kick off related/downstream builds upon completion.
