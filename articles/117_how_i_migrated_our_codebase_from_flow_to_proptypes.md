# How I Migrated Our Codebase from Flow to PropTypes

Recently I migrated a substantial codebase from using Flow types to using PropTypes. I’ve chronicled the process here for anyone looking to do the same thing.

---

## Motivation

First off, why did I do this? Well, there were several reasons.

The short answer is that the company had dozens of code repos, but only two of them were using Flow. Flow was implemented in these two repos around 2018 when Flow was more popular. Fast forward to 2023, and Flow has mostly fallen out of favor in the React community, with most companies moving to TypeScript.

The slightly longer answer is that the Flow implementation wasn’t really being maintained at this company. The Flow dev dependencies were years out of date and many versions behind. The generated flow-typed files for various third-party dependencies were also several major versions behind for important core packages. There had been Flow errors in the codebase for years, and no one was cleaning them up. Due to the Flow errors, the Flow check step of the build process had been disabled at some point so as not to keep the build from compiling and blocking the CI pipeline.

Essentially, we weren’t getting any of the benefits from actually using Flow, so there wasn’t much point in keeping it around.

So, we knew that we wanted to get off of Flow. From there, we either wanted to just use regular JavaScript along with PropTypes or else continue the migration onto TypeScript.

Many of our repos were already using PropTypes. Some newer repos were written in TypeScript, while all of the older repos were written in JavaScript.

We decided to hold off on migrating these two Flow repos to TypeScript until we came up with a unified plan for all of our repos and had more time to do this migration. Instead, we would start with removing Flow and adding PropTypes.

By choosing to move from Flow to regular JavaScript and PropTypes, I was able to complete this migration in less than a week’s worth of development time. (And, removing Flow was the easy part, with that just taking a few hours!)

Here’s how I did it.

---

## Removing Flow

