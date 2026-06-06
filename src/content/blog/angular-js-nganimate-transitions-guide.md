---
title: "A Guide to CSS Transitions in AngularJS"
date: 2015-08-25
description: "Animated elements are extremely useful in modern user interfaces and utilized by nearly every web and mobile app these days. Typically an animation would be tri"
tags: ["javascript", "angularjs", "css", "transitions"]
draft: false
---

Animated elements are extremely useful in modern user interfaces and utilized by nearly every web and mobile app these days. Typically an animation would be triggered by hovering-over or clicking an element. With Angular, the options for animating elements become endless, and its probably easier than you think to get started. In this guide I explore CSS-based transitions in AngularJS.

##`Animations` vs `Transitions`
Standard animations and transitions are created using CSS's `animation` and `transition` properties. The two methods are similar in that they both alter CSS properties over a specified amount of time. For example, changing the `background-color` of an element. 

Beyond that the two methods are very different for a variety of reasons:     

* how the animation is triggered
* the complexity of animation possible
* options for looping
* hooks for interacting with the animation via javascript

For more detail on the differences between `animations` and `transitions` [read this great article by Kirupa](http://www.kirupa.com/html5/css3_animations_vs_transitions.htm).

For the purpose of this article I'm only using `transitions` though many of the methods can be applied to `animations` too. Transitions are useful when you want to animate between two states based on an interaction - the user clicking or hovering, or the addition or removal of an element. Here's a simple example of a CSS `transition` triggered by hovering over an element. 

**HTML** 

    <button>Sign Up</button>  

**CSS (using `transitions`)**

    button {
      background-color:tan;
      transition:all linear 0.5s;
    }

    button:hover {
      background-color:wheat;
    }

<iframe width="100%" height="200" src="//jsfiddle.net/brettdewoody/h9xtdm26/embedded/result,html,css,js/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

##Transitions in Angular##

Adding transitions in Angular (v.1.3+) is deceptively simple. I tinkered for several hours before I reached my AHA moment so hopefully this guide can save others some time. 

There are two ways to add `transitions` in Angular - with CSS or JS. CSS-based transitions use only CSS classes, while the JS method triggers animations that are registered via `module.animation()`. The examples below will only cover the CSS method.

Here was my epiphany regarding `transitions` in Angular: 
> Angular dynamically adds and removes specific CSS classes to elements, allowing you to add standard CSS transitions.

In other words you don't have to do anything in your Angular code. Angular adds and removes classes from elements, providing hooks for adding `transitions`. We'll cover the specifics of the CSS classes shortly. 

####Installation####

To get started with transitions you'll first need to add the `ngAnimate` module to your app. 

Include the `angular-animate.js` in your HTML:

    <script src="angular.js">
    <script src="angular-animate.js">

Then add the ngAnimate module as a dependency:

    angular.module('app', ['ngAnimate']);

####`Transition`-aware directives

To add a `transition` to an element the element needs one of the following Angular directives attached:

* `ngShow`/`ngHide`
* `ngRepeat`
* `ngView`
* `ngInclude`
* `ngSwitch`
* `ngIf`
* `ngClass`
* form
* `ngModel`
* `ngMessage`/`ngMessages`

For example:

    <div ng-show="showThisDiv">This Div is Visible####What `ngAnimate` does####

When Angular encounters an element using one of these directives it dynamically applies utility classes to the element, making it easy to attach CSS `transitions`. The classes applied depend on the directive being used. 

####Example####

Going back to the previous example using `ng-show`, if `showThisDiv` returns (or is changed to) `false` Angular will add a class of `ng-hide` to the element. 

    <div ng-show="showThisDiv=false" class="box ng-hide">This Div is VisibleChanging `showThisDiv` to `true` will result in the `ng-hide` class being removed. 

The addition/removal of the `.ng-hide` class allows us to create the `transition` with some simple CSS:

    .box {
      transition:all ease-out 0.5s;
      opacity: 0;
    }

    .box.ng-hide {
      opacity:1;
    }

Here's a more useful example showing how to create a simple animated dropdown:

<iframe width="100%" height="300" src="//jsfiddle.net/brettdewoody/to4jncry/embedded/result,html,css,js" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Clicking the Menu bar toggles the `toggleDropdown` variable between `true` and `false` causing the dropdown to have the `.ng-hide` class added and removed with each click. The addition of some simple CSS completes the transition.

##Other Directives##

The other directives work similarly, with unique classes being dynamically applied depending on the directive.

It's not worth repeating the docs verbatim so for specifics on what classes are added to each directive and what triggers the classes head over to the [Angular docs](https://docs.angularjs.org/api/ngAnimate#usage). While there are a few classes specific to certain directives follow one of 2 patterns.

For example, several directives (`ng-repeat`, `ng-view`, `ng-include`, `ng-switch`, `ng-if` and `ng-messages`) use variations of of `.enter` and `.leave`. 

The remaining directives (`'ng-class`, form, `ng-model` and `ng-messages`) use variations of `.add` and `.remove`. 

####Variations?####

I have yet to find clear documentation on all the variations so it requires some DOM watching in your favorite inspector to figure it out. Here's the basic gist of it. 

Returning to the `ng-show` example from the beginning - when `ng-show` evaluates to `false` Angular will temporarily add a `.ng-hide-add` class during the transition, then remove it at the end of the transition. Similarly, when `ng-show` is `true` a class of `.ng-hide-remove` is temporarily added during the transition.   

**With `ng-if`**

Attaching `ng-if` to an element will dynamically inject `.ng-enter` and `.ng-leave` classes as the `ng-if` statement evaluates to `true` and `false` respectively. In addition to these 2 classes it will also inject `.ng-enter-active` and `.ng-leave-active`. The former being added temporarily as the element is transitioning to `.ng-enter` and the latter being added while the element is transitioning to `.ng-leave`.

These additional hooks give us the ability to create transitions between the two `ng-if` states. Here's the dropdown example from before using `ng-if` instead of `ng-show`.  

<iframe width="100%" height="300" src="//jsfiddle.net/brettdewoody/vob4mp87/embedded/result,html,css,js" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

**With `ng-class`**

The `ng-class` directive uses variations of `.add` and `.remove` by appending `add` and `remove` onto the classes in the `ng-class` expression. In the example below I'm using 

    ng-class="{'show-dropdown': vm.toggleDropdown}"

causing the `.show-dropdown` class to be added and removed as `vm.toggleDropdown` alternates between `true` and `false` respectively. Angular also adds utility classes of `.show-dropdown-remove-active` and `.show-dropdown-add-active`, giving us enough hooks to add the transition we want. 

<iframe width="100%" height="300" src="//jsfiddle.net/brettdewoody/p96yj59o/embedded/result,html,css,js" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

##Wrapping Up##

Angular's `ngAnimate` module enables endless options for adding useful animations to your app. This guide is merely the tip of the iceberg and I encourage you to dive deeper into Angular's animation features.
