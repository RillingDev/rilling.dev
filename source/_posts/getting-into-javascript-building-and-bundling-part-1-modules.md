---
title: "Getting into JavaScript Building & Bundling. Part 1: Modules"
icon: file-code
date: 2016/07/19
updated: 2019/05/12
tags:
    - JavaScript
    - Modules
    - Workflow
---

The last few weeks I did a lot of research on the different ways to manage JavaScript dependencies (and bundling them), here is what I found out:

## So many ways

Most of the time when working on a project with a lot of JavaScript you'll want to split up the code into multiple files to make the structure easier to read and understand, or making testing easier - but how do you manage the actual including of your code parts or 3rd party libraries?

<!-- more -->

### Concating

The oldest and the probably still most popular way is to just mash every file into one, huge file that contains all dependencies and code parts in a reasonable order. While this is easy to do, there are several points which can cause issues if you use this technique.

Pro:

-   Requires minimal build tool functionality.

Con:

-   Keeping track in which order files should be concatinated can become extremely hard.
-   More often than not, concatinated files pollute the global scope because of internal variables leaking due to missing [IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)s.

### AMD

AMD (not to be confused with the hardware company) stands for ["Asynchronous Module Definition"](http://requireJS.org/docs/whyamd.html#amd) and was the first popular approach to modules. As the main definition used by [RequireJS](http://requireJS.org/), it gained quite some popularity in the past, however is declining since Node and CommonJS became popular.

Example:

```JavaScript
// AMD
//Main file
require(["foo"], function (foo) {
    foo.bar();
});

//Module file
define(["foo"], function () {
    return {
        bar: function () {
            return "fooBar";
        }
    };
});
```

Pro:

-   Supported by many legacy tools.

Con:

-   Declining popularity.
-   Relatively verbose / requires more setup code.

### CommonJS

CommonJS is another module definition syntax, made popular by [Node.js](https://nodejs.org/en/). Did you ever use Node.js `require()`? - yup that's CommonJS. Being used in hundreds of thousands of Node.js packages, it is safe to say that CommonJS is the module definition with the widest adoption.

Example:

```JavaScript
// CommonJS
//Main file
const foo = require("foo");
foo.bar();

//Module file
module.exports = {
    bar: function () {
        return "fooBar";
    }
};
```

Pro:

-   Huge community.
-   Large amount of build tools support it.

### ES6 Modules

ES6 Modules are the youngest in the family of module definitions. While being introduced with the rest of ES6, [not all browsers support it](https://developer.mozilla.org/en/docs/web/JavaScript/reference/statements/import#Browser_compatibility). But! this doesn't stop us from using it, since we have module bundling tools. Hopefully the support will get better in the near future, which would further improved the JavaScript economy.

Example:

```JavaScript
// ES6 Modules
//Main file
import {foo} from "foo";
foo.bar();

//Module file
export const foo = {
    bar: function() {
        return "fooBar";
    }
};
```

Pro:

-   Official ECMAScript standard.
-   No build tools or boilerplate code required when the browser / runtime supports it.
-   Huge community.
-   Large amount of build tools support it

Con:

-   Currently not support by all browsers.

### Conclusion

All of the named ways of defining modules have their place, and while I personally really enjoy using the ES6 syntax, CommonJS is still a valid choice. I would avoid using AMD or concating unless you are working on a legacy project that already uses them.

[Continue reading about JavaScript Building & Bundling in the second Part of this series.](https://rilling.dev/getting-into-javascript-building-and-bundling-part-2-bundling-tools)
