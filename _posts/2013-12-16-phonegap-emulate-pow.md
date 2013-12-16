---
layout: post
title: PhoneGap Emulate - POW
categories:
- blog
date: 2013-12-16-09:00
---

I'm working on a prototype using [PhoneGap](http://www.phonegap.com) and ran across the awesome [PhoneGap Emulator](http://emulate.phonegap.com/).  The emulator lets you run your web app as if you're in a device simulator/emulator (note however, that it doesn't emulate the particular device's rendering engine... it's always Chrome).

The problem (for me) is that the PhoneGap Emulator assumes your PhoneGap webapp is hosted somewhere.  My problem is that my PhoneGap webapp is simply the "www" directory inside of my PhoneGap project.

## Pow to the rescue

I could go through the whole process of figuring out how to [turn apache on for Mavericks](http://brianflove.com/2013/10/23/os-x-mavericks-and-apache/), but that involves mucking with apache and it's config files.  __No thanks.__

I remembered hearing about [Pow](http://pow.cx) from [37 Signals](http://37signals.com/) when it first came out awhile back, and thought that might be perfect for this... turns out it is.

Pow can host static sites (like my PhoneGap webapp) as easily as it does ruby/rack apps. The issue is, it wants your site to be in the `public` directory of your app directory; whereas PhoneGap places it into the `www` directory.

1. Install Pow

         (~) $ curl get.pow.cx | sh

1. Add a symbolic link from the `www` directory to create a `public` directory:

         (~/dev/myphonegapapp) $ ln -s www public

1. Hook it into Pow:

         (~/.pow) $ ln -s ~/dev/myphonegapapp

1. Emulate your app: [http://emulate.phonegap.com/](http://emulate.phonegap.com/).  Your app is available at the URL: [http://myphonegapapp.dev](http://myphonegapapp.dev) (of course, changing `myphonegapapp` to the actual name of the directory)
1. Continue PhoneGapping.
