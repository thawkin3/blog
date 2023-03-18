# JavaScript Dependency Management with npm and Yarn

Keeping your JavaScript dependencies up to date is important. In companies that have dozens (or hundreds!) of repos, this is no small feat. Oftentimes, dependency upgrades become neglected, which leads to you using dependencies that are years out of date.

When you don’t keep your dependencies up to date, you miss out on the latest features, bug fixes, and security patches in these packages. Even worse, you may find yourself in a tight spot when your dependency versions reach their end of life stage and you need to upgrade in a short timeframe.

Whether you use npm or yarn to manage your JavaScript dependencies, there are tools you can use to simplify the upgrade process.

In this article we’ll explore how we can use [yarn upgrade-interactive](https://classic.yarnpkg.com/en/docs/cli/upgrade-interactive) for yarn and [npm-check-updates](https://www.npmjs.com/package/npm-check-updates) for npm to keep our dependencies up to date.

---

## Upgrading Dependencies with Yarn

Yarn has a built-in command for upgrading dependencies in an interactive command line interface: `yarn upgrade-interactive`.

To find the latest versions of each dependency you use, regardless of the constraints specified in your `package.json` file, you can use the `--latest` flag, so `yarn upgrade-interactive --latest`.

After running this command, you’ll be shown a list of dependencies that have newer versions available. You can select as many dependencies as you’d like using the `Space` key and then hit `Enter` to start the upgrade. My general strategy is to do all the minor and patch versions together, since those *in theory* should not contain any breaking changes. Then I do my major version upgrades separately, one by one, since those by definition will have breaking changes in them that I may need to handle in my code.

Here’s what that process looks like:

![yarn upgrade-interactive --latest](https://cdn-images-1.medium.com/max/2000/1*93A22eSatK6LdYz0LUxDmg.gif)
*yarn upgrade-interactive --latest*

---

## Upgrading Dependencies with npm

Unfortunately, npm doesn’t have a built-in command for easily upgrading multiple dependencies in an interactive manner. However, there are third-party npm packages you can install globally that offer this functionality. `npm-check-updates` is one of those packages.

You can install `npm-check-updates` globally by running `npm install -g npm-check-updates`. You can see your list of globally-installed dependencies by running `npm list -g --depth=0`.

Once `npm-check-updates` is installed, you can use it by running `ncu -i`. This will open up an interactive command line prompt similar to what you’d see with `yarn upgrade-interactive --latest`. The user experience isn’t quite as good as Yarn’s tool, but it’s pretty close.

With `ncu -i`, after you select your dependencies, you can have it run `npm install` for you, or you can choose to have it just modify the `package.json` file without installing, meaning you would need to run `npm install` yourself.

Here’s what that process looks like:

![ncu -i](https://cdn-images-1.medium.com/max/2000/1*WLJBFflBWqzfa_0_c4j8zg.gif)*ncu -i*

---

## Going Further

Both `yarn upgrade-interactive --latest` and `ncu -i` make it easier for you to upgrade your dependencies, but it’s still a manual process. You’ll want to upgrade your dependencies on a somewhat regular cadence, maybe every two weeks, month, or quarter.

If this becomes burdensome to maintain, you may want to look into using automated tools that will do most of the heavy lifting for you. [Renovate](https://docs.renovatebot.com/) (created by Mend) and [Dependabot](https://github.com/dependabot) (created by GitHub) are both good choices for this, but we won’t cover either of these tools here. That’s a good topic for another article!

By now though, you should understand the importance of keeping your dependencies up to date and be equipped with a couple tools for npm and Yarn that will help you in your efforts.

Thanks for reading, and happy coding!
