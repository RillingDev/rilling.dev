---
title: "Getting into JavaScript Building & Bundling. Part 1: Modules"
icon: file-code
date: 2016/07/19
updated: 2020/07/13
tags:
    - JavaScript
    - Modules
    - Workflow
---

The last few weeks I did a some research on the different ways to bundle JavaScript dependencies, here is what I found out:

## So Many Ways

Most of the time when working on a project with more than a few lines of JavaScript you'll want to split up the code into multiple files to make the structure easier to read and understand, or to make testing easier - but how do you manage the actual importing of your code parts or 3rd party libraries?

<!-- more -->

### Concating

The oldest way is to just mash every file into one, huge file that contains all dependencies and code parts in a working order. While this is easy to do, there are several issues which can occur with this technique.

Pro:

-   Requires minimal build tool functionality.

Con:

-   Keeping track in which order files should be concatinated can become extremely hard.
-   Requires (either manual or automatic) wrapping of file contents in [IIFEs](https://developer.mozilla.org/en-US/docs/Glossary/IIFE).

### AMD

AMD (not to be confused with the hardware company) stands for ["Asynchronous Module Definition"](http://requireJS.org/docs/whyamd.html#amd) and was the first popular approach to modules. As the main bundling format used by [RequireJS](http://requireJS.org/), it gained quite some popularity in the past, however is declining rapidly since Node.js and CommonJS became popular.

Example:

```js
// Main file
require(["foo"], function (foo) {
    foo.bar();
});

// Module file
define(["foo"], function () {
    return {
        bar: function () {
            return "fooBar";
        },
    };
});
```

Pro:

-   Supported by many legacy tools.

Con:

-   Declining popularity.
-   Quite verbose / requires more setup code.

### CommonJS

CommonJS is th next module definition system, made popular by [Node.js](https://nodejs.org/en/). Did you ever use Node.js `require()`? - yup that is CommonJS. Being used in hundreds of thousands of Node.js packages, it is safe to say that CommonJS is the module definition with the widest adoption right now.

Example:

```js
// Main file
const foo = require("foo");
foo.bar();

// Module file
module.exports = {
    bar: function () {
        return "fooBar";
    },
};
```

Pro:

-   Huge community.
-   A large amount of build tools support it.

### ES Modules

ES Modules are the youngest in the family of module definitions. While being introduced with the rest of ES6, [not all browsers support it yet](https://developer.mozilla.org/en/docs/web/JavaScript/reference/statements/import#Browser_compatibility). But! This doesn't stop us from using it, since we have module bundling tools, like [webpack](https://webpack.js.org/) or [rollup](https://rollupjs.org/guide/en/).

Example:

```js
// Main file
import { foo } from "foo";
foo.bar();

// Module file
export const foo = {
    bar: function () {
        return "fooBar";
    },
};
```

Pro:

-   Official ECMAScript standard.
-   No build tools or boilerplate code required when the browser / runtime supports it.
-   Large amount of build tools support it

Con:

-   Currently not directly supported by all browsers.

### Conclusion

I personally really enjoy using the ES syntax, but CommonJS is still a valid choice. I would avoid using AMD or concating unless you are working on a legacy project that already uses them.

[Continue reading about JavaScript Building & Bundling in the second Part of this series.](https://rilling.dev/getting-into-javascript-building-and-bundling-part-2-bundling-tools)
