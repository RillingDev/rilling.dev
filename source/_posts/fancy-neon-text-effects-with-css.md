---
title: "Fancy Neon Text Effects with CSS"
date: 2015-07-04
updated: 2021-09-14
tags:
    - CSS
    - Neon
    - Text-Shadow
---

This article describes how CSS can be used to make text look like neon signs. For an example, check out [my CodePen showcasing this effect](https://codepen.io/FelixRilling/pen/qzfoc).
The neon effect mostly is achieved by using the CSS property [`text-shadow`](https://developer.mozilla.org/en-US/docs/Web/CSS/text-shadow) with multiple layers and a saturated colour, combined with some CSS animations to make it look more realistic.

The most important part is the [`text-shadow`](https://developer.mozilla.org/en-US/docs/Web/CSS/text-shadow) - we won't use it for an actual shadow but the blurred colored glow effect of the text. Each line in the value represents one of the radiuses of the glow (e.g. The first part of the value creates a 'shadow' with a 10px radius). In most cases we want the inner shadows to be a brighter version of the colour or even white, while the outer ones have a less bright version of the color.

<!-- more -->

```css
body {
	background-color: #424242;
}

.neon-text {
	color: #ffffff;
	text-shadow: 0 0 10px #ffffff, 0 0 20px #ffffff, 0 0 30px #ffffff, 0 0 40px
			#228dff, 0 0 70px #228dff, 0 0 80px #228dff, 0 0 100px #228dff, 0 0
			150px #228dff;
}
```

If we want, we can also animate this; The [`animation`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation) attribute and a corresponding [`@keyframes`](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes) can be used here to animate the `text-shadow`:

```css
.neon-text {
	color: #ffffff;
	text-shadow: 0 0 10px #ffffff, 0 0 20px #ffffff, 0 0 30px #ffffff, 0 0 40px
			#ff1177, 0 0 70px #ff1177, 0 0 80px #ff1177, 0 0 100px #ff1177, 0 0
			150px #ff1177;

	/* 
     * Specifies the keyframe rule name, the duration, an optional easing,
     * as well as that we want to animation to be repeated infinitely, going back and forth. 
     */
	animation: neon-animation 1.5s ease-in-out infinite alternate;
}

/* Keyframe rule for going from a large total 'shadow'-radius to a smaller one. */
@keyframes neon-animation {
	from {
		text-shadow: 0 0 10px #ffffff, 0 0 20px #ffffff, 0 0 30px #ffffff, 0 0
				40px #ff1177, 0 0 70px #ff1177, 0 0 80px #ff1177,
			0 0 100px #ff1177, 0 0 150px #ff1177;
	}
	to {
		text-shadow: 0 0 5px #ffffff, 0 0 10px #ffffff, 0 0 15px #ffffff, 0 0
				20px #ff1177, 0 0 35px #ff1177, 0 0 40px #ff1177,
			0 0 50px #ff1177, 0 0 75px #ff1177;
	}
}
```

That's all! Feel free to experiment with different colors, 'shadow's or animations.
