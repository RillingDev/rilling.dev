---
title: "JavaScript Building & Bundling: Module Types"
date: 2016-07-19
updated: 2023-09-30
extra:
  tags:
    - JavaScript
description: "This article explores the different module types that exist for JavaScript."
---

When working on a project with more than a few lines of JavaScript, you will want to split up the code into multiple files to make the structure easier to read and understand. This article explores the different module types that exist for JavaScript.

<!-- more -->

## ES Modules

[ES Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) are an [ECMAScript standard](https://tc39.es/ecma262/#sec-modules). It is the youngest of the module types listed here.

It is natively [supported by modern browsers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#Browser_compatibility) (which excludes Internet Explorer). Note however that it is common to use [bundling tools](@/javascript-building-and-bundling-tools.md) instead of directly using the modules in the browser to improve performance by reducing the total amount and size of files.
[Node.js also supports ES Modules](https://nodejs.org/api/esm.html) natively with some restrictions such as forcing file extensions to be specified when importing modules.

Use ES modules instead of the other types listed in this article whenever you can, as they are part of the official specification and the most future-proof.

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

## CommonJS

[CommonJS](https://en.wikipedia.org/wiki/CommonJS) was a module type made popular by Node.js in its early days. You may have already come across it in the form of `require()` or `module.exports`.

In the past, almost every NPM package used CommonJS, but in the last years, many of them have migrated to ES modules. CommonJS should be avoided for new projects and only used if you have to deal with legacy code.

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

## AMD

[AMD](https://requireJS.org/docs/whyamd.html#amd) (not to be confused with the hardware company) stands for "Asynchronous Module Definition" and was an early approach for modules in the JavaScript ecosystem. A noteworthy feature was the ability to dynamically load modules at runtime. As the bundling format used by [RequireJS](https://requireJS.org/), it gained some popularity in the past.

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

## Conclusion

In summary, use ES modules whenever you can.

[Continue reading the second part of this series which explores bundling tools.](@/javascript-building-and-bundling-tools.md)
