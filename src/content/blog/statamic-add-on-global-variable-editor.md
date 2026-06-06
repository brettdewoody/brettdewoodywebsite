---
title: "Statamic Add-on: Global Variable Editor"
date: 2015-09-08
description: "Based on feedback and requests I've updated my Global Variable Editor add-on for Statamic. Namely the ability to use Statamic's fieldtypes within the global var"
tags: []
draft: false
---

Based on feedback and requests I've updated my [Global Variable Editor add-on](https://github.com/brettdewoody/statamic-global-variable-editor) for [Statamic](http://statamic.com/). Namely the ability to use Statamic's fieldtypes within the global variable editor. 

Version 1 of the add-on was bare-bones, merely a tab within the Statamic control panel to edit global variables through a standard text input field. Within days of releasing the add-on I had requests for additional fieldtypes.

## Version 2##
Version 2 allows global variables to take advantage of [Statamic's fieldtypes](http://statamic.com/learn/documentation/fieldtypes), making the global variables more flexible and easier to edit for site admins. This allows global variables to use fieldtypes like `redactor`, `radio`, `select` and others. 

Allowed fieldtypes are:

* `checkbox`
* `checkboxes`
* `date`
* `markitup`
* `radio`
* `redactor` - no file uploads
* `select`
* `tags`
* `table`
* `text`
* `textarea`
* `time`
* `users`

##Changes##

One change in the new add-on is how global variables are used in templates. To output a global variable use the `{{globes}}` tag with a `name` parameter. For example, if you created a `phone` variable: 

**theme.yaml**

    globals:
      - 
        name: phone
        display: Phone
        type: text
        value: 555-555-5555

to display the `phone` variable in a template you would use: 

    `{{ globes name='phone' }}`

More info about the add-on are available on [Github](https://github.com/brettdewoody/statamic-global-variable-editor).

##Planned Changes##

I'd like to add support for the `file` fieldtype as it would be useful for changing static images on the site. Hopefully this will happen in the coming weeks. 

If you want to request other features or changes post a comment below.
