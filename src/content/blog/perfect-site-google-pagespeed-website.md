---
title: "Building a 'Perfect' Site"
date: 2015-04-13
description: "With countless studies showing the benefits of a fast site, speed has become a core metric when analyzing a site or app. A study from KISSmetrics discovered a m"
tags: []
draft: false
---

> Fast and optimized pages lead to higher visitor engagement, retention, and conversions.

With countless studies showing the benefits of a fast site, speed has become a core metric when analyzing a site or app. A [study from KISSmetrics](https://blog.kissmetrics.com/loading-time/) discovered a mere 1-second delay increases page abandonment significantly and can cause a 7% reduction in conversions. For large e-commerce sites this can add up to millions of dollars in lost revenue. 

With such a huge focus on speed a variety of tools have popped up to help analyze and optimize web sites and apps. [Google PageSpeed](https://developers.google.com/speed/pagespeed/) is one of the most popular and provides a detailed report of how to optimize any site. The tool is available as a Google Chrome extension or online. You can test your site, or any site, here.

##Google PageSpeed##

PageSpeed analyzes the site from the perspective of a mobile and desktop user and provides a performance grade. The score is based on a wide range of factors, from image and other file sizes, time to load above-the-fold content, server response times, caching setup, and even several UI metrics like font and button sizes. 

From my experience, most sites will score 50-80 for Mobile and slightly higher for Desktop. And for most sites this is totally fine. A score of 80-90 is as good as many sites will ever achieve for a number of reasons. As an example, Google's own homepage scores a 78 for Mobile, and a 92 on Desktop. So don't worry if your site doesn't score above 90.

##But Let's Try...##

This past weekend I built a small site where I thought it'd be fun to max out the Google PageSpeed score. This quickly became an obsession.

After getting the basic site up and running I ran it through PageSpeed and achieved a respectable score of ~80 for both Mobile and Desktop. 

A few Apache rules (to set expire headers and gzip files) pushed the score up a few points but the site still wasn't cracking the 90 mark. The report was showing I could improve image file sizes, and the 'delivery' of CSS and Javascript.

##CSS & JS Delivery?##

Like many sites, I was linking to the site's CSS file in the header like this:

    <link rel="stylesheet" href="./css/styles.css" />

The problem with this setup is that it holds up the site from loading. When the browser encounters the link to the CSS file it stops loading the site, loads the CSS file, then resumes loading the site. This creates a delay in loading the page.

Google recommends adding the CSS for the initial view (above-the-fold content) to be 'inlined' - added to the HTML of the page so the browser has everything it needs to render without waiting for a CSS file to load. 

##Easier Said Than Done##

Unfortunately, determining the exact CSS pertaining to the ATF (above-the-fold) content isn't easy. This is especially true if the site has multiple templates.

There are several tools available to help parse a page and assemble the CSS related to the ATF content. I tried several but none of them worked in a quick, scalable fashion.

ATF content wasn't the only issue however, the site was still being docked points for image file sizes and other possible optimizations. I needed a way to take the un-optimized site and related assets (CSS, JS and image) through a ringer to create a highly optimized version.

Thankfully there are a number of tools to automate tasks like this, Gulp being one of the best. Gulp is a [NodeJS](https://nodejs.org/) module that makes it pretty simple to run automated tasks, ranging from something as simple as moving a file, to compressing JS and CSS files, optimizing image files and a huge variety of other tasks. As of this post there are more than [1470 plugins for Gulp](http://gulpjs.com/plugins/) to streamline various tasks. 

##A Gulp Example##

Here's an example of how Gulp works. Let's say I want to move a few files from one directory to another. Here's what a Gulp task to do that would look like:

    gulp.task('move-assets', function() {
      gulp.src('./assets/**/*')
        .pipe(gulp.dest('./build/assets'))
    });

The name I've chosen for this task is 'move-assets' (on the first line).

I then tell Gulp where to look for the files I want to move (the 2nd line), in this case in the /assets directory. The /**/* simply tells Gulp to move EVERYTHING within that directory and every directory inside it. 

The next line tells Gulp where to move the files to, in this case a directory called /build/assets. 

When I run this task Gulp will copy all the directories and files within /assets and move them to /build/assets. 

##Perfection (according to PageSpeed)##

Using a handful of Gulp plugins I created a series of tasks to put the unoptimized site through a ringer - minimizing every file, inlining the CSS and outputting a highly optimized version of the site. Here's a summary of the tasks:

- **Minify the CSS file**.
- **Remove unused CSS**. Because I was using a front-end framework (Zurb Foundation) the CSS file included a lot of extraneous styles. Thankfully there's a [Gulp plugin](https://www.npmjs.com/package/gulp-uncss) to handle this. It analyzes the HTML to find the minimum CSS and removes the extra. The minified CSS file went from 83kB to 16kB.
- **Inline the CSS**. Replace the linked CSS file in the head with the contents of the minified CSS file. In my case the CSS for the entire site is inlined.
-  **Minify the HTML**. Even with the CSS inlined the HTML file is just 20kB.
- **Combine all the external JS files into a single, minimized JS file**.
- **Run the JS file asynchronously**. This means the JS is executed while the page is being parsed, instead of holding up the parsing of the page.
- **Minify the images**. This shaved 596kB from the total file size for all the images.

My workflow is the same as any other site. When I'm ready to deploy the site I run the Gulp tasks to create an optimized version, then upload it to the web server.

The result? A [perfect score on PageSpeed](https://developers.google.com/speed/pagespeed/insights/?url=http%3A%2F%2Fsidecarmt.com%2F). 

##Additional Tests##

It's always a good idea to get a second and third opinion (or more) so I ran the site through a few additional test. The [Pingdom Website Speed Test](http://tools.pingdom.com/) showed [similar results](http://tools.pingdom.com/fpt/#!/cp4MaN/http://sidecarmt.com/), with a perfect score of 100/100 and a speedy load time of just 615 milliseconds it loads faster than 96% of sites tested on Pingdom. Not bad considering there's a 2600x1600px image included.

[Yahoo's YSlow](http://yslow.org/) analyzer gave the site an A on all 23 tests.

[GTMetrix](http://gtmetrix.com/reports/sidecarmt.com/GO0YTsz0) rated it 99%. Sitespeed.me rated the average load time (from locations around the world) at [0.69 seconds](http://sitespeed.me/tracer/511516), with average times from U.S. locations around 0.40 seconds. 

##Is it worth the trouble?##

I'm pretty impressed with the results. The site loads extremely quick, even on the first visit. If you're building landing or transactional pages I think it's worth spending some time to make the page perform well on these tests. 

If you're dealing with an entire website I wouldn't recommend trying to optimize every page. Find your primary landing pages, like your homepage, and focus on optimizing those, then move down the chain as needed. Most sites, and certainly specific pages, can benefit from some performance optimization. 

##One Caveat##

The site is an extremely simple example, it has only 2 pages and no content management system. For larger sites/apps, CMS-powered and other dynamic sites I think it would be tough to achieve a perfect score, but again, it's not necessary for most. 

##The Final Product##

The final, optimized and speedy site is available at http://sidecarmt.com/. Feel free to view the source to see the final output.

The site is built on the [Zurb Foundation](http://foundation.zurb.com/) front-end framework. Site design, logo and photography by the talented [Yogesh Simpson](https://www.linkedin.com/in/yogeshsimpson).
