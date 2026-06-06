---
title: "Deploying a Heroku App From a Subdirectory"
date: 2015-01-01
description: "We’ve entered the final weeks of Hack Reactor and are a week into our final thesis projects. My team has decided to build a turn-key mobile app for realtors - s"
tags: []
draft: false
---

We’ve entered the final weeks of Hack Reactor and are a week into our final thesis projects. My team has decided to build a turn-key mobile app for realtors - so realtors can quickly put together a mobile app to showcase listings in their area.

We agreed on a building hybrid mobile app using the [AppGyver’s Supersonic framework](http://www.appgyver.com/). To power our app we opted to create a simple Rails API to fetch and return MLS listings data.

Having used [Heroku](http://www.heroku.com/) for hosting on several projects we figured it would be an easy way to get our API up and running quickly. Not so fast…

##App Structure

When you start a Supersonic app you start with a structure similar to this:

- .git/
- .gitignore
- app/
- bower_components/
- bower.json
- config/
- dist/
- logs/
- node_modules/
- package.json

The problem comes when adding the Heroku app into the mix. Heroku also wants to live in the root, and you would typically deploy your Heroku app doing a push to your heroku master repository.

The issue with this setup is the Supersonic files would confuse Heroku. To get around this we created our Heroku app in a subdirectory named web/. Our directory structure now looks like this:

- .git/
- .gitignore
- app/
- bower_components/
- bower.json
- config/
- dist/
- logs/
- node_modules/
- package.json
- **web/**
 - Heroku app files

With this structure we can’t use the [recommended Heroku deployment](https://devcenter.heroku.com/articles/git). Instead, we want to deploy only the contents of web/. To do this we use a git subtree push.

With our Heroku app in web/ we now deploy to Heroku using:

git subtree push --prefix web heroku master
So far we’ve had no problems with this setup. Supersonic and Heroku living in peace.

#UPDATE

After working on our project for about a week we came across an issue. We started getting fast-forward issues after others team members deployed to Heroku. Once another team member deployed to Heroku are usual deployment command of:

`git subtree push --prefix web heroku master`

no longer worked.

The reason being our local branch was behind the Heroku remote. Even after attempting a pull from the Heroku remote we were still having issues. The solution was this:

`git push heroku 'git subtree split --prefix web branch':master --force`

where web is the subdirectory where you Heroku app lives.