Removing Flow was surprisingly easy. I found an awesome package called [flow-remove-types](https://flow.org/en/docs/tools/flow-remove-types/) that functioned as a codemod to remove all the Flow syntax from the code. After that, there was just a bit of manual cleanup for removing unneeded dependencies and npm scripts.

The step-by-step process was:

1. Install the `flow-remove-types` package as a dev dependency: `npm install --save-dev --legacy-peer-deps flow-remove-types`
2. Create this script in the `package.json` file to generate a new `src2` directory that contains the transformed source code without any of the Flow types: `"remove-flow": "flow-remove-types --pretty src/ -d src2/"`
3. Run `npm run remove-flow`
4. See that it creates a new `src2` directory with all the files but with the Flow types removed from them
5. Delete the `src` directory
6. Rename the `src2` directory to `src`
7. Run `git status` to see if anything got deleted that shouldn’t have been. In my case, the script deleted a bunch of files with the following extensions: `.snap`, `.scss`, `.md`, `.svg`, and `.json`
8. Restore the deleted files that shouldn’t have been deleted (for example, by running `git checkout -- *.md` to restore all `.md` files)
9. Delete the `flow-typed` directory that may have been generated and stored in source control in the past
10. Remove the `remove-flow` npm script in the `package.json` file that we added earlier in Step 1
11. Remove all other Flow-related npm scripts in the `package.json` file
12. Uninstall any Flow-related dependencies in your project: `npm uninstall --legacy-peer-deps flow-remove-types @babel/preset-flow eslint-plugin-flowtype flow-bin flow-copy-source flow-typed flow-watch`
13. Remove any Flow setup from your ESLint config file (like from the `.eslintrc` file), if you’re using ESLint
14. Remove any Flow setup from your `.prettierignore` file, if you’re using Prettier
15. Delete the `.flowconfig` file
16. Remove any leftover `$FlowFixMe` comments in your code
17. Remove any `//` comments found at the top of files that used to have `// @flow` comments (the codemod removes the `@flow` part of the comment, but not the full `// @flow` comment)
18. Do whatever manual or automated testing you’d like in order to verify all your npm scripts work fine (build, test, lint, format, etc.)

With that, Flow is officially removed!

---

## Adding PropTypes

If your goal is simply to remove Flow from your codebase, you can stop here. In my case, I still wanted a little bit of helpful typing surrounding the props for each of our React components, especially since one of the repos I was working in was the component library. So, I chose to use PropTypes.

My methodology here was to look at all the changes from the PR in which I removed Flow and then add the appropriate PropTypes for each component that previously had Flow types included.

This looked like a very tedious and manual process to do on my own, so I began looking for a codemod I could use. Ideally, I would have just started with something like a “Flow to PropTypes” codemod, but I couldn’t find anything suitable that would help with that.

So, to help expedite this process of adding PropTypes, I played around with the [AST Explorer](https://astexplorer.net/) and wrote a small codemod with [jscodeshift](https://github.com/facebook/jscodeshift) that could handle the most basic conversions from Flow types to PropTypes. It was a neat exercise!

My codemod looked like this:

```js
export const parser = "flow";

export default function transformer(file, api) {
  const j = api.jscodeshift;

  const root = j(file.source);

  root.find(j.StringTypeAnnotation).forEach((path) => {
    j(path).replaceWith("PropTypes.string");
  });

  root.find(j.NumberTypeAnnotation).forEach((path) => {
    j(path).replaceWith("PropTypes.number");
  });

  root.find(j.FunctionTypeAnnotation).forEach((path) => {
    j(path).replaceWith("PropTypes.func");
  });

  root.find(j.BooleanTypeAnnotation).forEach((path) => {
    j(path).replaceWith("PropTypes.bool");
  });

  root.find(j.AnyTypeAnnotation).forEach((path) => {
    j(path).replaceWith("PropTypes.any");
  });

  root.find(j.GenericTypeAnnotation).forEach((path) => {
    j(path).replaceWith("PropTypes.object");
  });
  
  // Remove the "+" in front of some types
  root.find(j.Variance).forEach((path) => {
    j(path).replaceWith("");
  });

  return root.toSource();
}
```

Using that in the AST Explorer, I could start with some Flow typed code that looked like this:

```js
type Props = {
  someNumberProp: number,
  someStringProp: string,
  someBooleanProp: boolean,
  someFunctionProp: (event: Object) => void,
  someObjectProp: {
    someStringProp: string,
    someNumberProp: number,
  },
  someOneOfTypeProp: string | number,
  someOptionalStringProp?: string,
  someArrayOfNumberProp: number[],
  someArrayOfStringProp: Array<string>,
};
```

And get a resulting output that looked like this:

```js
type Props = {
  someNumberProp: PropTypes.number,
  someStringProp: PropTypes.string,
  someBooleanProp: PropTypes.bool,
  someFunctionProp: PropTypes.func,
  someObjectProp: {
    someStringProp: PropTypes.string,
    someNumberProp: PropTypes.number,
  },
  someOneOfTypeProp: PropTypes.string | PropTypes.number,
  someOptionalStringProp?: PropTypes.string,
  someArrayOfNumberProp: PropTypes.number[],
  someArrayOfStringProp: PropTypes.object,
};
```

This codemod handles simple types like `number`, `string`, `bool`, and `func` nicely. You can see that the conversion isn’t perfect though:

* The object prop doesn’t get `PropTypes.shape` wrapped around it
* The optional prop still has the `?` included
* The “one of type” prop still has the `|` syntax instead of the valid PropType syntax of `PropTypes.oneOfType([PropTypes.string, PropTypes.number])`
* The array of numbers (`number[]`) isn’t converted to valid PropType syntax of `PropTypes.arrayOf(PropTypes.number)`
* The alternative array syntax (`Array<string>`) is converted to `PropTypes.object` instead of `PropTypes.arrayOf(PropTypes.string)`

I’m sure if I put more than 30 minutes into building my codemod that it could handle these cases! But, since I didn’t, I used the initial output and then fixed any issues by hand to get a correct end result for my PropTypes:

```js
MyComponent.propTypes = {
  someNumberProp: PropTypes.number,
  someStringProp: PropTypes.string,
  someBooleanProp: PropTypes.bool,
  someFunctionProp: PropTypes.func,
  someObjectProp: PropTypes.shape({
    someStringProp: PropTypes.string,
    someNumberProp: PropTypes.number,
  }),
  someOneOfTypeProp: PropTypes.oneOfType([PropTypes.string, PropTypes.number]),
  someOptionalStringProp: PropTypes.string,
  someArrayOfNumberProp: PropTypes.arrayOf(PropTypes.number),
  someArrayOfStringProp: PropTypes.arrayOf(PropTypes.string),
};
```

From there I double-checked all the required vs. optional props and corrected them by adding `.isRequired` to each prop as needed.

Correcting the PropTypes syntax was admittedly a very manual process that I’m sure could have been improved with a better codemod. Spending a little more time up front could have saved a few hours here!

Finally, I ran my app locally and opened the JavaScript console in the browser’s dev tools. As I navigated each page, I made corrections to the code usage or to the PropTypes definitions as needed.

---

## Differences Between Flow and PropTypes

Having gone through the conversion process from Flow to PropTypes, I noticed several important differences that are worth highlighting. Because Flow and PropTypes work differently, it’s not quite the same thing as moving from Flow to TypeScript.

Some key differences are:

* PropTypes will console log warnings at runtime (as opposed to compile time like Flow or TypeScript).
* PropTypes only shows the console logs in development mode but not in production mode.
* Because the warnings are in the browser’s JavaScript console, it requires you to pay attention to the console and fix your problems.
* PropTypes are specifically for React components and props, not functions or typing anything else.
* PropTypes don’t allow you to specify the function signature unless you want to write a complex validation function. You just use the func type.
* PropTypes feels a little more verbose and harder to read compared to Flow or TypeScript, especially for objects (`PropTypes.shape`) and objects with dynamic keys (`PropTypes.objectOf`).

---

## Conclusion

There you have it. That’s how I managed to migrate from Flow to PropTypes in a couple code repos each containing a few hundred thousand lines of code, all within less than a week’s worth of work.

My hope is that developers in a similar situation searching for a process will find this guide helpful.

Thanks for reading, and happy coding!
