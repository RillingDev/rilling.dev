---
title: "Fancy Neon Text Effects with CSS"
date: 2015-07-04
updated: 2023-08-26
extra:
  tags:
    - CSS
description: "This article describes how to use CSS to make text look like neon signs."
---

This article describes how to use CSS to make text look like [neon signs](https://en.wikipedia.org/wiki/Neon_sign). You can see the result in [my CodePen showcasing this effect](https://codepen.io/RillingDev/pen/qzfoc).

The core of this technique is using the CSS property [`text-shadow`](https://developer.mozilla.org/en-US/docs/Web/CSS/text-shadow) with multiple layers and a saturated color. The `text-shadow` is not used for an actual shadow, but the blurred and colorful glow effect around the text.

Each line in the value of the property represents one of the [radii](https://en.wikipedia.org/wiki/Radius) of the glow (for example, the first part of the value creates a "shadow" with a 10px radius). We want the inner shadows to be a brighter version of the color or even white, while the outer ones have a less bright version of the color.

<!-- more -->

```css
body {
	/* Use a dark background to make the neon text stand out */
	background-color: #424242;
}

.neon-text {
	color: #ffffff;
	/*
	 * Start with very bright inner shadows,
	 * and get less bright towards the outside
	 */
	text-shadow:
		0 0 10px #ffffff,
		0 0 20px #ffffff,
		0 0 30px #ffffff,
		0 0 40px #228dff,
		0 0 70px #228dff,
		0 0 80px #228dff,
		0 0 100px #228dff,
		0 0 150px #228dff;
}
```

This is then combined with some CSS animations to make it look more realistic and "pulsating". The [`animation`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation) attribute and corresponding [`@keyframes`](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes) allow animating the `text-shadow`:

```css
.neon-text {
	color: #ffffff;
	text-shadow:
		0 0 10px #ffffff,
		0 0 20px #ffffff,
		0 0 30px #ffffff,
		0 0 40px #ff1177,
		0 0 70px #ff1177,
		0 0 80px #ff1177,
		0 0 100px #ff1177,
		0 0 150px #ff1177;

	/* 
     * Specifies the keyframe rule name, the duration, an optional easing
     * and that we want to animation to be repeated infinitely,
     * going back and forth. 
     */
	animation: neon-animation 1.5s ease-in-out infinite alternate;
}

/* Keyframe rule for going from larger radii to smaller ones. */
@keyframes neon-animation {
	from {
		text-shadow:
			0 0 10px #ffffff,
			0 0 20px #ffffff,
			0 0 30px #ffffff,
			0 0 40px #ff1177,
			0 0 70px #ff1177,
			0 0 80px #ff1177,
			0 0 100px #ff1177,
			0 0 150px #ff1177;
	}
	to {
		text-shadow:
			0 0 5px #ffffff,
			0 0 10px #ffffff,
			0 0 15px #ffffff,
			0 0 20px #ff1177,
			0 0 35px #ff1177,
			0 0 40px #ff1177,
			0 0 50px #ff1177,
			0 0 75px #ff1177;
	}
}
```
