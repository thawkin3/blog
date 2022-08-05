# Tools and Integrations to Significantly Improve Code Review in GitHub

![CodeSee Review Maps example on a pull request](https://cdn-images-1.medium.com/max/3200/0*MTiO-R31BScczOg5)*CodeSee Review Maps example on a pull request*

As software engineers, we love to automate. Yet code review is often a fairly manual process that takes up a large percentage of our time. How do we improve this? There are plenty of [tips and tricks](https://betterprogramming.pub/7-ways-to-dramatically-reduce-your-time-in-code-review-febe05e9f38c) for [doing code reviews effectively](https://google.github.io/eng-practices/review/), so we won’t go into those here. Instead, let’s focus on the parts of code review that we can automate.

In this article, we’ll consider five tools and integrations that can significantly improve your code review experience in GitHub.

---

## Travis CI or CircleCI

First, let’s talk about continuous integration (CI). Setting up CI for your repos is step one in automating your code review process. With a CI pipeline in place, you can run anything you want! At the very least, you should have your CI pipeline include jobs for formatting, linting, and unit tests. You might also consider creating additional jobs for building artifacts, deploying review apps, or running end-to-end tests.

Continuous integration helps you keep your master branch in a good state at all times. It also speeds up your code review process by handling all the tedious things that computers are good at and that humans aren’t. No more bickering over code styles or best practices during code reviews! With a CI pipeline in place, you can run automated checks for every new pull request you create.

![Travis CI example](https://cdn-images-1.medium.com/max/3200/0*ujESnycSU7a7clzD)*Travis CI example*

[Travis CI](https://www.travis-ci.com/) and [CircleCI](https://circleci.com/) are both excellent options for continuous integration. Both allow you to configure your CI pipeline using YAML files, enabling you to write your infrastructure as code. In the YAML file you can specify things like which Docker image to use, what version of Node to run, and what installation and build steps to follow.

Best of all, Travis CI and CircleCI are free to use for open-source projects.

---

## Jest and Codecov

Next, let’s take a look at test coverage. Unit tests are important. They help prevent regressions, serve as living documentation for your codebase, and enable you to make changes with confidence.

Tests are easily enforceable in a CI pipeline. If any of the tests fail, then the test job fails and the pull request is blocked from being merged until the tests are fixed.

It’s important not just that your tests pass, but also that your code coverage is high. Passing tests that only cover 20% of the codebase won’t give you a reasonable assurance that the other 80% of the codebase is in a good state after making any given change. Higher code coverage means fewer places for bugs to hide.

Test frameworks like [Jest](https://jestjs.io/) allow you to collect code coverage results and also enforce certain code coverage thresholds. If you want to take those coverage reports one step further, you can add a tool like [Codecov](https://about.codecov.io/) to your CI pipeline.

Codecov takes the data that Jest generates and turns it into reports that you can view inline in your pull requests. Codecov can give you insights into how each pull request affects the overall coverage, if there was an increase or decrease in coverage, and which files were affected. Codecov is easy to integrate into just about any CI tool you might use, whether that be Travis CI, CircleCI, GitHub Actions, GitLab CI, Jenkins, or something else.

And, CodeCov is free to use for open-source projects.

![Codecov report example](https://cdn-images-1.medium.com/max/3200/0*FAVU1W67gdD7SyBs)*Codecov report example*

---

## SonarQube and SonarCloud

Third, let’s discuss static analysis checkers. Linters like ESLint are a must for any project. Linters are great for enforcing best practices and agreed-upon coding styles.

A static analysis tool like [SonarQube](https://www.sonarsource.com/products/sonarqube/) can provide extra insights in addition to what a normal linter can find. SonarQube can help identify code smells, code complexity, code duplication, and security issues.

![SonarQube example](https://cdn-images-1.medium.com/max/2182/0*fXJfN_smXmMtPsZ3)*SonarQube example*

SonarQube can be run as a self-managed service, or you can run it in the cloud as [SonarCloud](https://www.sonarsource.com/products/sonarcloud/). You can also integrate SonarQube into your CI pipeline so that it posts a comment on your pull requests.

Sonar offers solutions for both enterprise companies and open-source projects. Like the other tools we’ve discussed so far, SonarQube and SonarCloud are free to use for open-source projects.

---

## CodeSee Review Maps

Fourth, let’s explore ways to visualize our codebase and the changes we make in our pull requests. Wouldn’t it be nice to know how each changed method or file affects the rest of the functionality in the app?

[CodeSee Review Maps](https://www.codesee.io/code-reviews) help you visualize what files were changed and how those changes affect upstream and downstream dependencies. By integrating with your GitHub repo, CodeSee can automatically post a comment and a diagram on every pull request.

![CodeSee Review Maps example on a pull request](https://cdn-images-1.medium.com/max/3200/0*MTiO-R31BScczOg5)*CodeSee Review Maps example on a pull request*

CodeSee makes it easier for you to walk through the changed files in a logical order rather than just viewing them alphabetically. It even lets you — as the code author — [create interactive tours](https://app.codesee.io/maps/review/github/Codesee-io/oss-port/pr/244?_ga=2.26313149.1847448165.1658437521-1515474781.1650061133) of your code to walk reviewers through your changes.

And — you guessed it — CodeSee Review Maps are free to use for open-source projects.

---

## GitHub Actions

Fifth, this wouldn’t be a proper article about automating parts of code review in GitHub if we didn’t mention [GitHub Actions](https://github.com/features/actions).

GitHub Actions allow you to create any kind of workflow you can imagine. These workflows can be run as jobs or checks within GitHub, so they function similar to other CI tools like Travis CI or CircleCI.

![GitHub Actions example for running unit tests as part of a CI pipeline](https://cdn-images-1.medium.com/max/3200/0*4DYrCSSIJC3uHZyp)*GitHub Actions example for running unit tests as part of a CI pipeline*

The [GitHub Actions Marketplace](https://github.com/marketplace?type=actions&query=sort%3Apopularity-desc+) is a great place to find open-source actions to use in your project. You can filter the results by category such as “code quality,” “code review,” or “continuous integration.”

Just as a teaser, a couple interesting actions in those categories that caught my eye were the [semantic-pull-request](https://github.com/marketplace/actions/semantic-pull-request) and [automatic-rebase](https://github.com/marketplace/actions/automatic-rebase) actions.

The semantic-pull-request action “ensures your PR title matches the [Conventional Commits spec](https://www.conventionalcommits.org/),” which is incredibly useful if you squash and merge your pull requests and then rely on those commit messages for further automation during your release process.

The automatic-rebase action allows you to rebase your commits simply by commenting `/rebase` on your pull request. Rebasing made easy!

With GitHub Actions, we can automate all the things! And — spoiler alert — GitHub Actions are free to use for public repos.

---

## Conclusion: 5 Tools for Improving Code Reviews in GitHub

To recap, here are five tools for improving your code review experience in GitHub:

1. Travis CI or CircleCI (for continuous integration)
2. Jest and Codecov (for code coverage)
3. SonarQube and SonarCloud (for code smells and security issues)
4. CodeSee Review Maps (for code diagrams)
5. GitHub Actions (for everything else)

Thanks for reading, and happy coding!
