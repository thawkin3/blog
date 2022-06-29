# Step-by-Step Guide to Updating Your npm Dependencies with Yarn

For any active code repository you maintain, it's essential to keep your dependencies up to date. By staying up to date, you have access to all the latest features and bug fixes in each third-party package you use. It's also much easier to update to one major version ahead, like from v2 to v3, than it is to update to several versions ahead, like from v2 to v7. Staying on top of your dependency updates helps you avoid the mess of dealing with several breaking changes at once.

I tend to update the dependencies in projects I own about every two weeks, which is once per sprint. This may seem like a lot of time spent updating dependencies, but the truth is, if you are diligent in staying on top of things, it doesn't take long at all. 15–30 minutes is all the time you need.

Here's the process I use. It should work nicely for you too.

---

## Step 1

Run `yarn upgrade-interactive --latest` in your terminal. This opens up an interactive CLI that allows you to pick and choose which packages you'd like to update at this time. Choose all the minor and patch version updates, and then hit Enter.

---

## Step 2

Run `yarn upgrade-interactive --latest` in your terminal again. This time, select any major version updates you'd like to tackle. By definition, a major version signifies a breaking change, such as the removal of a feature or API that your code may be using. This means you should visit the GitHub repo for the package, view the changelog or release notes, and then make updates to your code as needed. Sometimes you may get lucky and find that the breaking change didn't apply to any of the functionality you were using, so no additional work is required.

---

## Step 3

Run `yarn outdated` in your terminal to view all remaining outdated dependencies. Why? Because sometimes `yarn upgrade-interactive` can't handle updates properly and you have to do the updates manually on your own.

For instance, `yarn upgrade-interactive` doesn't work for upgrading dependencies that are not in the root-level `package.json` file inside a [Lerna monorepo](https://github.com/lerna/lerna/issues/2477).

This command also doesn't work properly if you use the `resolutions` field in your `package.json` file to use a specific version of any given package. [The command will fail silently](https://github.com/yarnpkg/yarn/issues/5413) and not update the package version or resolution version.

So, instead, you must manually change the version specified in the `package.json` file for any remaining dependencies you'd like to update and then run `yarn install` to install those new versions.

---

## Step 4

Now that you've updated all the dependencies that you wanted to, it's time to verify that everything in your codebase is still working properly. If you don't have formatters, linters, or tests in your repo, good luck to you! You'll be relying on a manual spot check of your app to check for regressions.

If you do have helpful tooling installed, now is the time to run your formatters, linters, and tests. I use [Prettier](https://prettier.io/) for formatting, [ESLint](https://eslint.org/) for linting, and [Jest](https://jestjs.io/) for unit tests. If any errors are found, go ahead and resolve those issues.

Note: While I've left this check in Step 4, you may find it useful to run these checks after Steps 1 and 2 as well. It's up to you.

---

## Step 5

Add, commit, and push your code. Get your merge request reviewed and merged, and you are good to go!

---

## Conclusion

That's it! I've been following this process for years now, and the results have been amazing. No more getting bogged down in old versions of dependencies that hinder your development work. By spending just a little time maintaining your repo, you can then spend the rest of your time building awesome new features.
