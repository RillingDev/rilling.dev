---
title: 'Getting into JavaScript Building & Bundling. Part 2: Bundling Tools'
published: true
date: '2016-07-20 17:14'
taxonomy:
    category:
        - JavaScript
    tag:
        - JavaScript
        - Modules
        - Workflow
visible: true
icon: file-code-o
---

This is the second part of my series about JavaScript Bundling & Building. If you haven't read [the first Part](http://f-rilling.com/getting-into-javascript-building-and-bundling-part-1-modules) yet, go check it out, otherwise the content of this part might not make sense to you.

## Bundling Tool Overview

let's start by taking a look at this table:

Feature              | RequireJS | Browserify | Webpack | Rollup
-------------------- | --------- | ---------- | ------- | ------
Supports AMD         | yes       | no         | yes     | no
Supports CommonJS    | no        | yes        | yes     | yes
Supports ES6 Modules | no        | no         | no      | yes
Runs Standalone      | yes       | yes        | yes     | yes
Runs with Grunt      | yes       | yes        | yes     | yes
Runs with Gulp       | yes       | yes        | yes     | yes
Project Stars        | 9501      | 9948       | 16811   | 5166
Project Contributors | 97        | 158        | 145     | 40
Project Commits/m *  | 0         | 0          | 54      | 113

**Data: Github and Webpack docs; 20.07.2016**

_(*)Commits from the 01.06 to the 01.07.2016_

Let's take a closer look at the tools listed above, shall we?

### [RequireJs](http://requirejs.org)

RequireJS is probably the oldest popular module bundler, created in 2011 it's still a viable choice today. RequireJS choose [AMD](http://f-rilling.com/getting-into-JavaScript-building-and-bundling-part-1-modules) as the primary(and only) syntax for module definition. Different from most other bundling tools, RequireJS works more like a library than a classic bundling tool: simply drop require.JavaScript and the config.JavaScript in your project and you're ready to go.

Pro:

- Easy/quick to use
- No build process needed

Con:

- Large file size
- Only supports AMD

### [Browserify](http://browserify.org/)

With almost 10000 Stars on Github, Browserify is the second largest bundling tool on the list. Browserify works as a command-line tool that supports the [CommonJS](http://f-rilling.com/getting-into-JavaScript-building-and-bundling-part-1-modules) syntax. One of the advantages of Browserify is the simple setup: no config files needed, just run "browserify", or the [grunt](https://www.npmjs.com/package/grunt-browserify)/[gulp](https://www.npmjs.com/package/gulp-browserify) plugin. Browserify is used by many popular projects, such as react, mapbox or cloudflare.

Pro:

- Easy/quick to use
- Big community

Con:

- Only supports CommonJS

### [Webpack](https://webpack.github.io/)

Webpack is the biggest and most hyped bundling system right now - and rightfully so: Even though it can be tricky to set up, once it gets running its a super powerful bundling system that does not only support AMD and CommonJS, but can be extended by all sorts of plugins that give webpack similar complexity to a grunt/gulp powered build process. Not only can webpack include bundles, it also supports transpiling & compiling JavaScript: you could build ES6 JavaScript, Cofeescript and Typescript in a single project with webpack. But webpack doesn't stop at JavaScript: it also allows bundling of CSS, images etc. etc.

Pro:

- Greatly customizable
- Can replace grunt/gulp
- Huge community

Con:

- Setup takes time

### [Rollup](http://rollupjs.org/)

Rollup is the youngest of the bundler listed but grows at a steady rate (roughly 113 commits/month!) nonetheless. Rollup specialises in ES6 modules but supports CommonJS as well. The huge advantage of Rollup/ES6 Modules is the so-called "Tree-shaking": you can specify in the import statement which parts of the file required should be included, allowing to select just one part of a library for example which allows for a much smaller and organised output file. The plugins available for Rollup allow for a build process similar to webpack, with plugins for babel or uglifyjs for example. Another cool thing about Rollup is the focus on ES6 Modules, In which I see the future of module bundling.

Pro:

- ES6 Module support
- Tree-shaking
- Quick project growth

Con:

- relatively small community

### Conclusion

Similar to the conclusion from the last part, every bundler has its point, and its up to you to decide when and how to use them.

- RequireJS is good for legacy projects and people who don't want to build files every time they change something
- Browserify is for people who want to start quick and use CommonJS
- Webpack if you want one beefy tool that does everything
- Rollup is for developers who care about performance and file size, while keeping up with latest ES standards
