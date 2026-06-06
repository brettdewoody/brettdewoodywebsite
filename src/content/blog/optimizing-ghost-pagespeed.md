---
title: "Optimizing Ghost for Speed"
date: 2015-08-26
description: "I recently decided to try Ghost, a relatively new, open-source blogging platform (you're viewing the outcome right now). The selling point was two-fold."
tags: ["ghost", "performance", "gulp", "responsive"]
draft: false
---

I recently decided to try [Ghost](http://ghost.org/), a relatively new, open-source blogging platform (you're viewing the outcome right now). The selling point was two-fold.

For one, Ghost is coded in Javascript, namely [Node.js](https://nodejs.org/). With my recent Javascript immersion it's a natural fit. 

Secondly, I want to focus on writing... not themes, widgets, plug-ins, etc. Ghost has an amazing, uncluttered UI, making it as easy as possible to write. 

## Out-of-the-Box Speed

One advantage of being built on Node.js is speed. The entire Ghost application, from the front-facing website to the Ghost admin runs exceptionally quick and smooth. But being obsessed with website performance like I am I decided to take a deeper look. 

I ran several Ghost theme demo sites (like [Urban](http://urban-themeghost.rhcloud.com/), [Meca](http://meca.ghost.io/) and [Wharton](http://wharton.ghost.io/)) through Google's PageSpeed test. The results varied from theme to theme but were less than spectacular. 

Most themes scored in the 35-75 range (out of 100). On the lower end of the spectrum the Meca theme [scored a 38/100](https://developers.google.com/speed/pagespeed/insights/?url=http%3A%2F%2Fmeca.ghost.io%2F&tab=mobile) on the mobile speed test.

Not good at all. 

## Server-side

Improving the performance of a Ghost site involves two parts.

First, the server needs to be configured correctly. This involves enabling compression and browser caching, reducing server response time and a number of other best practices. 

There are many great articles detailing how to configure a server for Ghost. 

- [High Performance Ghost Configuration with NGINX](http://pnommensen.com/2014/09/07/high-performance-ghost-configuration-with-nginx/)
- [Configuring Nginx to Speed Up Your Ghost.io Blog](http://www.sitepoint.com/configuring-nginx-speed-ghost-blog/)
- [NGINX config gist](https://gist.github.com/vvo/7414035)

Implementing these steps will improve a Ghost site's PageSpeed score into the 70s or 80s. Good, but not great. Next comes client-side optimizations.

##Client-side##

Before going into the details on client-side optimizations, one quick note: **not all themes are made equal**. 

As I discovered in my quick sample of theme demo sites, some themes score significantly better on the Pagespeed test than others. If you want to save yourself some time and work, select a theme that scores well on the PageSpeed test out of the box.

After finding a good-looking, semi-performant theme you're ready to take the next steps.

####Images####

In my case one of the biggest issues was images resizing and compression. My theme, Wharton, uses large images for each story. This requires me to upload relatively large images so they look good on large screens, but means small screens also get a really large image. Much larger than they need. 

Google's PageSpeed tool is smart enough to know my site is serving up a large image, physically reduced in size to fit the small screen. To handle this correctly I should be serving a smaller version of the image to smaller screens, and the larger version of the image to larger screens. 

Many CMSs can dynamically resize image to do this very task. Ghost unfortunately, can not (yet). 

To remedy this I added [Mobify.js](https://www.mobify.com/mobifyjs/) - a library for improving responsive sites by providing responsive images, JS/CSS optimization, Adaptive Templating and other features. Mobify detects the screen size and serves up an appropriately sized image. Now small screens get small images and large screens get large images. It's a pretty powerful little tool. 

####CSS/JS Delivery####

The next issue to tackle was [delivery of CSS](https://developers.google.com/speed/docs/insights/OptimizeCSSDelivery) and JS files. Linking to multiple CSS and JS files is highly frowned upon by the PageSpeed tool. 

Due to my site's simple design (and small `.css` file - 43kb) I decided inlining the CSS was the best option. To accomplish this I created a simple [`gulp` script](http://gulpjs.com/). 

    var gulp = require('gulp');
    var fs = require('fs');
    var minifyCss = require('gulp-minify-css');
    var plumber = require('gulp-plumber');
    var gutil = require('gulp-util');
    var htmlReplace = require('gulp-html-replace');
    var replace = require('gulp-replace');

    gulp.task('minimize', function() {
        return gulp.src('./css/**/*.css')
          .pipe(replace('../img/',          'http:///brettdewoody.com/assets/img/'))
          .pipe(minifyCss())
          .on('end', function(){ gutil.log('Compress CSS...'); })
          .pipe(gulp.dest('./dist'));
    });

    gulp.task('replace', ['minimize'], function() {
        return gulp.src('../default.hbs')
          .pipe(htmlReplace({'CSS': '<style>' +     fs.readFileSync('./dist/screen.css', 'utf8') + '</style>'}, {keepBlockTags: true}))
          .pipe(gulp.dest('../'));
    });

    gulp.task('build', ['minimize', 'replace']);

When I make changes to my site's styling a run `gulp build` to create the optimized build.

The above script reads my `.css` file, minimizes it and pastes the entire file into a `<style>` tag within the `<head>` of my site. If you `view:source` my site you'll see the entire CSS file within the source of the page. 

For Javascript, I minimized each of the included scripts and wrapped them into a single file.

##Results##

With the updates in place the site is now scoring a [98/100 on Mobile Speed, 100/100 on User Experience and 96/100 on Desktop Speed](https://developers.google.com/speed/pagespeed/insights/?url=http%3A%2F%2Fbrettdewoody.com&tab=mobile). Average load times from locations around the world is just a hair over 0.5 seconds too. 

What tricks and tips do you have for speeding up your Ghost site?
