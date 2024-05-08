# Going with the Flow for CI/CD: Heroku Flow with Gitflow

In the previous article in this series, we looked at how we could automate deploys to Heroku [using Heroku Flow for our CI/CD](https://dev.to/thawkin3/how-i-finally-got-all-my-cicd-in-one-place-getting-my-cicd-act-together-with-heroku-flow-4fo2). That setup had two Heroku apps for staging and production, but only used a single `main` branch. The staging app was automatically deployed when you committed to the `main` branch, and the production app was manually deployed by clicking the “Promote to production” button in the Heroku pipeline.

That strategy assumes that you’re doing “[trunk-based development](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development)”, in which you commit to a single `main` branch in your code repo.

However, some engineering teams still follow a branching strategy called [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), which involves using a `dev` branch and a `main` branch. Feature branches are merged into the `dev` branch to deploy code to a staging environment, and on a regular cadence the `dev` branch is merged into the `main` branch to release the code to the production environment.

In this article, we’ll consider how we can configure [Heroku Flow](https://www.heroku.com/flow) to meet our CI/CD needs when following the Gitflow branching strategy. We’ll use our Heroku Pipeline to deploy our app to our staging environment when we push to the `dev` branch and deploy our app to our production environment when we push to the `main` branch.

Ready to go with the flow?

---

## Getting Started

If you’d like to follow along throughout this tutorial, you’ll need a Heroku account and a GitHub account. You can [create a Heroku account here](https://www.heroku.com/), and you can [create a GitHub account here](https://github.com).

The demo app shown in this article is deployed to Heroku, and [the code is hosted on GitHub](https://github.com/thawkin3/heroku-flow-gitflow-demo).

---

## Running Our App Locally

You can run the app locally by forking the repo in GitHub, installing dependencies, and running the start command. In your terminal, do the following after forking the repo:

```sh
$ cd heroku-flow-gitflow-demo
$ npm install
$ npm start
```

After starting the app, visit [http://localhost:5001/](http://localhost:5001/) in your browser, and you’ll see the app running locally:

![Demo app](https://cdn-images-1.medium.com/max/3200/0*6IVIjjWEFBYAGAz3)

<figcaption>Demo app</figcaption>

---

## Creating Our Heroku Pipeline

Now that we have the app running locally, let’s get it deployed to Heroku so that it can be accessed anywhere, not just on your machine.

We’re going to create a Heroku pipeline that includes two apps: a staging app and a production app.

To create a new Heroku pipeline, navigate to your Heroku dashboard, click the “New” button in the top-right corner of the screen, and then choose “Create new pipeline” from the menu.

![Create new pipeline](https://cdn-images-1.medium.com/max/2000/0*VZEQrxmci8F__AQ5)

<figcaption>Create new pipeline</figcaption>

In the dialog that appears, give your pipeline a name, choose an owner (yourself), and connect your GitHub repo. If this is your first time connecting your GitHub account to Heroku, a second popup will appear in which you can confirm giving Heroku access to GitHub.

After connecting to GitHub, click “Create pipeline” to finish the process.

![Configure your pipeline](https://cdn-images-1.medium.com/max/2732/0*lKoAjnTqhjRLzmUa)

<figcaption>Configure your pipeline</figcaption>

With that, you’ve created a Heroku pipeline. Nice work!

![Newly created pipeline](https://cdn-images-1.medium.com/max/3200/0*AUvZCnIMpcptIHfW)

<figcaption>Newly created pipeline</figcaption>

---

## Creating Our Staging and Production Apps

As we mentioned, we’ll use two environments for our app: a staging environment and a production environment. The staging environment is where code is deployed for acceptance testing and any additional QA. The production environment is where code is released to actual users.

Let’s add a staging app and a production app to our pipeline. Both of these apps will be based on the same GitHub repo.

To add a staging app, click the “Add app” button in the Staging section. Next, click “Create new app” to open a side panel.

![Create a new staging app](https://cdn-images-1.medium.com/max/2000/0*9vUGKfgeSEO16WB0)

<figcaption>Create a new staging app</figcaption>

In the side panel, give your app a name, choose an owner (yourself), and choose a region (I left mine in the United States). Then click “Create app” to confirm your changes.

![Configure your staging app](https://cdn-images-1.medium.com/max/2000/0*KHIFyQinuqRPkuBk)

<figcaption>Configure your staging app</figcaption>

Congrats, you’ve just created a staging app!

![Newly created staging app](https://cdn-images-1.medium.com/max/2000/0*EVCywmw1x_FTBEwC)

<figcaption>Newly created staging app</figcaption>

Now let’s do the same thing, but this time for our production app. When you’re done configuring your production app, you should see both apps in your pipeline:

![Heroku pipeline with a staging app and a production app](https://cdn-images-1.medium.com/max/3200/0*o2oscCcecKUSvVJp)

<figcaption>Heroku pipeline with a staging app and a production app</figcaption>

---

## Configuring Automatic Deploys

We want our app to be deployed to our staging environment any time we commit to our repo’s `dev` branch. To do this, click the dropdown button for the staging app and choose “Configure automatic deploys” from the menu.

![Configure automatic deploys for the staging app](https://cdn-images-1.medium.com/max/2000/0*9wl1St1dqcLVfUMf)

<figcaption>Configure automatic deploys for the staging app</figcaption>

In the dialog that appears, make sure the `dev` branch is targeted, and check the box to “Wait for CI to pass before deploy.” In a later step, we’ll configure Heroku CI so that we can run tests in a CI pipeline. We don’t want to deploy our app to our staging environment unless CI is passing.

![Deploy the dev branch to the staging app after CI passes](https://cdn-images-1.medium.com/max/2520/0*dtLBrxAQBkkJBRb8)

<figcaption>Deploy the dev branch to the staging app after CI passes</figcaption>

Now let’s do the same thing for the production app, but this time choosing to deploy our production app whenever we commit code to the `main` branch.

![Configure automatic deploys for the production app](https://cdn-images-1.medium.com/max/2000/0*T_gAaU04H2_bX6wZ)

<figcaption>Configure automatic deploys for the production app</figcaption>

![Deploy the main branch to the production app after CI passes](https://cdn-images-1.medium.com/max/2512/0*hbvD-JaQOA-mCwUF)

<figcaption>Deploy the main branch to the production app after CI passes</figcaption>

---

## Enabling Heroku CI

If we’re going to require CI to pass, we had better have something configured for CI! Navigate to the “Tests” tab and then click the “Enable Heroku CI” button.

![Enable Heroku CI](https://cdn-images-1.medium.com/max/3200/0*LH-Rf0dI9GWUYGxt)

<figcaption>Enable Heroku CI</figcaption>

Our demo app is built with Node and runs unit tests with Jest. The tests are run through the `npm test` script. Heroku CI allows you to configure more complicated CI setups using an `app.json` file, but in our case because the test setup is fairly basic, Heroku CI can figure out which command to run without any additional configuration on our part. Pretty neat!

---

## Enabling Review Apps

For the last part of our pipeline setup, let’s enable review apps. Review apps are temporary apps which get deployed for every pull request (PR) created in GitHub. They’re incredibly helpful when you want your code reviewer to manually review your changes. With a review app in place, the reviewer can simply open the review app rather than having to pull down the code onto their machine to run the app locally.

To enable review apps, click the “Enable Review Apps” button on the pipeline page.

![Enable Review Apps](https://cdn-images-1.medium.com/max/2000/0*XpRjAi6n6unpBBmd)

<figcaption>Enable Review Apps</figcaption>

In the dialog that appears, check all three boxes.

- The first box enables review apps to be automatically created for each PR.
- The second box ensures that the review app isn’t created until CI passes.
- The third box sets a time limit on how long a stale review app should exist until it is destroyed. Review apps use Heroku resources just like your regular apps, so you don’t want these temporary apps sitting around unused and costing you or your company more money.

When you’re done with your configuration, click “Enable Review Apps” to finalize your changes.

![Configure your review apps](https://cdn-images-1.medium.com/max/2000/0*neCwrPLiBLAReuPk)

<figcaption>Configure your review apps</figcaption>

---

## Seeing It All in Action

Alright, you made it! Let’s review what we’ve done so far.

1. We created a Heroku pipeline.
2. We created a staging app and a production app for that pipeline.
3. We enabled automatic deploys from the `dev` branch to our staging app.
4. We enabled automatic deploys from the `main` branch to our production app.
5. We enabled Heroku CI to run tests for every PR.
6. We enabled the creation of Heroku review apps for every PR.

Now let’s see it all in action.

Create a feature branch off of your `dev` branch with any code change you’d like, and then use that to create a PR in GitHub to merge that feature branch into dev. I made a very minor UI change, updating the heading text from “Heroku Flow Gitflow Demo” to “Heroku Flow Gitflow Branching”.

Right after the PR is created, you’ll note that a new “check” gets created in GitHub for the Heroku CI pipeline.

![GitHub PR check for the Heroku CI pipeline](https://cdn-images-1.medium.com/max/3200/0*B1b9fs-3HIxdld00)

<figcaption>GitHub PR check for the Heroku CI pipeline</figcaption>

You can view the test output back in Heroku on your Tests tab:

![CI pipeline test output](https://cdn-images-1.medium.com/max/3200/0*qWpvWstcICmf3ueS)

<figcaption>CI pipeline test output</figcaption>

After the CI pipeline passes, you’ll note that another piece of info gets appended to your PR in GitHub. The review app is deployed, and GitHub shows a link to the review app. Click the “View deployment” button, and you’ll see a temporary Heroku app with your code changes.

![View deployment to see the review app](https://cdn-images-1.medium.com/max/3200/0*eXul6jRGfWq4bz5U)

<figcaption>View deployment to see the review app</figcaption>

You can also find a link to the review app in your Heroku pipeline:

![Review app found in the Heroku pipeline](https://cdn-images-1.medium.com/max/2000/0*ppVp4FBG9Bp0hWcW)

<figcaption>Review app found in the Heroku pipeline</figcaption>

Let’s assume that you’ve gotten a code review and that everything looks good. It’s time to merge your PR.

After you’ve merged your PR, look back at the Heroku pipeline. You’ll see that Heroku automatically deployed the staging app since new code was committed to the `dev` branch.

![Staging app was automatically deployed](https://cdn-images-1.medium.com/max/2000/0*wl2COvidYnk3Puo8)

<figcaption>Staging app was automatically deployed</figcaption>

At this point in the software development lifecycle, there might be some final QA or acceptance testing of the app in the staging environment. Let’s assume that everything still looks good and that you’re ready to release this change to your users in production.

You can create another PR to merge the `dev` branch into the `main` branch, or you can do this from your terminal by checking out the `main` branch and then running git merge `dev` and git push.

Once you’ve committed these changes to the `main` branch, look at your Heroku pipeline again, and you’ll see that the production app was automatically deployed.

![Production app was automatically deployed](https://cdn-images-1.medium.com/max/2000/0*9QJ1fVla7y1DeURg)

<figcaption>Production app was automatically deployed</figcaption>

And with that, your changes are now in production for all your users to see. Way to go!

![Updated demo app with new changes in production](https://cdn-images-1.medium.com/max/3200/0*ypu8IDVAXxpvUEWL)

<figcaption>Updated demo app with new changes in production</figcaption>

---

## Conclusion

If you’ve followed along this far, give yourself a pat on the back. Walking through these steps, we’ve configured everything we need for an enterprise-ready CI/CD solution for a team using the Gitflow branching strategy and multiple environments.

It doesn’t matter if you prefer trunk-based development or Gitflow as your branching strategy — Heroku Flow can support either.

Now not only can you host your apps on Heroku, but you can set up all of your CI/CD infrastructure on Heroku’s platform as well.
