---
title: 'Getting into JavaScript Building & Bundling. Part 1: Modules'
icon: file-code
date: 2016/07/19
tags:
    - JavaScript
    - Modules
    - Workflow
---

The last few weeks I did a lot of research on the different ways to manage JavaScript dependencies (and bundling them), here is what I found out:

## So many ways

Most of the time when writing a project with a lot of JavaScript you'll want to split up the code into multiple files to make the structure easier to read & understand, or to have easier access to testing tools - but how do you realise including your code parts or 3rd party libraries?

<!-- more -->

### Concating

The oldest and the probably still most popular way is to just mash every file into one, huge file that contains all dependencies and code parts in a reasonable order. While this is super easy to do, there are a lot of points which will cause issues if you use this technique.

Pro:

- Easy & quick to set up
- No new JavaScript needs to be included for it to work
- The old code doesn't need to be changed
- Good for small projects, where a module system would be overkill

Con:

- Keeping track in which order files should be concated can become extremely hard
- Code structure has to be kept flat, subfunctions or extending objects gets harder to realise
- More often than not, concated files pollute the global scope because of internal variables leaking

### AMD

AMD (not to be confused with the hardware company) stands for ["Asynchronous Module Definition"](http://requireJS.org/docs/whyamd.html#amd) and is the first popular approach to modules. As the main definition used by [RequireJS](http://requireJS.org/), it gained quite some popularity in the past, however, is declining since Node and CommonJS became popular.

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

- Big community
- Asynchronous

Con:

- Declining popularity
- Ugly syntax (IMO ;))

### CommonJS

CommonJS is another module definition syntax, made popular by [NodeJS](https://NodeJS.org/en/). Did you ever use Nodes "require()"? - yup that's CommonJS. Being used in hundreds of thousands of Node packages, it is save to say that CommonJS is the module definition with the widest adoption. Contrary to AMD, CommonJS is Synchronous.

Example:

```JavaScript
// CommonJS
//Main file
var foo = require("foo.JavaScript");
foo.bar();

//Module file
module.exports = {
    bar: function () {
        return "fooBar";
    }
};
```

Pro:

- Huge community
- Great documentation
- Large amount of build tools support it

Con:

- You have to distinguish between browser-compatible and node-only packages(those who require node to work, like 'fs')

### ES6 Modules

ES6 Modules are the youngest in the family of module definitions. While being introduced with the rest of ES6(aka ES 2015), [most browsers still don't support it](https://developer.mozilla.org/en/docs/web/JavaScript/reference/statements/import#Browser_compatibility). But! this doesn't stop us from using it, since we have module bundling tools. Hopefully the support will get better in the near future, which would further improved the JavaScript economy.

Example:

```JavaScript
// ES6 Modules
//Main file
import {foo} from "foo.JavaScript";
foo.bar();

//Module file
export const foo = {
    bar: function() {
        return "fooBar";
    }
};
```

Pro:

- Official syntax
- Might replace bundling tools altogether in the future

Con:

- Currently bad support

### Conclusion

All of the named ways of defining modules have their place, and while I personally really enjoy using the ES6 syntax, all the others can be fitting, depending on project size and side factors.

[Continue reading about JavaScript Building & Bundling in the second Part of this series.](https://f-rilling.com/getting-into-javascript-building-and-bundling-part-2-bundling-tools)
