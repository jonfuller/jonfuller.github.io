---
layout: post
title: Striving to Be Replaceable
categories:
- blog
date: 2014-04-15 09:00
---

In the book, The [Passionate Programmer](http://www.amazon.com/gp/product/1934356344/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=1934356344&linkCode=as2&tag=sansblo0d-20), there's a section about making sure you're replaceable.

Several of us here at [SEP](http://www.sep.com) read that book during a book club.  When my group first started this section we were taken aback.  Finally, I think we teased apart two different ways of being replaceable.

__Bad Replaceable__: Basically just be a cog in a machine (makes me think of the term 'human resource').  I'm replaceable because I'm no better than anyone else.  This is not a position I want to be in... I'll be replaced soon, likely against my will.

__Good Replaceable__: What Chad is really talking about in the book, however, is being amazing at what I do, __AND__ making sure I'm not the only one that can do it.  If I'm the only one that can do it, technology moves fast enough, I'll be in the previous category before I know it.

This not only benefits the whole (i.e. raising the water level), it also benefits me.  I love working on new projects at work, but if I'm the only guy that can maintain this old project; guess what?  I'm going to be the guy working on that old project.

### Tactics

I recently left a project where I played tech lead for a couple of the major components (UI and workflow).  We knew from the get go that I was going to roll off the project, and leave a less-experienced engineer on the project to continue to add new features once we had solidified the core.

Here are a handful of techniques I used to help him _fall into the pit of success_ after I left:

1. __Conventions__ - We were using .NET on this project, but we did some very non-mainstream (at the time) things.  Lots of convention over configuration for registering and loading instances and views.  This helped us keep the code tight, delcarative, and well-tested.
1. __Focus, Direction, and Tone__ - Your code needs to tell a story.  It needs focus so the story is clear; direction so the story it's easy to tell where the story is heading; and tone so we know how the author intends for the story to be told.
1. __Teach, Teach, Teach__ - When working with anyone that isn't as familiar with what you're doing as you are, it's seductively easy to give short (and correct) answers to questions, but it's more fun and rewarding in the long run to turn any given question into a learning experience.

    __Use fishing pole, rather than giving out fish.__

1. __Have a philosophy__ - We made a README on this project that stated our philosophy:

    > the philosophy:
    >
    >   * do what matters
    >   * do what makes sense, when it makes sense
    >   * challenge what doesn't make sense

    We also boiled our product purpose down to a single question we could ask if we had any questions during development:

    > __"Does this help the sales rep get data onto this device more easily?"__

    This type of laser focused question helped us __all day, every day__.
    

1. __Encode your philosophy into the code__ - As alluded to previously, we were ruthless with our code.  If something didn't matter or didn't make sense, it didn't make it past review.  In fact, it usually didn't make it past the minute after typing it in since we pair-programmed most of the app.

    Our code was small, declarative, and well-tested.  Our process was no fuss (no electronic work item tracking systems).  We built [tools](https://github.com/jonfuller/awefsm) and [libraries](https://github.com/sep/TwoIoc) to help keep our code tight.  We automated __everything__ (builds, tests, VM's, releases, document generation, etc.).

None of this implies you need to start your project with an exit strategy in mind.  These tactics apply to any project, whether you know you're leaving in 6 months or 6 years.
