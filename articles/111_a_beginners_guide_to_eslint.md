# A Beginner’s Guide to ESLint

![ESLint logo](https://cdn-images-1.medium.com/max/4656/1*ZOwFeIjPVvnJMrIkF3IXWw.png)*ESLint logo*

Most frontend developers use [ESLint](https://eslint.org/) for linting their JavaScript. ESLint is a static analysis checker that analyzes your source code and looks for errors and violations of best practices. It helps you quickly find problems, even before running your app locally.

ESLint is configured with an `.eslintrc.json` file. You can add individual rules, and you can also use pre-built plugins and configs.

One popular config is the `eslint:recommended` config. As its name suggests, this config contains a set of recommended rules from the maintainers of ESLint. You might choose to use the ESLint config published by another company, like the config from Airbnb: `eslint-config-airbnb`. If you use Prettier for your code formatting, you would also find yourself using the ESLint config from Prettier (`eslint-config-prettier`) that tells ESLint to ignore formatting rules and leave those to Prettier.

---

## Running ESLint

ESLint can be run from the command line using a script that you add to the `package.json` file. Most repos are configured with scripts like `npm run eslint` and `npm run eslint:fix`. For example:

```json
"scripts": {
  "eslint": "eslint ./src",
  "eslint:fix": "eslint ./src --fix"
}
```

It’s useful to have a script you can run from the command line because then you can run that same script in your CI pipeline to validate that you don’t have any errors prior to merging new code into your main branch.

ESLint can also be used through a plugin in your IDE, like with the [VS Code ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint).

---

## Making Changes to ESLint Rules

The actual rules, configs, and plugins that you use with ESLint really don’t matter (sort of). Some rules do enforce best practices, but others simply enforce conventions that you may or may not like. What’s important is that as a team you agree on a set of rules for your code standards and follow them.

For example, the Airbnb config is a good preset, but they may have rules that they care about at Airbnb that you don’t care about at your company. In a difference of opinion, it’s completely fine to say that you don’t care about a certain rule. In the event of that decision, you can modify an ESLint rule by changing it in the `.eslintrc.json` file.

---

## When First Running ESLint

You may find yourself adding ESLint to a repo that wasn’t previously configured with ESLint. Or you may find yourself working in a repo that did have ESLint configured, but no one was keeping the repo clean so there were hundreds or thousands of existing ESLint violations.

When cleaning up your ESLint violations for the first time, consider running `npm run eslint:fix` to automatically fix many of the errors. Just be sure to review your code changes after running this to make sure ESLint didn’t cause any funky formatting errors as a result of the auto-fixing.

---

## Handling ESLint Errors

When you encounter an error from ESLint, you have a few options:

1. **Fix the error.** ESLint is usually right, so if it’s complaining about something in your code, it’s very likely that you have a problem or have violated a best practice. In most cases, you should listen to ESLint and go fix the problem. If you don’t understand the error message that ESLint is providing, do a Google search for that rule name. Every rule has a markdown doc that explains the rule and gives examples of correct and incorrect usage.

2. **Ignore the error with an `eslint-disable` comment.** Sometimes ESLint is mistaken. It’s a static analysis tool, and static analysis isn’t perfect. If you’re certain that ESLint is wrong in a specific instance (and you’ve verified this with another developer), you can disable ESLint for that line in the file. See the ESLint docs for [how to disable ESLint](https://eslint.org/docs/latest/user-guide/configuring/rules#disabling-rules).

3. **Disable the rule globally.** If you decide at your company or within your team that you don’t care about a specific rule, you can disable it in the `.eslintrc.json` file. Keep in mind though that you shouldn’t disable ESLint rules just because you can’t figure them out or because it’s a lot of work to fix them. Make sure you have a valid reason for disabling the rule that you can defend.

---

## Wrapping Up

By now you should know what ESLint is, how to configure it, how to run it, how to use it to keep your code clean, and how to handle the errors that ESLint reports.

For more information on installing and using ESLint, I’d highly recommend reading [ESLint’s Getting Started guide](https://eslint.org/docs/latest/use/getting-started) as well as their other documentation.

Thanks for reading, and happy coding!
