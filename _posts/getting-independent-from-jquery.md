---
title: 'Getting independent from jQuery'
published: true
visible: true
icon: bullseye
date: '23-09-2015 15:14'
tags:
    - JavaScript
    - jQuery
    - Performance
---

When I first started coding JavaScript, I quickly found myself using jQuery because it simplifies a lot of basic tasks ranging from DOM manipulation to animation (which it does [rather bad](https://css-tricks.com/myth-busting-css-animations-vs-javascript/) by the way)

But after I got further into JavaScript, I realized that in a lot of cases jQuery is not the best solution: It does a lot of things, but none of them very good. Further if you include jQuery in every project you always have a pretty massive library to handle: 80kb doesn't seem that big on paper, but combined with other libraries and plugins it surely has a noticeable impact on performance and load times. Especially when creating non-web-based apps you should care about performance: using jQuery in an Android app, for example, has a huge impact on the app performance

So what can you do against this? The first question you must ask yourself when moving away from jQuery: What did you use jQuery for?:

*   DOM Manipulation?
*   AJAX?
*   Event handlers?
*   Animation?
*   the smaller utility Functions?

Depending on the answer there are some dedicated libraries for that single purpose! lets explore some of them.

### DOM Manipulation Libraries

**[DollarDom](https://github.com/julienw/dollardom)**
DollarDom is a tiny(5kb) library dedicated to DOM manipulation. very useful if you need a basic and lightweight DOM-manipulation code.

**[ZeptoJavaScript](http://zeptoJavaScript.com/)**
Even though Zepto is more than a pure DOM-manipulation library, its the main purpose of it. JQuery-like syntax makes it easy to use if you already know jQuery

**[microDOM](https://github.com/TheBinaryBee/microDOM)**
Even smaller than DollarDom, this jQuery-syntax-like library has all the essential DOM manipulation functions while being only 1.7KB minified! (Made by myself)

### AJAX Libraries

**[MicroAJAX](https://code.google.com/p/microajax/)**
MicroAjax is very tiny (800byte) library providing access the most important AJAX functions

**[MarmottAJAX](https://github.com/dimitrinicolas/marmottajax)**
This Library is more functional and includes advanced utilities like "update" and "delete" for your AJAX requests

**[esQuery](https://github.com/TheBinaryBee/esQuery)**
esQuery is a ES6 based, modern and small library for DOM-Manipulation and event binding with a jQuery-like syntax.

### Event handlers

**[MiniEvents](https://github.com/allouis/minivents)**
Again, very tiny but with solid utility, can really recommend this one

### Animation

**[GSAP](https://greensock.com/gsap)**
GSAP is an animation library from GreenSock focused on performance. it comes in various version and supports plugins!

**[VelocityJavaScript](http://julian.com/research/velocity/)**
VelocityJavaScript is more feature oriented than GSAP, and can be integrated in Zepto or jQuery. Has a lot of options.

### Utility Functions

**[UnderscoreJavaScript](http://underscoreJavaScript.org/)**
Underscore is most likely the most popular utility library with over 100 functions for everyday purposes like "map", "filter", "max", "contains" etc...

**[lodash](https://lodash.com/")**
Right after underscore, lodash is another very popular "utility-belt" library with tons of cool features!
