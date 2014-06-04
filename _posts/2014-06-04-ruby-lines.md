---
layout: post
title: Add a fully-featured irb console in just 3 lines of ruby
categories:
- blog
date: 2014-06-04 12:30
---

When I write code in Ruby, I spend a decent amount of time in irb poking API's, doing spikes, and testing out some logic.

The first couple times it's a little annoying that I need to `require` whatever gems and ruby files I need a play with, but after about the fifth time I'm raging at my keyboard yelling __"WHY HAVEN'T I AUTOMATED THIS YET!?"__

You might be thinking: "Just type `rails c`, duh!"

And I would tell you I don't do Rails... _on principle_!

I picked up a trick a couple years ago that lets me automate this process and give me a nice `rails c` like console... and it's only 3 lines of code.

I'll assume you have a `Rakefile`:

    desc 'Start IRB and load the app'
    task :console => :load_app do
      require 'irb'
      ARGV.clear
      IRB.start
    end

Okay, it's actually 6, but the meat is only 3.

Now I can type `rake console` and bam, I'm sitting at an irb prompt, inside the context of my application (I'm also assuming either your `Rakefile` or that `load_app` task loads up all the things you want to play around with at your console).

The above example is for a [Sinatra app](https://h2w.cc) I'm working on.  The next example is for a [git server migration script](https://github.com/sep/gitlab-import) (that [I wrote about a little while ago](http://jonfuller.co/blog/2014/04/08/git-migration.html)).  It doesn't have an application to load, but does have a couple handy files to require, and then it even instantiates a class and tells me how to access it:

    desc 'an irb console with export data and the gitlab api available.'
    task :console do
      require 'irb'
      ARGV.clear

      @importer = setup_importer()

      puts "The importer is available here: @importer"
      puts "The gitlab instance is available here: @importer.gitlab"

      IRB.start
    end

This is extremely useful for nearly any ruby project I'm working on, whether it's a web app/api, import/export script, or a gem.

Let me know how it works out for you! [@jon_fuller](http://twitter.com/@jon_fuller)
