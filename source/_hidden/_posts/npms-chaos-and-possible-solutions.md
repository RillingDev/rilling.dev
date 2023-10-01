---
title: "NPM's Chaos and Possible Solutions"
date: 2018-12-12
updated: 2021-09-24
tags:
    - JavaScript
    - NPM
    - Development
---

By now there are probably more articles about the flaws of [NPM](https://github.com/npm/cli) and the [NPM registry](https://www.npmjs.com/) than articles about how Java will die soon, but I felt like there haven't been many of them which suggest solutions or workarounds to these problems, so these are the aspects I want to try to focus on here.

<!-- more -->

## The Problems

Most of the articles I read in the past months regarding the NPM registry boil down to a handful of problems:

1.  Quantity over quality - there are a _lot_ of packages, many of them abandoned or near-duplicates of each other (who needs [over a hundred different packages](https://www.npmjs.com/search?q=levenshtein) for calculating Levenshtein distance?).
2.  Needlessly large dependency trees - a single, simple package often comes with a ton of transient dependencies.
3.  Handling of package ownership and removal - see the infamous ["left-pad"](https://github.com/stevemao/left-pad/issues/4) and ["event-stream"](https://github.com/dominictarr/event-stream/issues/116) incidents.
4.  Lack of security (often caused by points 2 and 3). There have been several incidents of backdoors or other malicious code being inserted into NPM packages (see "event-stream" in the previous bullet point).

3 and 4 are problems only the NPM team can fix, and honestly, I don't see any simple solution for them. So let's focus on 1 and 2, which can be fixed by package authors and consumers.

### How Authors Can Help Fix the Problems

What makes a 'good' package? I would argue: A library or tool should have a clear scope, be good at what it does and handle edge cases appropriately, while also making sure to have as few dependencies as possible. Additionally, it should be actively maintained and for example, security issues should be fixed in time.

Here are some questions you could ask yourself before publishing a new package:

-   Does the code of this package justify publishing it for re-use? If the content of your package is limited to a handful of lines, it probably won't need to be its own package.
-   Does something like this already exist? Is it worth it to publish this rather than making a pull request for an existing package? (I've been guilty of ignoring this too many times myself).
-   Are all the dependencies of the package required? Are there any which have numerous dependencies themselves? Are they trustworthy?
-   Are you willing to maintain this package? This one is a bit tricky; While I would argue that you should _try_ to commit to maintaining your package, this one is not always possible. The GitHub user dominictarr also summed this up nicely in [a response to the "event-stream" incident](https://gist.github.com/dominictarr/9fd9c1024c94592bc7268d36b8d83b3a).

### How Consumers of Packages Can Avoid Problems

It is always a good idea to stick to popular, well-tested, and reviewed packages if possible. However, to me, it seems like finding those is not quite that easy in the JavaScript ecosystem when comparing it to the ecosystem of other languages, notably Java and Python; For Java, there is the very neat [Apache Commons project](https://commons.apache.org/), which is a collection of quality libraries for many purposes, which makes for a great hub when looking for a library for a given problem. Sadly nothing like this exists for JavaScript (yet), as far as I know.
