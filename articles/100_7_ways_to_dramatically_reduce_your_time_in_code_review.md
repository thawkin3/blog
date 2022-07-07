# 7 Ways to Dramatically Reduce Your Time in Code Review

Code reviews can be painful. Software engineers often complain that the review process is slow, delays downstream tasks, and leads to context switching as you navigate back and forth between an open pull request (PR) and your next task. Code reviews can also be full of nitpicking and bikeshedding, making it a poor experience for everyone involved.

To remedy this problem, some engineers have even gone as far as suggesting we get rid of pull requests and code reviews altogether. While that may work for small teams at startups, I don’t think this is the right solution for everyone, especially enterprise-level companies.

Rather, there are plenty of ways we can make the code review process a better experience for both the code author and the code reviewer. Let’s consider seven of these best practices together.

---

## #1: Keep Pull Requests Small

Every engineer dreads reviewing pull requests that have 1000+ lines of code changed. These reviews can take hours to complete, and oftentimes what ends up happening is that the reviewer begins to skim over the code rather than carefully review it.

![10 lines of code equals 10 issues. 500 lines of code equals looks fine. Code reviews.](https://cdn-images-1.medium.com/max/2364/0*uAQv427CIPm5qTvW)
<figcaption>Source: https://twitter.com/iamdevloper/status/397664295875805184</figcaption>

The solution is to keep your pull requests small. Small PRs are easier and quicker to review because the reviewer doesn’t need to spend as much time building up a mental model of how all the changes work together. There is also less code changed, which hopefully equates to fewer errors, fewer comments, and fewer rounds of back and forth between the author and the reviewer.

Keeping your PRs small may seem difficult at first, but it can be done if you break down your work into small tasks and stay focused. Don’t do major refactors while also implementing new features or fixing bugs. Use feature flags in your code so you can merge small parts of new features into the master branch without it showing up in the production app.

Keep your PRs small. Your reviewers will be grateful.

---

## #2: Use Pull Request Templates

Another annoyance is being asked to review a pull request without any context. When a PR is dropped in your lap with no explanation, you’re often left wondering, “What is this PR for? What issue is this solving? Is there a related task for this? Why was this particular approach taken?”

A [pull request template](https://levelup.gitconnected.com/managing-complexity-through-merge-request-templates-9a00cc9a5fb1) is a small, configurable form you can set as the default text on every new pull request. The PR template prompts the code author to provide relevant details for their PR. Typically a PR template would ask for a brief description of what you’ve done and why, a link to the task ticket, and a test plan for validating the changes.

Good PR templates also usually include a short checklist for the code author to go through to ensure that they haven’t missed any of the basics. This checklist might include items like unit tests, docs, internationalization, cross-browser support, and accessibility.

Below is an example pull request template that I like to use for all of my repos:

![Example pull request template](https://cdn-images-1.medium.com/max/2800/0*3GwljUpMtsLcejC7)
<figcaption>Example pull request template</figcaption>

---

## #3: Implement Response Time SLAs

If you’re finding that pull requests sit unreviewed for longer than you’d like, now is a good time to set expectations as a team for how quickly a new pull request should be reviewed. In other words, what is the maximum amount of time a PR can exist before it must be picked up? One hour? Two hours? 24 hours? Your answer to that question will likely depend on the size of your team. You may also have a different answer for internal pull requests from your team versus external pull requests from other teams.

When choosing a response time SLA (service level agreement), you’ll want to find the right balance. It’s not reasonable to expect everyone to immediately drop whatever they’re doing and review your code when you post a new PR, but you also don’t want PRs to stay unreviewed for hours on end. Find the right balance that allows your teammates to get into flow state. They should be able to work on their own code and then review PRs at natural stopping points throughout the day.

Personally, I like having a two-hour response time SLA for internal team PRs and a 24-hour response time SLA for external team PRs.

Regardless of what you and your teammates decide, having a team agreement allows you to hold each other accountable. If everyone has agreed to a specific SLA and that time has elapsed for one of your PRs, you know it’s ok to start bugging people about it.

---

## #4: Train Junior and Mid-Level Engineers

Training opportunities are everywhere. Mentoring less experienced engineers includes more than just teaching them about the technologies and languages they’re working with. It also includes teaching them soft skills like how to do an effective code review.

Teach your teammates what you look for during a code review. Help them understand what’s important and what’s not. Teach them how to communicate effectively in their [code review comments](https://conventionalcomments.org), like by prefixing non-blocking suggestions with “nit.”

There are plenty of great resources on how to be a more effective code reviewer. Google’s [Code Review Developer Guide](https://google.github.io/eng-practices/review/) is worth reading in its entirety. The guide has excellent advice for both the code author and the code reviewer. For a more cheeky resource, [How to Make Your Code Reviewer Fall in Love with You](https://mtlynch.io/code-review-love/) is easily some of the best (and entertaining) advice for developers creating pull requests.

---

## #5: Set Up Continuous Integration Pipelines

Code reviews become tedious when most of the comments are things like “Missing semicolon” or “Indentation seems off here.” Don’t spend time during code reviews on things that code formatters and code linters can take care of for you. Let computers automate the trivial stuff so that you can focus on the important things that require a human.

For JavaScript projects, it’s simple to configure a formatter like [Prettier](https://prettier.io/) and a linter like [ESLint](https://eslint.org/) for your repo. You can then set up continuous integration for your repo using something like [Travis CI](https://travis-ci.org/), [CircleCI](https://circleci.com/), [GitHub Actions](https://github.com/features/actions), or [GitLab CI/CD](https://docs.gitlab.com/ee/ci/).

Your CI pipeline will run these formatting and linting tasks for you along with your unit tests. If the CI pipeline fails at any step on a pull request, it will block that pull request from being merged.

Now you’ve automated several important parts of the code review, saving you hours.

---

## #6: Use Pull Request Review Apps

Sometimes it’s necessary not just to review the code in a pull request but also to manually view the changes in the app to verify that things look good. For apps with complex setup steps, pulling down someone else’s code and running it locally on your machine may take anywhere from five minutes to an hour. What a headache!

[Pull request review apps](https://betterprogramming.pub/how-to-create-pr-review-apps-with-render-fe5e78a073ae) are used to automatically deploy your code to a short-lived test environment any time a new PR is created. This allows reviewers to easily inspect UI changes without having to pull down the code and run it locally on their machine. Not only does this save time, but it also nudges reviewers to be more thorough in their reviews by making it easy.

---

## #7: Generate Diagrams to Visualize Your Code Changes

When reviewing code in GitHub or GitLab, the files are typically shown in alphabetical order. For relatively small PRs, this may not be a problem. But when there are dozens of files involved in a PR, sometimes it’s helpful to see those changes grouped together logically so you can see how they fit together in a bigger picture.

[CodeSee Review Maps](https://www.codesee.io/code-reviews) help you visualize what files were changed and how those changes affect their upstream and downstream dependencies. They integrate with GitHub to automatically post a comment and a diagram on your PR. You can even create interactive tours of your code to help guide your code reviewers. Best of all, CodeSee Maps are free to open-source organizations and their public repositories.

![Example CodeSee Map](https://cdn-images-1.medium.com/max/3200/0*FqcgluxV-GOLiECW)
<figcaption>Example CodeSee Map</figcaption>

---

## Conclusion: Best Practices for Speeding Up Code Reviews

To recap, here are seven tips for dramatically reducing your time in code review:

1. Keep pull requests small.
2. Use pull request templates to provide all the context a reviewer would need.
3. Implement response time SLAs.
4. Train junior and mid-level engineers on the key things you look for during a code review.
5. Set up CI pipelines to run linters, formatters, and unit tests.
6. Use pull request review apps so you can easily inspect UI changes.
7. Generate diagrams to visualize your code changes using a tool like CodeSee Review Maps.

Thanks for reading, and happy coding!
