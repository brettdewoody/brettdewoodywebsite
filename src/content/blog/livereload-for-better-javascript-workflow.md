---
title: "LiveReload for Better Javascript Workflow"
date: 2014-11-14
description: "Learning to code is like learning any other craft, the right tools and workflows can make all the difference."
tags: []
draft: false
---

Learning to code is like learning any other craft, the right tools and workflows can make all the difference.

Three weeks ago I became a student at [Hack Reactor](http://www.hackreactor.com/), a 12-week immersive coding program focused on full-stack Javascript development. Prior to class I had only superficial knowledge/experience with Javascript, mostly in the form of jQuery. Over the past 3 weeks my knowledge has expanded by leaps and bounds every hour of every day.

Over the coming weeks I’ll be sharing some of what I learn, my experience at Hack Reactor and the projects I’m working on. To start here’s a tool I recently picked up to make my learning/coding workflow more efficent.

##A Faster Workflow?

In my previous forays into Javascript I’ve always found it a difficult language to write, test and debug. Thankfully there are many tools available these days - [JSLint](http://brettdewoody.github.io/live-reload-for-better-javascript-workflow/jslint.com), [Chrome Developer Tools](https://developer.chrome.com/devtools) and even a [standalone browser for developers](https://www.mozilla.org/en-US/firefox/developer/).

But the act of writing, saving, loading and editing (and repeating) has always seemed repetitive, even unnecessary to me. Chrome’s Dev Tools has [some features](https://developer.chrome.com/devtools/docs/authoring-development-workflow) to help on this front but not to the extent I hoped for.

##LiveReload to the Rescue

Earlier this week I mentioned my frustration to Hack Reactor instructor and co-founder [Doug Calhoun](https://www.linkedin.com/pub/douglas-calhoun/44/998/500) and he pointed me in the direction of [LiveReload](http://livereload.com/).

LiveReload is a tool that watches for changes to files and reloads pages in your browser. It comes in many forms - a [Mac App](https://itunes.apple.com/us/app/livereload/id482898991?mt=12), a Javascript `<script>` file you can include in your web pages, or as a [browser extension](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei?hl=en).

I went with the latter due to it’s ease of implementation.

##No More Reloading

First I installed the [LiveReload plug-in for Sublime Text](https://sublime.wbond.net/packages/LiveReload). The plug-in handles the ‘watch’ part - watching for changes to my Javascript, HTML or CSS files.

Next I installed the [LiveReload Chrome extension](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei?hl=en). The extension handles the ‘reload’ side of things.

The combo of the plug-in and extension allows me to edit a file and watch the changes appear instantly in my browser.

##A Real Example

On a recent toy problem the benefits of LiveReload become very apparent. I created a simple HTML page containing a few [Mocha](http://unitjs.com/guide/mocha.html)/[Chai](http://chaijs.com/) tests and a `<script>` tag to include my .js. I pulled this up in my browser, revealing my current code was failing some of the tests.

Then I opened my `.js` file in Sublime and started working on my function. With each `CMD + s` (aka save) the changes were reflected in the HTML test file. All without having to reload the browser page.

Edit code, `CMD + s` to see the effects within milliseconds, and repeat.

<iframe src="https://player.vimeo.com/video/111821714?color=06b25b&title=0&byline=0&portrait=0" width="1600" height="900" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

Thanks to LiveReload my workflow is faster and more enjoyable than ever.
