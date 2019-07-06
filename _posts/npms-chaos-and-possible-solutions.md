---
title: "NPM's Chaos and Possible Solutions"
published: true
date: "2018-12-12 15:14"
taxonomy:
    category:
        - JavaScript
    tag:
        - JavaScript
        - NPM
        - Development
visible: true
icon: book-reader
---

By now there are probably more articles about the flaws of [NPM](https://github.com/npm/cli) and the [NPM registry](https://www.npmjs.com/) than articles about how Java will die soon, but I feel like there haven't been many of them which suggest solutions or workarounds to these problems, so these are the aspects I want to try to focus on in this article.

## The Problems

Most of the articles I read in the past months regarding NPM boil down to a handful of problems:

1.  Quantity over quality - there are way too many packages, a lot of them redundant or of low quality.
2.  Needlessly deep and big dependency trees - a single, simply package often comes with hundreds of transient dependencies.
3.  Handling of package ownership and removal - see the infamous ["left-pad"](https://github.com/stevemao/left-pad/issues/4) and ["event-stream"](https://github.com/dominictarr/event-stream/issues/116) incidents.
4.  Lack of security (often caused by points 2 and 3).

3 and 4 are problems only the NPM team can fix, and honestly I don't see any simple solution for them. So lets focus on 1 and 2, which can fixed by package authors and consumers.

### How Authors can Help Fix the Problems

A good, high quality library or tool is good at what it does, having a clean code base which is covered by tests and handles edge-cases, while also making sure to have as few dependencies as possible, and these dependencies having a certain quality as well.

Here are some questions to ask yourself before publishing a package:

-   Does the code of this package justify publishing it for re-use? An infamous example of this is "is-even" which is...inverting the result of the package "is-odd". If the content of your package can be written within seconds by someone else instead of adding the package as a dependency, or the same functionality exists in the standard library or a popular utility library like [lodash](https://lodash.com/), you probably wont need this as its own package.
-   Does something like this already exists? is it worth it to publish this rather than making a pull request to an existing package? (I've been guilty of ignoring this a few times myself)
-   Are the dependencies of my package acceptable? Are all of them required? Are there any which have a large amount of dependencies themselves? are they trustworthy?
-   Am I willing to maintain this package? This one is a bit tricky; While you should _try_ to commit to maintaining your package, the GitHub user dominictarr also wrote [a response to this regarding the "event-stream" incident](https://gist.github.com/dominictarr/9fd9c1024c94592bc7268d36b8d83b3a) with good points, showing why this one is not always possible.

### How Consumers of Packages can Avoid the Problems

It is always a good idea to stick to popular, well tested and reviewed packages if possible, but to me it seems like finding those is not quite that easy; Usually the best way is to either search for the problem through a search engine, or search StackOverflow or GitHub and other source code repository hosts directly. For Java there is the very neat [Apache Commons project](https://commons.apache.org/) which is basically a collection of high quality libraries for a large number of purposes, which makes for a great hub when looking for a library for a given problem. Sadly nothing like this exists for JavaScript (yet).
