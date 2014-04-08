---
layout: post
title: Moving Git Servers
categories:
- blog
date: 2014-04-08 10:00
---
I recently migrated [SEP](http://www.sep.com) from using an old crusty version of gitorious, to a new shiny version of [gitlab](http://gitlab.com).

Overall, it was a pretty smooth process, here's a chronicle of some of the events, and some things to think about/look out for when doing a tool migration like this.

## Step 1: Export

There are two logical steps for a migration effort like this.  __Export__, and then __Import__.  Usually I'd use an API for the export, but the version of gitorious we were on didn't have a documented API.  So, I made a script, and loaded it into a rails console.  __BAM__: Instant API.

Once you're on the gitorious machine, it looks something like this:

    $ cd /var/www/gitorious
    $ sudo script/console production
    $ load '~/gitorious-export/export.rb'

Here's the script in all it's glory: [https://github.com/sep/gitorious-export/blob/master/export.rb](https://github.com/sep/gitorious-export/blob/master/export.rb)

It does 3 things:

1. Export users and their data (ssh keys, usernames, etc.) to a JSON file.
1. Export groups (teams) and their memberships to a JSON file.
1. Exports project information (names, memberships, source code) to a JSON file and bare git repos.

## Step 2: Import

Now, [gitlab](http://gitlab.com) has a pretty decent looking [API](http://doc.gitlab.com/ce/api/), and a [ruby gem](https://github.com/NARKOZ/gitlab) to go with it... so this should be easy, right?

Well, sort of.

The API has all the things I needed.  The API, however, has some shortcomings in it's non-happy-path scenarios.

Namely: ALL NON-HAPPY-PATH SCENARIOS RETURN __404 NOT FOUND__.

__WTF?__

Anyways, given the completeness of the gitlab API, it was relatively easy to complete the several logical steps:

1. Create users and load their SSH keys from the exported data.
1. Add _groups_ (this is like a _github organization_ or a _gitorious project_) and their associated members.
1. Add _projects_ (repositories) and their associated members.
1. Push source code.

Here is the code for the importer... it's a little less hackety than the exporter since I could use a real API.  [https://github.com/sep/gitlab-import](https://github.com/sep/gitlab-import)

## Done, right?

As with any widespread change that's going to affect the rest of the company... there's a couple other things to do.  __Test, Test, Test__ and __Spread the word__.

I tested this thing OVER and OVER until I knew it would be flawless.  This took hours (exporting years worth of data from the gitorious and loading into a VM running gitlab), but it would be worse if a single developer couldn't work for a couple hours because I screwed something up.

Spreading the word is easy... just send out a couple company wide emails, and all is cool.  (__Hint:  Engineers don't read email.__)

## A quick retro on the execution:

Things I thought about:

* Repo URL's:  They need to stay the same.  Everyone already has lots of code on their machines.
* People don't read email
* LDAP
* The RSA Key (known hosts) problem (the RSA key for the new server is different than the old server... SSH will show you a scary _man in the middle attack_ message).
* People won't read email
* SSH Keys
* Even if people read email, they won't do what they need to do.

Things I didn't think about:

* One of the reason's we're moving away from the system struck at the moment I went to do the export.  This added 3 hours to the process.
* Gitlab doesn't support the _git protocol_ (i.e. `git://whatever.org/repo.git`).
    * This affected all our code review projects on [Crucible ](https://www.atlassian.com/software/crucible/overview).
    * This affected all our CI builds on [Jenkins](http://jenkins-ci.org)
    * This also affected some [capistrano](http://capistranorb.com/) scripts we have that have git urls in them.
* People not only wouldn't read email, if they did, some folks still wouldn't do that they needed to do (e.g. create user account, and fix known_hosts file) to be able to get their work done.

