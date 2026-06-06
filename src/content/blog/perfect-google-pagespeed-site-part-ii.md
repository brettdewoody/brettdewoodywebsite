---
title: "Building a 'Perfect' Site - Part II"
date: 2015-08-08
description: "A few months ago I set out to build a 'perfect' site in the eyes of Google's PageSpeed tool. The results were better than expected, with the site scoring a near"
tags: []
draft: false
---

A few months ago I set out to [build a 'perfect' site](http://brettdewoody.com/perfect-site-google-pagespeed-website/) in the eyes of Google's PageSpeed tool. The results were better than expected, with the site scoring a near perfect score on several website speed tests. 

The site, [SidecarMT.com](http://sidecarmt.com) is a simple example with only 2 pages and no content management system (CMS). The simple setup made it easy to optimize. 

## How to improve on perfect

For many sites a CMS is a requirement - allowing the client to update the site, add fresh content and keep the site up-to-date. Nearly every site I've built in recent years is powered by a CMS, usually [Craft](https://buildwithcraft.com/), [WordPress](https://wordpress.org/), [ExpressionEngine](https://ellislab.com/expressionengine) or [Statamic](http://statamic.com/). Each great for their own reasons.

The first three of those are traditional CMSs with a PHP/MySQL backend. The last, Statamic, is a flat-file CMS, using directories and basic file data to store the site's content. Flat-file CMSs are simple to install, build and customize, and they're also inherently fast.

In my opinion, a flat-file CMS is, at least currently, the easiest path to building a really fast site. While database-driven sites can be optimized, and even fast, a flat-file CMS can be more easily optimized to blazing-fast speeds. 

I love Statamic but opted to explore other flat-file CMSs for this project. Turns out there are a number of great ones.

* [Kirby](http://getkirby.com/)
* [Pico](http://picocms.org/)
* [Grav](http://getgrav.org/)
* [and many others](https://www.google.com/webhp?hl=en#hl=en&q=flat+file+cms)

Of these three [Grav](http://getgrav.org/) was the most interesting.

## Let's Try it ##

The new site is for a local Italian pizza joint, [Pizza Campania](http://pizzacampania.net). Similar in format to SidecarMT.com but with some minor differences. The site is a mobile first, responsive design. A bit simpler in that it's a single-page site (as opposed to two pages), and features a full-screen video background. 

![Pizza Campania](http://brettdewoody.com/content/images/2015/08/pizzacampania-1.jpg)

Nearly all of the strategies I used on SidecarMT.com were utilized on PizzaCampania.net:

* Optimized images
* Optimized and in-lined JS
* Optimized and in-lined CSS
* Compressed HTML
* Caching

With the exception of a small customization to Grav to output the CSS and JS in-line the CMS is a stock installation. Only one plugin is installed, [Static File Cache](https://github.com/fbrnc/grav-plugin-staticfilecache).

## Results ##

In short, even with the CMS the site is absurdly fast.

Google's [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/?url=http%3A%2F%2Fpizzacampania.net%2F&tab=mobile) gave the site a near perfect score. The main reason points were docked is due to the server response time, which I can only guess is due to the site being hosted on GoDaddy's shared hosting. 

Pingdom [ranked the site a 97/100](http://tools.pingdom.com/fpt/#!/b4iLwZ/pizzacampania.net) with an average load time of just 434 milliseconds, less than half a second, and faster than 98% of all test websites. Despite a full-page video background the page's size is just 1MB. 

Yahoo's YSlow ranked the site a perfect 100 with an A on all 23 tests. 

A survey of page load times from [SiteSpeed](http://sitespeed.me/en/tracer/536228) (from around the world) recorded an average load time of only 0.72 seconds, with a best of 0.22 seconds. 

## About the site ##

Site design is by the talented [Yogesh Simpson](http://yogeshsimpson.com/). 

## Next Steps

I'm hoping to try a more complex site next - powered by a CMS, with multiple pages, and with little to no loss in performance.
