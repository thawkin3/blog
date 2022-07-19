# Updating Dependencies While Keeping package.json and yarn.lock in Sync

The JavaScript ecosystem is filled with hundreds of thousands of npm packages for you to choose from. As you build out your project, it’s likely that you will soon depend on at least a handful of third-party dependencies.

Npm packages get updated by their maintainers all the time. These updates could be bug fixes, security patches, new features, and major re-writes.

---

## Semantic Versioning

To help consumers of these packages understand how each new release will affect their project, package maintainers generally use what’s known as [semantic versioning](https://semver.org/).

Semantic versioning looks like `MAJOR.MINOR.PATCH`. For example, a package version could be set at `1.0.0`. If a new release contains only bug fixes, then the version would be bumped to `1.0.1`. If a new release contains new features that don’t break existing APIs, then the version would be bumped to `1.1.0`. And if a new release contains breaking changes that consumers of the package will need to be aware of and adjust to in how they use the package, then the version would be bumped to `2.0.0`.

---

## Storing Your Project’s Dependencies

![Filing Cabinet](https://miro.medium.com/max/8512/0*iGvCY0dWjy1m10EK)
<figcaption>Photo by Maksym Kaharlytskyi on Unsplash</figcaption>

Your dependencies are specified in your `package.json` file. For each package listed in your `dependencies` or `devDependencies` object, you can specify exactly how you want the package to be updated.

Including only the version number means you only want to use that exact package version only.

Prefixing the version number with a tilde (`~`) means you only want to accept patch updates when they’re available.

Prefixing the version number with a caret (`^`) means you want to accept minor and patch updates when they’re available.

If you are using Yarn to manage your packages, exact versions of each dependency that is installed in your project are stored in your `yarn.lock` file. The `yarn.lock` file’s main purpose is to ensure that the versions of each package remain consistent for each person that has pulled down your code onto their machine.

---

## Updating Your Project’s Dependencies

![Arrow pointing forward](https://miro.medium.com/max/8064/0*ozczF7cDS_btxi5E)
<figcaption>Photo by Nick Fewings on Unsplash</figcaption>

As mentioned above, npm packages get updated very frequently. This means that if you want to keep your project using the latest releases, you have to continually update your dependencies.

I try to update my project’s dependencies about once per week so that I don’t fall too far behind. Even in that timeframe, it’s common that I’ll have 10 or 20 packages that have new versions available.

---

**Now, on to the crux of the issue**: When running `yarn upgrade` to upgrade your dependencies, the `yarn.lock` file gets updated with the latest requested package versions, but the `package.json` file does not!

![Frustrated person](https://miro.medium.com/max/10368/0*bZQgjPDK7ETuwGA9)
<figcaption>Photo by Icons8 Team on Unsplash</figcaption>

For example, if you have a package `"something-cool": "^2.0.3"` in your dependencies object in your `package.json file`, and there is an available version for `2.4.0`, and you run `yarn upgrade`, then version `2.4.0` will be installed for your project, and version `2.4.0` will be shown as what’s installed in your `yarn.lock` file. But, your `package.json` file will still show `"something-cool": "^2.0.3"`.

That’s because you’ve specified that you’re ok with installing the latest version of the package as long as it’s still part of the major version `2`. That requirement holds true, so `package.json` remains unchanged even though `yarn.lock` changes and the later version is installed.

---

For me, this is a little counterintuitive. When I update the package from `2.0.3` to `2.4.0`, I’d like the version to update in both `yarn.lock` *and* `package.json`.

After doing a quick Google search, it seems that I’m not alone. Many other developers also expect this behavior.

So, is it possible to get this kind of behavior to happen?

Yes!

---

## The Solution

![Happy person](https://miro.medium.com/max/10660/0*sz5klkuZYjFrSNTW)
<figcaption>Photo by bruce mars on Unsplash</figcaption>

The best solution I’ve found so far is to use the following command to update my packages: `yarn upgrade-interactive --latest`.

Let’s break that down.

We already know that `yarn upgrade` is used to upgrade package versions.

Running `yarn upgrade-interactive` instead puts you into a command line interface tool that allows you to pick and choose which packages you’d like to upgrade.

Passing the `--latest` flag is the key to getting the `package.json` file to also be updated.

---

Now, it’s important to note that the `--latest` flag updates your package version to the latest version, regardless of what rules you’ve specified for that package in your `package.json` file. That means that if you have `"something-cool": "^2.0.3"` specified, and a version `3.1.0` is available, running yarn upgrade `--latest` would in fact update that package to version `3.1.0`, despite the fact that you were only wanting to make minor and patch updates by default.

That’s why I use `yarn upgrade-interactive` instead of just `yarn upgrade` so that I can pick which packages I’d like to update. When choosing, I only choose those packages that have minor and patch updates available.

I update all those, and then run my linters and tests to make sure I haven’t broken anything.

If there are major versions available to upgrade, I generally handle those individually, one by one. That way it’s easy to know what package broke things if anything goes wrong.

---

## Conclusion

I hope this trick helps you as you maintain your JavaScript projects and their dependencies. Yarn has some documentation on their [upgrade command](https://classic.yarnpkg.com/en/docs/cli/upgrade/), and on their [upgrade-interactive command](https://classic.yarnpkg.com/en/docs/cli/upgrade-interactive), but I found their docs a bit confusing when trying to solve for this specific problem.

Now you too can easily update your packages in `package.json` and `yarn.lock` while keeping them in sync.

Happy coding!
