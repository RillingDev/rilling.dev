---
title: "Highly Inefficient Ways to Store Images in CSS"
date: 2021-07-17
updated: 2021-07-17
tags:
    - CSS
    - Humor
---

If you regularly work with CSS, chances are that you are used to referencing images from CSS code. Usually this is done with the [`url()`](<https://developer.mozilla.org/en-US/docs/Web/CSS/url()>) function which can be used with an URL for an image file or to directly embed a base64 encoded image.

But what can we do if these approaches are not exotic enough for our tastes? What if rendering images like this is just way too easy and does not have a big enough performance impact? Well, you're in luck.

<!-- more -->

## Abusing CSS Shadows

The idea for this article came to me when I looked through some relatively old projects of mine from shortly after I learned how to program. One of those projects used JavaScript to convert an image file to a value that can be used with the CSS [`box-shadow`](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow) property in such a way that the box shadow produced looks like the original image.

In order to understand how this works, it is important to note that `box-shadow` and the closely related `text-shadow` in CSS have several properties that allow us to abuse them to freely draw an arbitrary amount of rectangular shapes that do not actually look like shadows: A shadow can take an X and Y offset, an optional blur radius (which we will not use, as we do not actually want the output to look like a shadow), an optional spread radius (not needed in our case, as the default of `1px` is fine), and the color of the shadow. This notation can be repeated any number of times, allowing for multiple shadows on a single element.

Encoding an existing image for usage as a shadow is actually _relatively_ straightforward; After loading the image into a [`canvas`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) element, we can iterate over every pixel and use [`CanvasRenderingContext2D.getImageData()`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/getImageData) to get the color data (as a tuple of the red, green, blue and alpha channel respectively) for this pixel. We can then construct a single shadow for this pixel with the same position and color.

As reference images we will use the following images of Wikimedia Commons:

1.   [AdditiveColor](https://commons.wikimedia.org/wiki/File:AdditiveColor.svg) as an image with primitive shapes and few different colors.
2.   [PNG transparency demonstration 1](https://commons.wikimedia.org/wiki/File:PNG_transparency_demonstration_1.png) as an image with full and partial transparency.
3.   [Mona Lisa, by Leonardo da Vinci, from C2RMF retouched](https://commons.wikimedia.org/wiki/File:Mona_Lisa,_by_Leonardo_da_Vinci,_from_C2RMF_retouched.jpg) (161 × 240 pixels) as an image with a lot of different colors and complex shapes.

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

In the case of our example image 3, which originally was a JPEG with a file size of 11.8KiB, just the `box-shadow` value is roughly 1.1MiB - now that's what I call inefficient! I originally wanted to measure how long browsers take to paint the `box-shadow` image, but between Firefox devtools becoming extremely sluggish when opening them while the newly produced image was visible, and Chromium crashing altogether, I decided that more in-depth analysis is not required.

## Optimizations

Even though we are trying to be inefficient here, I thought it would be fun to optimize this a bit more:

### Smaller Tweaks

-   Omit the '`px`' if a pixel value is zero. The unit is redundant in this case and we can just write `0`.
-   Omit the alpha channel of a color if it is fully opaque, as that is the default.
-   Omit fully transparent pixels altogether, as they do not change the visual output at all.

### Color Notation

I ended up using hexadecimal notation (e.g. `#ff00c0`) instead of the RGB color function notation (e.g. `rgb(255 0 192)`). This saves a few characters, and even allows us to use the shorthand notation (e.g. `#f0a`) where applicable.
I originally opted for the RGB color function notation because I assumed that we need to handle images with colors more precise than the single byte per channel the hexadecimal notation supports which could only be represented in the RGB function notation (e.g. using `rgb(0.25 64.075 192 / 33.333%)`). This however turned out to be irrelevant later on as the [`ImageData`](https://developer.mozilla.org/en-US/docs/Web/API/ImageData) object returned from the canvas only uses integers from `0` to `255` for color channel values, which means a single byte per channel and thus the hexadecimal notation is sufficient.

After these optimizations the example image 3 `box-shadow` value size has been decreased to 710.8KiB.

### Further Optimizations That Were Not Implemented

Using a single shadow with increased spread size could be used if multiple adjacent pixels in the shape of a rectangle have the same color. If we increase the default spread of `1px` to `2px`, we effectively get a 2 by 2 rectangle in the given color with just one shadow. Not implemented as the logic for finding applicable situations seems rather complex.

## Other Inefficient Ways to Encode Images in CSS

Originally I wanted to showcase other approaches to encode images as well in this article, but ended up not doing so due to most of them either being very similar to the approach described above (e.g. using nested pseudo-elements like `:before`/`:after` for each pixel, or using a gradient for each row with a color stop for each pixel) or too complex (converting vector based graphics to pseudo-elements and drawing them using CSS properties).

If you have an idea for a cool way to encode images in CSS, feel free to [send me a mail](/contact) and I will try to showcase it here.
