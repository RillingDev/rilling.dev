---
title: "Highly Inefficient Ways to Store Images in CSS"
date: 2021-07-17
updated: 2021-07-17
tags:
    - CSS
    - Humor
---

If you regularly work with CSS, chances are that you are used to referencing images from CSS code. Usually this is done with the [`url()`](<https://developer.mozilla.org/en-US/docs/Web/CSS/url()>) function which can be used to reference an URL for an image file or directly embed a base64 encoded image.

But what can we do if these approaches are not exotic enough for our tastes? What if rendering images like this is just way too smooth and does not have enough cross browser inconsistencies? Well, you're in luck.

<!-- more -->

## Abusing CSS Shadows

The idea for this article came to me when I looked through some relatively old projects of mine from shortly after I learned how to program. One of those projects used JavaScript to convert an image file to a value that can be used for the CSS [`box-shadow`](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow) property in such a way that the box shadow produced looks like the original image.

In order to understand how this works, it is important to note that `box-shadow` and the closely related `text-shadow` properties in CSS have several properties that allow abusing them to freely draw an arbitrary amount of rectangular shapes that do not actually look like shadows: A shadow can take an X and Y offset, an optional blur radius (for which we will use `0`, as we do not actually want the output to look like a shadow), an optional spread radius (not needed in our case), and the color of the shadow. This notation can be repeated any number of times, allowing for multiple shadows on a single element.

Encoding is actually _relatively_ straightforward; After loading the image into a [`canvas`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) element, we can iterate over every pixel and use [`CanvasRenderingContext2D.getImageData()`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/getImageData) to get the color data (as a tuple of the red, green, blue and alpha channel respectively) for this pixel. We can then construct a single box shadow for this pixel with the same position and color.

As reference image we will use an [image of the Mona Lisa from Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Mona_Lisa,_by_Leonardo_da_Vinci,_from_C2RMF_retouched.jpg) (161 × 240 pixels):
![Image of the Mona Lisa](./161px-Mona_Lisa,_by_Leonardo_da_Vinci,_from_C2RMF_retouched.jpg)

The resulting `box-shadow`-encoded representation of this image looks like this:

```
0px 0px rgb(79 108 77 / 1),
1px 0px rgb(74 103 72 / 1),
2px 0px rgb(80 109 78 / 1),
3px 0px rgb(80 109 78 / 1),
4px 0px rgb(66 97 65 / 1),
5px 0px rgb(63 94 62 / 1), /* Further pixels of first row omitted */
0px 1px rgb(87 114 83 / 1),
1px 1px rgb(82 109 78 / 1),
2px 1px rgb(87 114 83 / 1); /* All further pixels omitted */
```

If we now use this as the value for an elements `box-shadow`, the shadow will look like the original image!

[The result and encoder can be found on CodePen](https://codepen.io/FelixRilling/full/xxdrwRe)
[Download source code](TODO)

In the case of our example image which originally was a JPEG with a file size of 11.8KiB, just the `box-shadow` value is roughly 1.1MiB! - now thats what I call inefficient! I originally wanted to measure how long browsers take to paint the `box-shadow` image, but between Firefox devtools becoming extremely slugish when opening them while the `box-shadow` was visible, and Chromium crashing altogether, I decided that more in-depth analysis is not required.

## Optimizations

Even though we are trying to be inefficient here, I thought it would be fun to optimize this a bit.

### Smaller Tweaks

-   Omit the 'px' if a pixel value is zero. The unit is redundant in this case and we can just write `0`.
-   Omit the alpha channel of a color if it is fully opaque, as that is the default.
-   Omit fully transparent pixels altogether, as the do not change the visual output at all.

### Color Notation

I ended up using hexadecimal notation (e.g. `#ff00c0`) instead of the RGB color function notation (e.g. `rgb(255 0 192)`). This saves a few characters, and even allows us to use the shorthand notation (e.g. `#f0a`) where applicable.
I originally opted for the RGB color function notation because I assumed that I needed to handle images with colors more precise than the single byte per channel the hexadecimal notation supports, and only could be represented in the RGB function notation (e.g. using `rgb(0.25 64.075 192 / 33.333%)`). This however turned out to be irrelevant later on as the [`ImageData`](https://developer.mozilla.org/en-US/docs/Web/API/ImageData) object returned from the canvas only uses integers from 0 to 255 for color channel values, which means a single byte per channel and thus the hexadecimal notation is sufficient.

### Merging Adjacent Pixels

TODO

## Other Inefficient Ways to Encode Images in CSS

Originally I wanted to showcase other approaches to encode images as well in this article, but ended up not doing so due to most of them either being very similar to the approach described above (e.g. using nested pseudo-elements like `:before`/`:after` for each pixel, or using a gradient for each row with a color stop for each pixel) or too complex (converting vector based graphics to pseudo-elements and drawing them using CSS properties).

If you have an idea for a cool way to encode images in CSS, feel free to [send me a mail](/contact) and I will try to showcase it here!
