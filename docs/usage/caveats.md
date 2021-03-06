---
layout: docs
title: Caveats
description: Just some things to keep in mind when using Babel.
permalink: /docs/usage/caveats/
redirect_from: /caveats.html
---

## Polyfills

In order for certain features to work they require certain polyfills. You can satisfy **all**
Babel feature requirements by using [babel-polyfill](/docs/usage/polyfill).

You may alternatively/selectively include what you need:

| Feature                     | Requirements                                                                          |
| --------------------------- | ------------------------------------------------------------------------------------- |
| Array destructuring         | `Symbol`                                                                              |
| Async functions, Generators | [regenerator runtime](https://github.com/facebook/regenerator/blob/master/runtime.js) |
| For Of                      | `Symbol`, `prototype[Symbol.iterator]`                                                |
| Spread                      | `Array.from`                                                                          |

There is also the `loose` option for some of these plugins. 

## Classes

Built-in classes such as `Date`, `Array`, `DOM` etc cannot be properly subclassed
due to limitations in ES5. (for the [es2015-classes](/docs/plugins/transform-es2015-classes) plugin)

## Internet Explorer

### Classes (10 and below)

If you're inheriting from a class then static properties are inherited from it
via [\_\_proto\_\_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto),
this is widely supported but you may run into problems with much older browsers.

**NOTE:** `__proto__` is not supported on IE <= 10 so static properties
**will not** be inherited. See the
[protoToAssign](/docs/plugins/transform-proto-to-assign) for a possible work
around.

For classes that have `super`s, the super class won't resolve correctly. You can
get around this by enabling the `loose` option in the [es2015-classes](/docs/plugins/transform-es2015-classes) plugin.

### Getters/setters (8 and below)

In IE8 `Object.defineProperty` can only be used on DOM objects. This is
unfortunate as it's required to set getters and setters. Due to this if
you plan on supporting IE8 or below then the usage of getters and setters
isn't recommended.

**Reference**: [MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty#Internet_Explorer_8_specific_notes).

#### Modules

By default, when using modules with Babel a non-enumerable `__esModule` property
is exported. This is done through the use of `Object.defineProperty` which is
unsupported in IE8 and below. A workaround for this is to enable the `loose` option in your corresponding module plugin.
