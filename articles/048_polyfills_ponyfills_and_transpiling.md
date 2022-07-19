# Polyfills, Ponyfills, and Transpiling

When discussing new JavaScript features and syntax, it’s common to hear words like **polyfill**, **transpile**, and even **ponyfill** used. For example, someone might say, “In order to use this in older browsers, you’ll need to use Babel to transpile the code.” Or maybe, “Make sure to provide a polyfill for this functionality so that older browsers can use it.”

If you’re not familiar with these terms, you may be asking yourself, “What’s the difference? Are these all just interchangeable words that mean the same thing?”

In this article, we’ll define these terms and clear the air of any surrounding confusion.

---

## Polyfill

A **polyfill** is used to implement an API or feature that the browser does not support. The polyfill code is implemented and stored in the same variable or method name that it would have been in had the browser supported the given feature.

For example, the `Promise` object for working with asynchronous JavaScript is [not supported in IE11](https://caniuse.com/promises). Trying console logging `window.Promise` in IE11, and you’ll see that the result is `undefined`. Now try console logging `window.Promise` in just about any other browser, and you’ll see that it contains a constructor function used to create new `Promise` instances.

In order for promises to work in IE11, you would need to provide a polyfill. The polyfill patches the global scope for your app by storing the needed functionality inside the `window.Promise` variable. After applying the polyfill, you could then console log `window.Promise` and get a function.

---

## Ponyfill

A **ponyfill** is also used to implement an API or feature that the browser does not support. *But*, unlike polyfills, ponyfills do not affect the global scope.

For example, if we wanted to use promises without polluting the global scope, we could use a package like [promise-polyfill](https://www.npmjs.com/package/promise-polyfill). This package offers both a polyfill and a ponyfill.

To use the polyfill, we’d simply import the necessary file. Note how we don’t store the import in a variable. It simply patches the `window` object to now include a `Promise` method.

```js
import 'promise-polyfill/src/polyfill';
```

But, if we wanted to use the ponyfill, we’d do this instead:

```js
import Promise from 'promise-polyfill';
```

Now instead of affecting the global scope, we’ve imported the `Promise` functionality and stored it in a variable. That means `window.Promise` is still `undefined`, but we can still create new promises in our file by writing `new Promise();`.

---

## Transpiling

A **transpiler** is used to transform code from one syntax to another. For example, you can use [Babel](https://babeljs.io/) to convert your code from ES6+ syntax to ES5 syntax so that older browsers like IE11 can understand it.

The key here is that transpilers are needed for *syntax* that the browser can’t understand. You can polyfill missing objects or methods, but you cannot polyfill syntax.

IE11, for example, [does not support arrow functions](https://caniuse.com/arrow-functions) and does not understand the `=>` syntax. You can’t polyfill syntax, and there’s no way to make IE11 understand what you mean when you write `const add = (a, b) => a + b`.

Instead, you must transpile the code to convert it into different older syntax that IE11 can understand: `function add(a, b) { return a + b; }`.

---

## Conclusion

There you have it. Now you, too, know the difference between polyfills, ponyfills, and transpiling.

Happy coding!
