# Understanding Dev, Peer, and Regular Dependencies

When building a modern JavaScript app, you will likely rely on many other packages and third-party dependencies. For example, you might use `react` and `react-dom` for your UI, `lodash` for utility helper methods, `webpack` or `rollup` as your bundler, and `jest` with `@testing-library/react` for unit tests.

All of these packages are stored as dependencies in your `package.json` file. Understanding whether to include them as `dependencies`, `devDependencies`, or `peerDependencies` can sometimes be a struggle for newer developers.

In this article we'll look at the three types of dependencies found in the `package.json` file and what each does.

---

## Dependencies

`dependencies` are packages used in your app's production bundle. If you're building a React app, then `react` and `react-dom` would be dependencies. If you're using `react-router` for client-side routing, that would also be part of your `dependencies`. Any other packages like `lodash` or a design system library like Material UI (`@mui/material`) would also be `dependencies`.

There is an exception to this rule, but we'll cover this down in the Peer Dependencies section below.

---

## Dev Dependencies

`devDependencies` are typically the tooling you use to build your project, but they're not actually included in the app's production bundle. For example, `webpack` and `rollup` would both be `devDependencies`. These are bundlers used to compile your app, but they're not part of your app.

The same goes for test frameworks and libraries like `jest`, `@testing-library/react`, `karma`, `mocha`, or `jasmine`. These libraries are needed for writing unit tests for your code, but they're not part of your production app.

Linters and code formatters like `eslint`, `prettier`, `commitlint`, `husky`, and `lint-staged` would all be `devDependencies` as well.

---

## Peer Dependencies

`peerDependencies` are dependencies that your app relies on but expects another package to provide. `peerDependencies` are a crucial tool in reducing the size of your final production app. Let's take a look at why.

Imagine that you have a large React app composed of many npm packages. Let's say there are 20 of them and they all use React. If each of these packages lists `react` and `react-dom` in their `dependencies` section, then that means you'll have 20 copies of React in your app! (21 if you count the copy that your main parent app provides as well.)

In order to avoid this massive bloat of dependencies, library developers can instead use the `peerDependencies` section to specify that their library depends on `react` and `react-dom`, but the consumer of the package needs to be the one to provide it. In doing so, you can then have your app with 20 dependencies but only have one copy of `react` and `react-dom` used in production provided in the `dependencies` in the `package.json` file of your parent app.

When using `peerDependencies`, it's important to note that if you mark a package as a peer dependency, you also must include it in your `devDependencies` section. This allows your app to install and use packages like `react` and `react-dom` during local development and unit testing, but it won't include them as real dependencies in the production build.

Finally, when using a peer/dev dependency combination, you also should add the package in the `externals` configuration section of your Webpack or Rollup config file.

---

## Conclusion

That's it! Now you know how to properly work with dev, peer, and regular dependencies in any JavaScript project. Doing so will help you reduce the final bundle size of your app and ensure that only the necessary packages are installed in production.

Thanks for reading!
