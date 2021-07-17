---
title: "Highly Inefficient Ways to Store Images in CSS"
date: 2021-07-17
updated: 2021-07-17
tags:
    - CSS
    - Humor
---

If you regularly work with CSS, chances are used to referencing images from CSS code. Usually this is done with the [`url()`](https://developer.mozilla.org/en-US/docs/Web/CSS/url()) function which can contain a HTTP URL that leads to an image, or in some cases a base64 encoded image directly.

But what can we do if these approaches are not exotic enough for our tastes? What if rendering images like this is just way too smooth and does not have enough cross browser inconsistencies? Well, you're in luck - just continue reading.

<!-- more -->

## Abusing CSS Shadows
