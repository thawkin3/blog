# Using Insomnia to Upgrade Dependencies — with Confidence

Always keep your dependencies up to date. When you don’t upgrade, you miss out on bug fixes, security patches, and new features. You may even be up against an “end of life” deadline if the version of a package you use will soon no longer be supported.

If upgrading dependencies is so important, why don’t many developers do it? They may not know how, or they may not understand the benefits of upgrading, or they may not feel like they have the time. Or, they may be afraid.

Why would developers be afraid to upgrade their dependencies? Because they think they might break something. And why are they afraid of breaking something? Because they don’t have good tests in place.

**When you have a good test suite running against your codebase, you can upgrade your dependencies with confidence.**

In this article, we’ll discuss semantic versioning, gotchas when upgrading dependencies, and how to upgrade dependencies with confidence. We’ll also use a small app to demonstrate how a good test suite can help you catch breaking changes from dependency upgrades before you deploy your app.

---

## Semantic Versioning

Let’s briefly talk about semantic versioning and how it works. JavaScript packages typically follow semantic versioning, which is a set of three numbers representing the major, minor, and patch versions of the package. So if a package is set at version 2.4.1, then that’s major version 2, minor version 4, and patch version 1.

Patch versions typically include bug fixes and security patches. Minor versions can include new features. But neither patch versions nor minor versions are supposed to break or change the existing API of the package. Major versions can come with breaking changes, usually through removing an API method or significantly reworking the underlying architecture of the code.

---

## Gotchas When Upgrading Dependencies

If package developers follow semantic versioning properly, it’s generally safe for consumers of those packages to upgrade minor and patch versions in their app, since by definition breaking changes are not allowed in those releases. However, some package maintainers may not follow this standard very well or may accidentally release breaking changes without realizing it, so you never know for sure. But generally speaking, upgrades to patch and minor versions of a dependency should go smoothly.

It’s the major versions that you need to be more careful with. When upgrading a package from one major version to the next, it’s always a good idea to consult the change log or release notes to see what’s changed.

Sometimes, the breaking changes in a major release don’t impact you, like if you aren’t using an API method that’s now been removed. Other times the changes will be relevant, and you’ll need to follow a migration guide to see what changes you need to make in order to use the new major version correctly. For massive breaking changes, sometimes developers will be kind enough to provide you with a codemod, a script that performs most or all of the changes for you.

The good news is that upgrading dependencies, even major versions, doesn’t need to be a scary experience.

---

## Upgrading Dependencies with Confidence

A test suite with high code coverage will benefit you greatly as you upgrade your dependencies. If your code is well covered by tests, then the tests should give you confidence that your app will still work properly after upgrading. If all the tests pass, you should feel confident that the upgrades went off without a hitch. If any tests fail, you know which areas of your app to focus on.

If you don’t have tests for your app, start writing them now! A good set of tests goes a long way — not just when upgrading dependencies, but also when refactoring existing code, writing new features, and fixing bugs.

Even with a good test suite, a small amount of manual testing after upgrading dependencies is also a good idea, just as an added safety measure. After all, there may be gaps in your test coverage or edge cases you hadn’t considered.

If you do find gaps in your test suite during manual testing, you should write a quick test for what you find and then go fix the issue. That way you now have an automated test to ensure that the particular bug you found doesn’t happen again in the future.

---

## Demo Time

Let’s now consider a small demo app that will help these abstract ideas become more concrete. Here we have a mind-blowingly useful app, [Is Today My Birthday](https://github.com/thawkin3/is-today-my-birthday). This app is the best, easiest, and fastest way to determine if today is your birthday. Simply input your birth date and today’s date, and the app will tell you if today is in fact your birthday.

![Demo app in action: “Is Today My Birthday”](https://cdn-images-1.medium.com/max/2000/0*zEbYCrqZrhlBApmk)
<figcaption>Demo app in action: “Is Today My Birthday”</figcaption>

Okay, I kid. But, we needed a simple app for demo purposes, so here we are.

This app is built with a Node.js and Express backend and a simple HTML, CSS, and vanilla JavaScript frontend. I used the [date-fns](https://date-fns.org/) package for working with dates, and I wrote API tests using [Insomnia](https://docs.insomnia.rest/). I’m able to run the API tests from the command line using the [Inso CLI](https://docs.insomnia.rest/inso-cli/introduction), and I’ve even integrated them into a continuous integration pipeline with [GitHub Actions](https://resources.github.com/devops/tools/automation/actions/). Pretty fancy, I know. You can [view all of the code for this app on GitHub](https://github.com/thawkin3/is-today-my-birthday).

The relevant part of the code that determines if today is your birthday is reproduced below:

{% gist https://gist.github.com/thawkin3/d131f03fc54b887f6a784bc0441e8b2a %}

The output from the three tests we’ve written looks like this:

![All three Insomnia tests are passing](https://cdn-images-1.medium.com/max/2000/0*WV6J_N0Cg3v85QCe)
<figcaption>All three Insomnia tests are passing</figcaption>

So let’s consider for a moment what we might do when upgrading the version of `date-fns` that our app uses. I’ve purposefully used v1.30.1 to begin with so that we can upgrade to v2.28.0 later. Going from v1 to v2 is a major release with breaking changes, and we’ll want to be sure that our app still works properly after we do our upgrades. If our app breaks after the upgrades, how will people ever be able to know if today is their birthday?

We’ll begin by changing the version of `date-fns` in our `package.json` file from v1.30.1 to v2.28.0. Then, we’ll run `yarn install` to install that new version.

After that, we can run our tests to see how things look:

![Two tests are failing after upgrading the date-fns package](https://cdn-images-1.medium.com/max/2820/0*Q4SNznoXUq1dTc9W)
<figcaption>Two tests are failing after upgrading the date-fns package</figcaption>

Oh no — we have some failures! Two of our three tests have failed, and it looks like we have a bad JSON response coming from our API. While it’s no fun to deal with failing tests, our tests have proved useful in detecting an issue when upgrading `date-fns` from v1 to v2.

If we investigate further, we’ll find the following error from `date-fns`:

```
“RangeError: Use `dd` instead of `DD` (in `MM-DD`) for formatting days of the month.”
```

Looking back at our code, we have indeed used `MM-DD` as our date format. Consulting the [change log for the 2.0.0 release of date-fns](https://date-fns.org/v2.28.0/docs/Change-Log#2.0.0-2019-08-20), we can see that one of the breaking changes is that the use of uppercase DD has been replaced with lowercase dd when formatting months and days together. Thanks for the helpful tip, change log!

We can now make that simple change in our code so it looks like this:

{% gist https://gist.github.com/thawkin3/8f31846b4d9e61de32b933af257c542c %}

We’ll then run our test suite again, and voila — all three tests are passing again. Order has been restored, and we’ve successfully upgraded one of the dependencies in our app.

---

## Conclusion

Upgrading dependencies is important. Staying up to date means you have the latest bug fixes, security patches, and features. By frequently updating your dependencies at regular intervals (perhaps once per month or once per quarter), you can avoid the panic of needing to upgrade end-of-life packages at the last minute.

Remember that tests help you upgrade with confidence. So what are you waiting for? Go write some tests and upgrade your app’s dependencies now!
