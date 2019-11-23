---
title: "Getting into JavaScript Building & Bundling. Part 2: Bundling Tools"
icon: file-code
date: 2016/07/20
updated: 2019/05/27
tags:
    - JavaScript
    - Modules
    - Workflow
---

This is the second part of my series about JavaScript bundling & building. If you haven't read [the first part](https://rilling.dev/getting-into-javascript-building-and-bundling-part-1-modules) yet, go check it out, otherwise the content of this part might not make sense to you.

<!-- more -->

## Bundling Tool Overview

let's start by taking a look at this table:

| Feature              | RequireJS | Browserify | Webpack | Rollup |
| -------------------- | --------- | ---------- | ------- | ------ |
| Supports AMD         | yes       | no         | yes     | no     |
| Supports CommonJS    | no        | yes        | yes     | yes    |
| Supports ES6 Modules | no        | no         | yes     | yes    |
| Runs Standalone      | yes       | yes        | yes     | yes    |
| Runs with Grunt      | yes       | yes        | yes     | yes    |
| Runs with Gulp       | yes       | yes        | yes     | yes    |
| Project Stars        | 9501      | 12875      | 51050   | 16541  |
| Project Contributors | 101       | 180        | 546     | 183    |
| Project Commits/m \* | 0         | 1          | 63      | 44     |

**Data: Github; 22.09.2019**

_(\*)Commits from the 22.08 to the 22.09.2019_

Let's take a closer look at the tools listed above, shall we?

### [RequireJs](http://requirejs.org)

RequireJS is probably the oldest popular module bundler, created in 2011 it's still a thing today. RequireJS chooses [AMD](http://rilling.dev/getting-into-JavaScript-building-and-bundling-part-1-modules) as the primary (and only) syntax for module definitions. Different from most other bundling tools, RequireJS works more like a library than a classic bundling tool: simply drop require.js and the config.js in your project and you're almost ready to go.

Pro:

-   Easy/quick to use.
-   No build process needed.

Con:

-   Large file size.
-   Only supports AMD.
-   Largely inactive

### [Browserify](http://browserify.org/)

With over 10000 Stars on Github, Browserify is the third largest bundling tool on the list. Browserify works as a command-line tool that supports the [CommonJS](https://rilling.dev/getting-into-JavaScript-building-and-bundling-part-1-modules) syntax. One of the advantages of Browserify is the simple setup: no config files needed, just run "browserify", or the [grunt](https://www.npmjs.com/package/grunt-browserify)/[gulp](https://www.npmjs.com/package/gulp-browserify) plugin.

Pro:

-   Easy and quick to use.

Con:

-   Only supports CommonJS.

### [Webpack](https://webpack.github.io/)

Webpack is the biggest and most popular bundling system - and rightfully so: Even though it can be a bit tricky to set up, once it gets running its a very powerful bundling system that does not only support all popular bundling syntax, it can also be extended by all sorts of plugins that give webpack similar complexity to a grunt/gulp powered build process. Not only can webpack build bundles, it also supports transpiling JavaScript through Babel or Typescript. But webpack doesn't stop at JavaScript: it also allows bundling of CSS, images etc.

Pro:

-   Supports all popular bundling syntax.
-   Greatly customizable.
-   Can replace grunt/gulp.
-   Huge community.

Con:

-   Setup takes time.

### [Rollup](http://rollupjs.org/)

Rollup is the youngest of the bundler listed but grows at a steady rate nonetheless. Rollup specializes in ES6 modules but supports CommonJS as well. The advantage of Rollup/ES6 modules is the so-called "Tree-shaking": you can specify in the import statement which parts of the file required should be included, allowing to select just one part of a library for example, which allows for a much smaller output file. The plugins available for Rollup allow for a build process similar to webpack, with plugins for Babel or UglifyJS for example. Another cool thing about Rollup is the focus on ES6 modules, which seem like the future of module bundling.

Pro:

-   ES6 module support.
-   Quick project growth.

Con:

-   No AMD support.
-   Smaller community than e.g. Webpack

### Conclusion

-   RequireJS is good for legacy projects and developers who don't want to build files every time they change something.
-   Browserify is for projects where you want to start quick and only use CommonJS.
-   Webpack if you want one beefy tool that can do everything very well, but requires more setup.
-   Rollup is similar to Webpack, but with less trouble setting it up in exchange for slightly less module syntax supported.
