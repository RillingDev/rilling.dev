---
title: "JavaScript Building & Bundling: Bundling Tools"
date: 2016-07-20
updated: 2021-06-27
tags:
    - JavaScript
    - Modules
    - ESM
---

This article aims to explore and compare the different tools available for JavaScript bundling.

This is the second part of this series. If you haven't read [the first part about module types](/blog/javascript-building-and-bundling-modules/) yet, you may want to do so.

<!-- more -->

<!--
TODO: update for
- esbuild (and vite)
- webpack (and vite)
- rollup
- parcel
- turbo
- old: browserify
- old: snowpack
-->

## Bundling Tool Overview

Let's start by taking a look at this table:

| Feature              | Browserify | Webpack | Rollup |
| -------------------- | ---------- | ------- | ------ |
| Supports CommonJS    | yes        | yes     | yes    |
| Supports ES Modules  | no         | yes     | yes    |
| Project Stars        | 12875      | 51050   | 16541  |
| Project Contributors | 180        | 546     | 183    |
| Project Commits/m \* | 1          | 63      | 44     |

**Source: GitHub; 22.09.2019.**

_(\*) Commits from 22.08 to 22.09.2019._

_Update 2021: Several tools such as [Snowpack](https://www.snowpack.dev/), [Parcel](https://parceljs.org/), or [esbuild](https://esbuild.github.io/) have risen in popularity recently, but are not covered by this article._

Let's take a closer look at the tools listed above, shall we?

<!-- TODO: remove this section -->

### [Browserify](http://browserify.org/)

With over 10000 Stars on GitHub, Browserify is the third-largest bundling tool on the list. Browserify works as a command-line tool that supports the [CommonJS](/blog/javascript-building-and-bundling-modules/#commonjs) module type. One of the advantages of Browserify is the simple setup: no config files are needed - Just run `browserify` with your input/output files.
Note that, unlike the other tools in this list, browserify seems to usually be used in conjunction with tools like Gulp, because other common tasks like transpiling or minifying are not handled by it.

Pro:

- Quick and easy to use.

Con:

- Only supports CommonJS.
- Usually has to be used together with other build tools.

### [Webpack](https://webpack.github.io/)

Webpack is the biggest and most popular bundling system in the JavaScript ecosystem. Even though it can be a bit tricky to set up, once it gets running it is a very powerful bundling system that does not only support every popular module type, it can also be extended by all sorts of plugins that give webpack similar functionality to Grunt or Gulp powered build process. Not only can webpack build bundles, but it also supports transpiling JavaScript through Babel or Typescript, as well as optimizing code via e.g. Terser. However, webpack doesn't stop at JavaScript: it also allows bundling of CSS, images, and a lot more.
One thing that should be noted is that webpack tends to include some runtime code for resolving modules in the final output, which might not be desired when bundling libraries.

Pro:

- Supports every popular module syntax.
- Greatly customizable.
- Can be used for pretty much everything a build process needs.
- Huge community.

Con:

- Configuration can be complex, especially with many plugins.

### [Rollup](http://rollupjs.org/)

Rollup is the youngest of the bundler listed but grows at a steady rate nonetheless. Rollup specializes in ES modules but can support CommonJS as well. The plugins available for Rollup allow for a build process similar to webpack, with plugins for Babel or UglifyJS for example.
Unlike webpack, no runtime code for resolving modules is injected, which makes Rollup a good choice for libraries.

Pro:

- Supports every popular module syntax.
- Greatly customizable.
- Can be used for pretty much everything a build process needs.
- Quick project growth.
- Zero runtime overhead.

Con:

- Smaller community than Webpack.

### Conclusion

- Browserify is for projects where you want to start quickly and only use CommonJS, and/or projects which already have a build process involving e.g. Gulp.
- Webpack if you want one beefy tool that can do everything very well, but can require more setup.
- Rollup is similar to Webpack, but with less trouble setting it up in exchange for a slightly less mature ecosystem and a smaller community behind it.
