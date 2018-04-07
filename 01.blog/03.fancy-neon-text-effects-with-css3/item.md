---
title: 'Fancy Neon Text Effects with CSS3'
published: true
visible: true
icon: lightbulb-o
date: '04-07-2015 15:14'
taxonomy:
    category:
        - CSS
    tag:
        - CSS
        - Design
        - Text
---

Did you ever want to create fancy neon effects? No? Well...to bad I'll explain how to create them anyways. Neon text mostly is achieved by using the CSS3 property "text-shadow" on multiple layers and a saturated colour, as well as some animations to look more realistic.

[Live Example](http://f-rilling.com/projects/NeonText/)


The most important are the "text-shadow" - We won't use it for an actual shadow but for the glow effect of the text. Each line in the value represents one of the radiuses of the glow (i.e. The first line creates the glow with a 10px radius). In most cases we want the inner shadows to be a brighter version of the colour or even white, while the outer ones have the neon-color.

```css
body{
  background-color:#424242;
}
.neon{
  text-align:center;
  font-size:40px;
  margin:20px 0 20px 0;
  color:#ffffff;
  /*Glow*/
  text-shadow: 0 0 10px #ffffff,
             0 0 20px  #ffffff,
             0 0 30px  #ffffff,
             0 0 40px  #228DFF,
             0 0 70px  #228DFF,
             0 0 80px  #228DFF,
             0 0 100px #228DFF,
             0 0 150px #228DFF;
}
```

If we want, we can also animate this!

We want to use an "animation" attribute here for the glow effect, don't forget the vendor prefixes "-webkit-" and "-moz-"! Right now the animation still needs its key frames to work

```css
.neon{
  text-align:center;
  font-size:40px;
  margin:20px 0 20px 0;
  color:#ffffff;
  text-shadow: 0 0 10px #ffffff,
             0 0 20px  #ffffff,
             0 0 30px  #ffffff,
             0 0 40px  #228DFF,
             0 0 70px  #228DFF,
             0 0 80px  #228DFF,
             0 0 100px #228DFF,
             0 0 150px #228DFF;
  /*Animation*/
  -webkit-animation: neonAnimation 1.5s ease-in-out infinite alternate;
  -moz-animation: neonAnimation 1.5s ease-in-out infinite alternate;
  animation: neonAnimation 1.5s ease-in-out infinite alternate;   
}
```

For the key frames we need to use the vendor prefixes again, like this:

```css
/*Webkit prefix*/
      @-webkit-keyframes neonAnimation {
        from {
          text-shadow: 0 0 10px #ffffff,
                     0 0 20px  #ffffff,
                     0 0 30px  #ffffff,
                     0 0 40px  #228DFF,
                     0 0 70px  #228DFF,
                     0 0 80px  #228DFF,
                     0 0 100px #228DFF,
                     0 0 150px #228DFF;
        }
        to {
          text-shadow: 0 0 5px #ffffff,
                     0 0 10px #ffffff,
                     0 0 15px #ffffff,
                     0 0 20px #228DFF,
                     0 0 35px #228DFF,
                     0 0 40px #228DFF,
                     0 0 50px #228DFF,
                     0 0 75px #228DFF;
        }
      }

      /*Moz prefix*/
      @-moz-keyframes neonAnimation {
        from {
          text-shadow: 0 0 10px #ffffff,
                     0 0 20px  #ffffff,
                     0 0 30px  #ffffff,
                     0 0 40px  #228DFF,
                     0 0 70px  #228DFF,
                     0 0 80px  #228DFF,
                     0 0 100px #228DFF,
                     0 0 150px #228DFF;
        }
        to {
          text-shadow: 0 0 5px #ffffff,
                     0 0 10px #ffffff,
                     0 0 15px #ffffff,
                     0 0 20px #228DFF,
                     0 0 35px #228DFF,
                     0 0 40px #228DFF,
                     0 0 50px #228DFF,
                     0 0 75px #228DFF;
        }
      }

      /*No prefix*/
      @keyframes neonAnimation {
        from {
          text-shadow: 0 0 10px #ffffff,
                     0 0 20px  #ffffff,
                     0 0 30px  #ffffff,
                     0 0 40px  #228DFF,
                     0 0 70px  #228DFF,
                     0 0 80px  #228DFF,
                     0 0 100px #228DFF,
                     0 0 150px #228DFF;
        }
        to {
          text-shadow: 0 0 5px #ffffff,
                0 0 10px #ffffff,
                0 0 15px #ffffff,
                0 0 20px #228DFF,
                0 0 35px #228DFF,
                0 0 40px #228DFF,
                0 0 50px #228DFF,
                0 0 75px #228DFF;
  }
}
```
