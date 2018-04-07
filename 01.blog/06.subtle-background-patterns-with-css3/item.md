---
title: 'Subtle background patterns with CSS3'
published: true
visible: true
icon: th
date: '2015-09-14 15:14'
taxonomy:
    category:
        - CSS
    tag:
        - CSS
        - Design
---

Did you ever get bored of those boring single-colored backgrounds and wanted to add some subtle patterns to your site but disliked the idea of using images to do so? Well, fear not! CSS3 introduces the repeating-linear-gradient function which makes it easy to create background patterns without the use of images! How cool is that?

[Live Example](http://f-rilling.com/projects/SubtlePatterns/)

### The repeating-linear-gradient function

The repeating-linear-gradient function is a rather complex one, as it consists of multiple values. Let's take a look at the basic version of it.

```css
body{
  background: repeating-linear-gradient(angle, color1, color2, color3, color4);
}
```

The angle defines if the gradient should be rotated (default is 0deg = horizontal) in degree, while the colour values define which colours should appear in the gradient.

### Color Stops

Right now we have soft transitions between our colours. if we want a subtle, flat look we need to adjust the colour stops. We also want color1 and color2 as well as color3 and color4 to be identical so that we have a two coloured, flat pattern:

```css
body{
  background: repeating-linear-gradient(angle, color1 stop1, color1 stop2, color2 stop3, color2 stop4);
}
```

The stops are provided as the percentage of the repeated pattern.

```css
body{
  background: repeating-linear-gradient(45deg,#e0e0e0 0,#e0e0e0 5%,#f8f8f8 0,#f8f8f8 50%) 0/10px 10px;
}
```

The default size of the repeated pattern is too big for our purposes, lets change it to be 15px x 15px by changing it to the following:

```css
body{
  background: repeating-linear-gradient(45deg, #333333 0%, #333333 25%, #444444 0%, #444444 50%) 0 / 15px 15px;
}
```

### Layering

We can also layer multiple gradients:


``` css
body{
  background:repeating-linear-gradient(90deg, #f0f0f0 0, #f0f0f0 5%, transparent 0, transparent 50%) 0 / 15px 15px ,
  repeating-linear-gradient(180deg, #f0f0f0 0, #f0f0f0 5%, transparent 0, transparent 50%) 0 / 15px 15px;
}
```

We just add another gradient after the first one, separated by a comma.  
Please note that you need to set one of the colours of the second gradient to transparent in order to be able to see the first one

### Related Links:

* [Mozilla Developer Network on repeating-linear-gradient](https://developer.mozilla.org/en-US/docs/Web/CSS/repeating-linear-gradient)
