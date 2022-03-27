---
title: "Getting into JavaScript Building & Bundling: Module Types"
date: 2016-07-19
updated: 2021-06-26
tags:
    - JavaScript
    - Modules
    - Workflow
---

The last few weeks I did some research on the different ways to bundle JavaScript dependencies, here is what I found out;
Most of the time when working on a project with more than a few lines of JavaScript you'll want to split up the code into multiple files to make the structure easier to read and understand, or to make testing easier - but how do you manage the actual importing/exporting of your code parts or 3rd party libraries?

<!-- more -->

## CommonJS

[CommonJS](https://en.wikipedia.org/wiki/CommonJS) is a module specification made popular by [Node.js](https://nodejs.org/en/). Did you ever use Node.js `require()` or `module.exports`? - that is CommonJS. Being used in hundreds of thousands of Node.js packages, it is safe to say that CommonJS is the module type with the widest adoption right now. [Webpack](https://webpack.js.org/) and [Browserify](https://browserify.org/) are popular build tools focusing on this format. When targeting Node.js, no build tools are needed as this module format is supported natively.

CommonJS Syntax Example:

```js
// Module file 'foo.js'
module.exports = {
	bar: function () {
		return "fooBar";
	},
};
```

```js
// Main file
const foo = require("./foo");
foo.bar();
```

Pro:

-   Widely adopted.
-   Numerous build tools support it.
-   Can be used without any bundling in Node.js.
-   Can dynamically load script files at runtime.

Con:

-   Some glue code is needed at runtime (when running in the browser).

## ES Modules

[ES Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules), an [ECMAScript standard](https://tc39.es/ecma262/#sec-modules), is the youngest in the family of module types. It is natively [supported by modern browsers](https://developer.mozilla.org/en/docs/web/JavaScript/reference/statements/import#Browser_compatibility), which notably excludes IE 11. However, this doesn't stop us from using it if we have to support IE 11, since we have module bundling tools, like [webpack](https://webpack.js.org/) or [rollup](https://rollupjs.org/guide/en/) which can create a regular JavaScript file that also works in IE 11. Note that it is common to use these bundling tools even if IE 11 is not required since bundling and related operations that can be done during the build can reduce the total file size and thus improve performance. [Node.js also supports ES Modules](https://nodejs.org/api/esm.html) natively but has some restrictions such as forcing the use of file extensions when specifying which file to import.

ES Module Syntax Example:

```js
// Module file 'foo.js'
export const foo = {
	bar: function () {
		return "fooBar";
	},
};
```

```js
// Main file
import { foo } from "./foo.js";
foo.bar();
```

Pro:

-   Official ECMAScript standard.
-   No build tools or boilerplate code is required if the browser/runtime supports it.
-   A large amount of build tools support it.
-   Zero runtime overhead.

Con:

-   If IE 11 support is needed, a build step is required.

## Historical

The following were often used in the past but are largely irrelevant nowadays.

### Concating

The oldest way is to just mash every file into one huge file that contains all dependencies and code parts in a working order. While concating is easy to do, many issues can occur with this technique, as the tooling usually has no idea which parts depend on which other parts.
Tools that can be used for this are for example [Grunt](https://gruntjs.com/) which has [grunt-contrib-concat](https://github.com/gruntjs/grunt-contrib-concat).

Pro:

-   Requires minimal build tool functionality.
-   No runtime overhead.

Con:

-   Keeping track of in which order files should be concatinated can become extremely hard.
-   Requires (either manual or automatic) wrapping of file contents in [IIFEs](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) to avoid polluting the global scope.
-   Cannot dynamically load script files at runtime. Additional code is needed to achieve this.

### AMD

AMD (not to be confused with the hardware company) stands for ["Asynchronous Module Definition"](http://requireJS.org/docs/whyamd.html#amd) and was the first popular approach to modules in the JavaScript ecosystem. A noteworthy feature is the ability to dynamically load modules at runtime, as well as optionally 'optimizing' them at build-time. As the bundling format used by [RequireJS](http://requireJS.org/), it gained quite some popularity in the past which however is declining since CommonJS got popular.

AMD Syntax Example:

```js
// Module file 'foo.js'
define(["foo"], function () {
	return {
		bar: function () {
			return "fooBar";
		},
	};
});
```

```js
// Main file
require(["foo"], function (foo) {
	foo.bar();
});
```

Pro:

-   Allows for more granular control of module dependencies than concating.
-   Can dynamically load script files at runtime.

Con:

-   Declining popularity.
-   Quite verbose and requires more setup code.
-   Some glue code is needed at runtime.

## Conclusion

I tend to use ES modules where possible, but CommonJS is still a valid choice. I would avoid using AMD or concating unless you are working on a legacy project that already uses them.

[Continue reading about JavaScript Building & Bundling in the second part of this series.](https://rilling.dev/getting-into-javascript-building-and-bundling-part-2-bundling-tools)
