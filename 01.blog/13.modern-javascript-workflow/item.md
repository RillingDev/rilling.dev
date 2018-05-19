---
title: 'Modern JavaScript Workflow'
published: true
date: '2017-06-18 15:14'
taxonomy:
    category:
        - JavaScript
    tag:
        - JavaScript
        - Workflow
        - Development
visible: true
icon: chart-line
---

While the JavaScript ecosystem is often criticized for being too complex and/or having way too many dependencies and tools (and rightfully so most of the time), we should keep in mind that most of them are used for a reason: Linters help you prevent and find bugs, bundlers and minifiers improve performance, etc.
This article tries to highlight tools which you might find useful and might improve your workflow.

## Use-cases
This article compares tools with relation to which project types profit from them. We will look at the following types:

- Traditional Website: A Static HTML or server-side-rendered website, with little JavaScript used
- Web-App: A website using quite a lof of JavaScript, most of the time powered by some kind of framework
- Library: A general purpose library that will be used/included by "real" applications
- NodeJS module: A project which is build to be used only inside NodeJS

## Tools

### Linters
Linters are almost always useful, both as CLI-tool or integrated into your editor. They help you prevent and debug much more easily, ranging from highlighting simple syntax errors, to advanced issues like out-of-scope errors or accidental assignments.

| Website | Web-App | Library | NodeJS module |
|---------|---------|---------|---------------|
| Yes     | Yes     | Yes     | Yes           |


#### [ESLint](https://github.com/eslint/eslint)
The most popular, and in my opinion, the best linter right now. ESLint has great integration into all common IDEs and editors as well as a great node module for all your linting needs. Another big advantage is the huge amount of customizable options as well as the plugins to support subsets like flow, typescript etc.

#### [JSHint](https://github.com/jshint/jshint)
I used JSHint before switching to ESLint. JSHint is a decent linter that is fast and customizable, however has less options and features than ESLint.

#### [JSLint](https://github.com/douglascrockford/JSLint)
Developed by Douglas Crockford, one of the big names in the JS community. The main advantage of this linter is the small codebase without any external dependencies, however The integration into build-tools or editors is quite limited.

### Formatters
Formatters are wonderful to enforce style-guides and are a great timesaver, once they are configured. So far I only used formatters as extension to my editors, but I'm sure integrating them into the build-chain can be useful as well.
If you are only writing small scripts, for example for a website, setting it up might be more work than doing it yourself, however in every other case I recommend using one.

| Website | Web-App | Library | NodeJS module |
|---------|---------|---------|---------------|
| Depends | Yes     | Yes     | Yes           |

#### [js-beautify](https://github.com/beautify-web/js-beautify)
I only ever used js-beautify in my projects and had a hard time finding other projects, probably for a reason: js-beautify does everything I ever wished for. Integration into all popular IDEs and editors and great customizability to tailor it to your style-guide makes working on "beautiful" code much easier.

### Minifiers
Minifiers are a great tool for every use-case where file-size and performance matters. Saving up to 70% of the original file-size in some cases can lead to great performance improvements for the end-user.

| Website | Web-App | Library | NodeJS module |
|---------|---------|---------|---------------|
| Yes     | Yes     | No      | No            |

#### [UglifyJS](https://github.com/mishoo/UglifyJS2)
UglifyJS is the biggest and most in-depth minifier that goes many steps further than simply stripping redundant spaces, tabs or line-breaks:
from mangling variable names, over replacing if-blocks with ternaries to stripping unused code, UglifyJS does it. From my experience UglifyJS is the most effective of all minifiers I used, with the only drawback being the slightly higher time it takes to process code.

#### [Google Closure Compiler](https://developers.google.com/closure/compiler/)
I never used Google Closure Compiler before, so all I say here is based on reading the descriptions.
The main advantage seems to be the REST API, which allows for much faster processing of huge code -bases.
Efficiency-wise it seems to perform slightly worse than UglifyJS.

#### [JSMin](https://github.com/douglascrockford/JSMin)
Another tool by Crockford.
While being much faster than the two other tools, this minifier only strips spaces/tabs/breaks. I would only recommend this of you value a fast build-process over efficient minification.


## Transpilers
Transpilers can allow you to use the latest ES features while supporting old browsers, or writing in a subset of JS like TypeScript or Flow to make development faster, easier and future-proof.

| Website | Web-App | Library | NodeJS module |
|---------|---------|---------|---------------|
| Maybe   | Maybe   | Maybe   | Maybe         |

### [Babel](https://github.com/babel/babel)
Babel is quite the big name in the JS community: Using all the latest ES features without having to care about browser support? Yes please.
Following that, the main use-case for babel is the web: both website and web-app development gets easier with babel. however if your code is only a few lines, you might be of better of directly writing "old" code rather than adding a new build step.

### [TypeScript](https://github.com/microsoft/typescript)
Microsoft's TypeScript is what many developers may call "JavaScript in good". TypeScript allows you to write staticly-typed code with all new ES features, and transpiles to regular JavaScript. However my personal concern is the complex setup and creating your own type-definitions. If you are working on a big project, TypeScript can help you avoid a lot of trouble and bugs - for small projects, probably not as much.

### [Flow](https://github.com/facebook/flow)
Facebook's Flow offers a similar feature as TypeScript: static typing that compiles into regular JS. The main disadvantage compared to TypeScript is the simplicity of some features, the main advantage the speed gained by using binaries.

## Bundlers
Bundlers allow you to organize your code as well as external dependencies before bundling it all up into a single file for distribution. Again, if you only have a few lines of code, you wont need this. Every other project that ends up in the browser should look into bundlers.

| Website | Web-App | Library | NodeJS module |
|---------|---------|---------|---------------|
| Maybe   | Yes     | Yes     | No            |

I wrote two in-depth articles about bundling, you can read them here:

- [Part 1: Module types](https://f-rilling.com/getting-into-javascript-building-and-bundling-part-1-modules)
- [Part 2: Bundling Tools](https://f-rilling.com/getting-into-javascript-building-and-bundling-part-2-bundling-tools)
