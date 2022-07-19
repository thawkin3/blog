# What Every Good README Should Contain

I work in a large engineering organization that has thousands of repos actively in use. The unique nature of my current role means that rather than working in just a few of these repos, I am often working in several new repos each week. That's a lot of new information to consume each week! It's not realistic for me to become intimately familiar with the ins and outs of every repo I work with, but there are certain core pieces of information that I almost always need to know. Ideally, this information should be contained in each repo's README.

So, what information should be included in a README? At the very least, every good README should contain the following:

1. What this repo is or does
2. How to run the project locally
3. How to run the tests
4. How to get merged changes into the larger app
5. Anything else that you think would be helpful to call out for your specific repo
6. Links to any additional resources

Let's take a brief look at each of these items and why they're important.

---

## What this repo is or does

The README should establish some context for the developer. What is this repo? What does it do? What purpose does this repo serve? What are the problems that it solves or the functionality that it provides? A couple paragraphs of high-level overview can help set the stage for everything else that the developer may want to know.

---

## How to run the project locally

Now that the developer knows what the repo is, how do they run it?

Do they need to install any dependencies or run a setup script first? For a frontend app, this is hopefully as simple as just running `yarn install` (or `npm install`).

Once the setup is done, how do they start the app locally? If the app can be run in an independent sandbox environment like Storybook, include instructions for that. This could be as simple as running `yarn start:storybook`.

What about running the app in the context of a larger app? For organizations that have many repos, it's common for each smaller repo to be published as an npm package, and then each package gets installed as a dependency in the larger parent app.

So how do you run this smaller app locally to see your new changes before publishing a new version? Instructions could include linking the dependency with something like `yarn link` or `yalc`. Or maybe you use a browser extension like Resource Override to override the bundled JS file in the browser. Or, maybe this app is a microfrontend, and so you simply need to start the app with a command like `yarn start` and then override the url for that resource used.

---

## How to run the tests

The developer knows what the app does and how to run it, but what about tests? Ideally, running the test suite is as easy as running `yarn test` or some variant of that.

Sometimes there is some hidden setup required before you can run your tests, so calling out that information is helpful. For example, maybe you run integration tests with Cypress, but an implied prerequisite for running the Cypress tests is that the app needs to be running locally first. If the test script doesn't start the app for you already, you should be sure to document that expected test setup.

---

## How to get merged changes into the larger app

The developer is able to run the app and the tests, and they successfully made some changes to the code. Now what? How do they get those changes merged into the larger app? You'll want to be sure to document your branching strategy and what the merge request process looks like.

After code is merged, what happens next? If this is an npm package, does a new version of the package get published automatically in a post-merge pipeline? Or does the developer need to manually publish a new version? If this is a microfrontend, do the changes get deployed automatically after merging? Or does the developer need to manually kick off a pipeline to do that?

---

## Anything else that you think would be helpful to call out for your specific repo

With those four previous items, we've covered the basics that all developers need to know. They have context for the project and are able to successfully run, test, and deploy the app.

Is there anything else that they should know? This is harder to write generic guidance for, but surely there is something special about your repo that may not be common knowledge. Do you use any unique branching strategies or have any special branches besides a `master` or `main` branch? Do you have any special linter or commit setup that people should be aware of? Are there any gotchas to know about in regards to pipelines or deployments? What about coupling with other repos? Is this repo used closely with another repo?

These kinds of hidden gems are incredibly useful to document so that it doesn't remain as tribal knowledge.

---

## Links to any additional resources

Finally, are there any other docs or wiki pages that a developer might be interested in reading? Perhaps you use something like Notion or Confluence, and you have additional information documented there. Be sure to include links to anything else that for whatever reason isn't included in the README but that may be important to know.

---

## Conclusion

The README is all about helping new developers work with your repo successfully. When a developer has all the context and information they need, they'll be more self reliant. They'll be less likely to come to you with questions, and you'll find yourself not having to repeat yourself as much. It's a win for everyone involved.
