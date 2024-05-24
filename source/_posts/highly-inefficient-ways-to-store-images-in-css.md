---
title: "Highly Inefficient Ways to Store Images in CSS"
date: 2021-08-29
updated: 2023-09-03
tags:
	- CSS
	- Humor
description: "This article explores silly ways to embed images using CSS."
---

If you regularly work with CSS, chances are that you have referenced images from CSS code. Usually, this is done with the [`url()`](<https://developer.mozilla.org/en-US/docs/Web/CSS/url()>) function, which takes a URL for an image file or directly embeds a base64 encoded image.
But what can we do if these approaches are not exotic enough for our tastes? What if rendering images like this is just too easy and does not have a big enough performance impact? Well, you are in luck. This article explores silly ways to embed images using CSS.

<!-- more -->

We use the following images from Wikimedia Commons as reference images:

1. [AdditiveColor](https://commons.wikimedia.org/wiki/File:AdditiveColor.svg) is an image with primitive shapes and a few different colors.
2. [PNG transparency demonstration 1](https://commons.wikimedia.org/wiki/File:PNG_transparency_demonstration_1.png) is an image with full and partial transparency.
3. [Mona Lisa, by Leonardo da Vinci, from C2RMF retouched](https://commons.wikimedia.org/wiki/File:Mona_Lisa,_by_Leonardo_da_Vinci,_from_C2RMF_retouched.jpg) (161 × 240 pixels) is an image with a lot of different colors and complex shapes.

## Misusing CSS Shadows

The idea for this article came to me when I looked through some relatively old projects of mine. One of those projects used JavaScript to convert an image file to a value that is usable with the CSS [`box-shadow`](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow) property in such a way that the box shadow produced looks like the original image.

To understand how this works, it is important to note that `box-shadow` allow us to use them to draw an arbitrary number of rectangular shapes that do not look like shadows: A shadow can take an X and Y offset, and the color of the shadow. When not specified, the blur of the shadow defaults to `0`, which means no blur. When applied to an element that is as large as a single pixel, this allows us to draw multiple "pixel"-shadows that look nothing like a shadow. This notation can be repeated any number of times, allowing for multiple shadows on a single element.

To encode an existing image, load it into a [`canvas`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) element. Then we iterate over every pixel and use [`CanvasRenderingContext2D.getImageData()`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/getImageData) to get the color data (as a tuple of the red, green, blue, and alpha channel respectively) for this pixel. We can then build a single shadow for this pixel with the same position and color.

The resulting `box-shadow`-encoded representation of this image looks roughly like this:

<!-- @formatter:off -->

```css
.selector {
	width: 1px;
	height: 1px;
	box-shadow:
		0px 0px rgb(79 108 77 / 1),
		1px 0px rgb(74 103 72 / 1),
		2px 0px rgb(80 109 78 / 1),
		3px 0px rgb(80 109 78 / 1),
		4px 0px rgb(66 97 65 / 1),
		5px 0px rgb(63 94 62 / 1),
		/* Further pixels of first row omitted */
		0px 1px rgb(87 114 83 / 1),
		1px 1px rgb(82 109 78 / 1),
		2px 1px rgb(87 114 83 / 1);
		/* All further pixels omitted */
}
```

<!-- @formatter:on -->

If we now use this as the value for an elements `box-shadow`, the shadow looks like the original image.

Demos for the reference images (Note: they may slow down your browser):

1. [`box-shadow` version of 'AdditiveColor'](https://apps.rilling.dev/box-shadow-image/demo1.html).
2. [`box-shadow` version of 'PNG transparency demonstration 1'](https://apps.rilling.dev/box-shadow-image/demo2.html).
3. [`box-shadow` version of 'Mona Lisa, by Leonardo da Vinci, from C2RMF retouched'](https://apps.rilling.dev/box-shadow-image/demo3.html).

Depending on the hardware you use to view the preceding links, you might have noticed that the pages load rather slowly and might freeze your browser during rendering. This is because the `box-shadow` encoded image is a bit larger (Image 2 grew from roughly 222 KiB to around 2.5 MiB!). I also assume most CSS parsers are not optimized to process multiple megabytes of a single property value.

## Optimizations

Even though we are not trying to be efficient here, I thought it would be fun to optimize this a bit more:

### Smaller Tweaks

-   Omit the `px` unit if a pixel value is zero. The unit is redundant in this case, and we can just write `0`.
-   Omit the alpha channel of a color if it is fully opaque, as that is the default.
-   Omit fully transparent pixels altogether, as they do not change the visual output at all.

### Color Notation

I ended up using hexadecimal notation (for example `#ff00c0`) instead of the RGB color function notation (for example `rgb(255 0 192)`). This saves a few characters, and even allows us to use the shorthand notation (for example `#f0a`) where applicable.
I originally opted for the RGB color function notation because I assumed that we need to handle images with colors more precise than the single byte per channel the hexadecimal notation supports which you can only represent in the RGB function notation (for example `rgb(0.25 64.075 192 / 33.333%)`). This however turned out to be irrelevant later as the [`ImageData`](https://developer.mozilla.org/en-US/docs/Web/API/ImageData) object returned from the canvas only uses integers from `0` to `255` for color channel values, which means a single byte per channel, and thus the hexadecimal notation is enough.

### Further Optimizations That Were not Implemented

Using a single shadow with increased shadow spread size could be used if multiple adjacent pixels in the shape of a rectangle have the same color. If we increase the spread from `1px` to `2px`, we effectively get a 2 by 2 pixel rectangle in the given color with just one shadow. Not implemented as the logic for finding applicable situations seemed rather complex.

## Summary

While this has been a rather weird experiment, I was positively surprised that even though performance is terrible, browsers can accurately re-draw an image encoded into a `box-shadow`.

The script for encoding the images is available on [CodePen](https://codepen.io/FelixRilling/full/xxdrwRe), or you can [download it as a Gist](https://gist.github.com/FelixRilling/82a6c6efad4be263ae43ea9d8e2f23a3).

Originally, I wanted to showcase other approaches to encode images as well in this article but ended up not doing so due to most of them being either:

-   Too similar to the approach described above, for example using nested pseudo-elements like `:before`/`:after` for each pixel, or using a gradient for each row with a color stop for each pixel
-   Too complex, such as converting vector-based graphics to pseudo-elements and drawing them using CSS properties.
