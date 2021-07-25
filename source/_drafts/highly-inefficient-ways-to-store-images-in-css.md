---
title: "Highly Inefficient Ways to Store Images in CSS"
date: 2021-07-17
updated: 2021-07-17
tags:
    - CSS
    - Humor
---

If you regularly work with CSS, chances are used to referencing images from CSS code. Usually this is done with the [`url()`](<https://developer.mozilla.org/en-US/docs/Web/CSS/url()>) function which can contain a HTTP URL that leads to an image, or in some cases a base64 encoded image directly.

But what can we do if these approaches are not exotic enough for our tastes? What if rendering images like this is just way too smooth and does not have enough cross browser inconsistencies? Well, you're in luck - just continue reading.

<!-- more -->

## Abusing CSS Shadows

The idea for this article actually came to me when I looked through some relatively old projects of mine shortly after I learned how to program. One of those projects used JavaScript to convert an image file to a value that can be used for the CSS [`box-shadow`](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow) property in such a way that the box shadow produced looks like the original image.

In order to understand how this works, it is important to note that `box-shadow` and the closely related `text-shadow` properties in CSS have several properties that allow abusing them to freely draw an arbitrary amount of rectangular shapes that do not actually look like shadows: Every individual shadow defined can take an X and Y offset, an optional blur radius (for which we will use `0`, as we do not actually want the output to look like a shadow), an optional spread radius (not needed in our case), and the color of the shadow. This notation can be repeated any number of times, allowing for multiple shadows on a single element.

Encoding is actually _relatively_ straightforward; After loading the image into a [`canvas`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) element, we can iterate over every pixel and use [`CanvasRenderingContext2D.getImageData()`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/getImageData) to get the color data for this pixel. We can then construct a single box shadow for this pixel with the same position and color.

As reference image we will use an [image of the Mona Lisa from Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Mona_Lisa,_by_Leonardo_da_Vinci,_from_C2RMF_retouched.jpg) (161 × 240 pixels):
![Image of the Mona Lisa](./161px-Mona_Lisa,_by_Leonardo_da_Vinci,_from_C2RMF_retouched.jpg)

The resulting `box-shadow`-encoded representation of this image will look like this:

```
0 0 rgb(79 108 77),
1px 0 rgb(74 103 72),
2px 0 rgb(80 109 78),
3px 0 rgb(80 109 78),
4px 0 rgb(66 97 65),
5px 0 rgb(63 94 62), /* Further pixels of first row omitted */
0 1px rgb(87 114 83),
1px 1px rgb(82 109 78),
2px 1px rgb(87 114 83); /* All further pixels omitted */
```

[The final result can be seen on this CodePen pen](https://codepen.io/FelixRilling/full/xxdrwRe)
[Download source code](TODO)

In the case of our example image which originally was a JPEG with a file size of 11.8KiB, just the `box-shadow` value is roughly 962KiB - now thats what I call inefficient! I originally wanted to measure how long browsers take to paint the `box-shadow` image, but between Firefox devtools becoming extremely slugish when opening them while the `box-shadow` was visible, and Chromium crashing altogether, I decided that more in-depth analysis is not required.

Now as you might have guessed, this is pretty inefficient - but can we find an even worse way?
