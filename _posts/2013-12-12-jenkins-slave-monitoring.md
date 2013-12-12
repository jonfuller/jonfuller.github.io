---
layout: post
title: Jenkins Slave Monitoring
categories:
- blog
date: 2013-12-12 09:00
---

Ever wanted to monitor the status of your [Jenkins](www.jenkins-ci.org) slaves?
As I've said before, [Jenkins is one of my favorite tools](http://jonfuller.co/blog/2013/10/16/simple-jenkins-jobs.html).

At [SEP](http://www.sep.com), we program [all the things](http://www.sep.com/services/software-development/), so we need many different build environments.  Jenkins [slaves](https://wiki.jenkins-ci.org/display/JENKINS/Distributed+builds) to the rescue!

We have several slaves that run the following environments:

* Mountain Lion
* Windows 8
* Windows 7
* Ubuntu

Occasionally there is a hiccup and one will go down, or not stay connected to the master.  Jenkins doesn't necessarily tell you that a node is down, so we cooked up a way for Jenkins to tell us.

There are typically two types of node failures:

1. The machine is actually not on
1. The slave is not connected to the master

## Addressing the _not on_ problem

`ping` is really good at telling me whether a machine is on or not.  So, I created a Jenkins job that is based on ping to tell me if the slave is up or not:

1. Create a free-style Jenkins job.
1. (no source control necessary)
1. Make a build step for "execute shell" with the following contents (replace `slave-hostname` with the hostname of your slave):

         ping -c 4 slave-hostname

1. Save.
1. Bask in the glory of your newly monitored slave.

## Addressing the _not connected to master_ problem

Jenkins obviously knows that a slave isn't connected... but doesn't give us a great way to monitor it.

![slaves](/static/jenkins-slaves.png)

This information _is_ available in the Jenkins _computer_ API.

So...

1. Create a free-style Jenkins job.
1. (again, no source control necessary)
1. Make a build step for "execute shell" with the following contents (replace `slave-hostname` with the hostname of your slave):

         ENDPOINT="http://jenkins.sep.com/computer/api/xml?xpath=computerSet/computer\[displayName='slave-hostname'\]/offline"
         ENDPOINT_RESULT=$(curl $ENDPOINT 2> /dev/null)
         
         if [ $ENDPOINT_RESULT = "<offline>false</offline>" ] ; then
           exit 0
         fi
         exit 1
1. Save.
1. Enjoy the piece-of-mind knowing that your slave status is now monitored by a Jenkins job.

## Now what?

Once I had Jenkins jobs for monitoring my slaves, I can hook them up to any sort of notification system I want.  For example, I have the [twilio](http://www.twilio.com/) plugin set up to send me a text message anytime a build node goes down, so I can go hook it back up.