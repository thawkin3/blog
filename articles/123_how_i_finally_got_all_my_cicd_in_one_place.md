# How I Finally Got All My CI/CD in One Place: Getting my CI/CD act together with Heroku Flow

![Heroku Flow demo app](https://cdn-images-1.medium.com/max/2940/0*Y0rsYVX5EGgYdHmI)_Heroku Flow demo app_

The Heroku team has long been an advocate of CI/CD. Their platform integrates with many third-party solutions like GitLab CI/CD or GitHub Actions.

In [a previous article](https://dev.to/thawkin3/deploying-to-heroku-with-gitlab-cicd-54f6), I demonstrated how you can configure your Heroku app with GitLab CI/CD to automatically deploy your app to production. In [a follow-up article](https://dev.to/thawkin3/deploying-heroku-apps-to-staging-and-production-environments-with-gitlab-cicd-3m8h), I walked you through a slightly more nuanced setup involving both a staging environment and a production environment.

But if you want to go all in on Heroku, you can use a series of solutions called [Heroku Flow](https://www.heroku.com/flow) to **configure all your CI/CD** without any third parties. Heroku Flow brings together [Heroku pipelines](https://devcenter.heroku.com/articles/pipelines), [Heroku CI](https://devcenter.heroku.com/articles/heroku-ci), [Heroku review apps](https://devcenter.heroku.com/articles/github-integration-review-apps), a GitHub integration, and a release phase.

In this article, I’ll show you how to set this up for your own projects.

---

## Getting Started

Before we begin, if you’d like to follow along, you’ll need a Heroku account and a GitHub account. You can [create a Heroku account here](https://signup.heroku.com/login), and you can [create a GitHub account here](https://github.com/signup).

The demo app shown in this article is deployed to Heroku, and [the code is hosted on GitHub](https://github.com/thawkin3/heroku-flow-demo).

---

## Running Our App Locally

You can run the app locally by forking the repo in GitHub, installing dependencies, and running the start command. In your terminal, do the following after forking the repo:

```sh
$ cd heroku-flow-demo
$ npm install
$ npm start
```

After starting the app, visit [http://localhost:5001/](http://localhost:5001/) in your browser, and you’ll see the app running locally:

![Demo app](https://cdn-images-1.medium.com/max/2932/0*zl1I6iElA6xVzMrh)
_Demo app_

---

## Creating Our Heroku Pipeline

Now that we have the app running locally, let’s get it deployed to Heroku so that it can be accessed anywhere, not just on your machine.

We’ll create a Heroku pipeline that includes a staging app and a production app.

To create a new Heroku pipeline, navigate to your Heroku dashboard, click the “New” button in the top-right corner of the screen, and then choose “Create new pipeline” from the menu.

![Create new pipeline](https://cdn-images-1.medium.com/max/2000/0*YOFPZDMUSsV6xutQ)
_Create new pipeline_

In the dialog that appears, give your pipeline a name, choose an owner (yourself), and connect your GitHub repo. If this is your first time connecting your GitHub account to Heroku, a second popup will appear in which you can confirm giving Heroku access to GitHub.

After connecting to GitHub, click “Create pipeline” to finish the process.

![Configure your pipeline](https://cdn-images-1.medium.com/max/2720/0*VVYHOkYr4mMfjdq6)
_Configure your pipeline_

With that, you’ve created a Heroku pipeline. Well done!

![Newly created pipeline](https://cdn-images-1.medium.com/max/3200/0*FothLy9tMzZtfQ-V)
_Newly created pipeline_

---

## Creating Our Staging and Production Apps

Most engineering organizations use at least two environments: a staging environment and a production environment. The staging environment is where code is deployed for acceptance testing and any additional QA. Code in the staging environment is then promoted to the production environment to be released to actual users.

Let’s add a staging app and a production app to our pipeline. Both of these apps will be based on the same GitHub repo.

To add a staging app, click the “Add app” button in the “Staging” section. Next, click “Create new app” to open a side panel.

![Create a new staging app](https://cdn-images-1.medium.com/max/2000/0*iyZ7it4rAQMqbtE7)
_Create a new staging app_

In the side panel, give your app a name, choose an owner (yourself), and choose a region (I left mine in the United States). Then click “Create app” to confirm your changes.

![Configure your staging app](https://cdn-images-1.medium.com/max/2000/0*xMY1AEEdYVDXC_nu)
_Configure your staging app_

Congrats, you’ve just created a staging app!

![Newly created staging app](https://cdn-images-1.medium.com/max/2000/0*YQt9ejdgRZkqpDil)
_Newly created staging app_

Now let’s do the same thing, but this time for our production app. When you’re done configuring your production app, you should see both apps in your pipeline:

![Heroku pipeline with a staging app and a production app](https://cdn-images-1.medium.com/max/3200/0*E1FzwFlU2gB8uRLz)
_Heroku pipeline with a staging app and a production app_

---

## Configuring Automatic Deploys

We want our app to be deployed to our staging environment any time we commit to our repo’s `main` branch. To do this, click the dropdown button for the staging app and choose “Configure automatic deploys” from the menu.

![Configure automatic deploys](https://cdn-images-1.medium.com/max/2000/0*wq_Oj8w_kY-jdpt9)
_Configure automatic deploys_

In the dialog that appears, make sure the `main` branch is targeted, and check the box to “Wait for CI to pass before deploy.” In our next step, we’ll configure Heroku CI so that we can run tests in a CI pipeline. We don’t want to deploy our app to our staging environment unless CI is passing.

![Deploy the main branch to the staging app after CI passes](https://cdn-images-1.medium.com/max/2492/0*8OYbJFHP4U4XO6VB)
_Deploy the main branch to the staging app after CI passes_

---

## Enabling Heroku CI

If we’re going to require CI to pass, we better have something configured for CI! Navigate to the “Tests” tab and then click the “Enable Heroku CI” button.

![Enable Heroku CI](https://cdn-images-1.medium.com/max/3200/0*kzVNx1rSQ9BgcsyU)
_Enable Heroku CI_

Our demo app is built with Node and runs unit tests with Jest. The tests are run through the `npm test` script. Heroku CI allows you to configure more complicated CI setups using an `app.json` file, but in our case, because the test setup is fairly basic, Heroku CI can figure out which command to run without any additional configuration on our part. Pretty neat!

---

## Enabling Review Apps

For the last part of our pipeline setup, let’s enable review apps. Review apps are temporary apps that get deployed for every pull request (PR) created in GitHub. They’re incredibly helpful when you want your code reviewer to review your changes manually. With a review app in place, the reviewer can simply open the review app rather than having to pull down the code onto their machine and run the app locally.

To enable review apps, click the “Enable Review Apps” button on the pipeline page.

![Enable Review Apps](https://cdn-images-1.medium.com/max/2000/0*6aEepjfrisyRrnfR)
_Enable Review Apps_

In the dialog that appears, check all three boxes.

- The first box enables the automatic creation of review apps for each PR.
- The second box ensures that CI must pass before the review app can be created.
- The third box sets a time limit on how long a stale review app should exist until it is destroyed. Review apps use Heroku resources just like your regular apps, so you don’t want these temporary apps sitting around unused and costing you or your company more money.

When you’re done with your configuration, click “Enable Review Apps” to finalize your changes.

![Configure your review apps](https://cdn-images-1.medium.com/max/2000/0*C0UsSM_zAhmL7DzO)
_Configure your review apps_

---

## Seeing It All in Action

Alright, you made it! Let’s review what we’ve done so far.

1. We created a Heroku pipeline
2. We created a staging app and a production app for that pipeline
3. We enabled automatic deploys for our staging app
4. We enabled Heroku CI to run tests for every PR
5. We enabled Heroku review apps to be created for every PR

Now let’s see it all in action.

Create a PR in GitHub with any code change you’d like. I made a very minor UI change, updating the heading text from “Heroku Flow Demo” to “Heroku Flow Rules!”

Right after the PR is created, you’ll note that a new “check” gets created in GitHub for the Heroku CI pipeline.

![GitHub PR check for the Heroku CI pipeline](https://cdn-images-1.medium.com/max/3120/0*MWct-aDetekEtoGf)
_GitHub PR check for the Heroku CI pipeline_

You can view the test output back in Heroku on your “Tests” tab:

![CI pipeline test output](https://cdn-images-1.medium.com/max/3200/0*PVyktkWAeUnfhRKZ)
_CI pipeline test output_

After the CI pipeline passes, you’ll note another piece of info gets appended to your PR in GitHub. The review app gets deployed, and GitHub shows a link to the review app. Click the “View deployment” button, and you’ll see a temporary Heroku app with your code changes in it.

![View deployment to see the review app](https://cdn-images-1.medium.com/max/2904/0*iRA72OO4uNamhYOy)
_View deployment to see the review app_

You can also find a link to the review app in your Heroku pipeline:

![Review app found in the Heroku pipeline](https://cdn-images-1.medium.com/max/2000/0*V1eV0VKXYvDNI2Hv)
_Review app found in the Heroku pipeline_

Let’s assume that you’ve gotten a code review and that everything looks good. It’s time to merge your PR.

After you’ve merged your PR, look back at the Heroku pipeline. You’ll see that the staging app was automatically deployed since new code was committed to the `main` branch.

![Staging app was automatically deployed](https://cdn-images-1.medium.com/max/2000/0*4CXsCPzpq1qzifPt)
_Staging app was automatically deployed_

At this point in the software development lifecycle, there might be some final QA or acceptance testing of the app in the staging environment. Let’s assume that everything still looks good and that you’re ready to release this change to your users.

Click the “Promote to production” button on the staging app. This will open a dialog for you to confirm your action. Click “Promote” to confirm your changes.

![Promote to production](https://cdn-images-1.medium.com/max/2508/0*cb0SBRR3UFyB-Har)
_Promote to production_

After promoting the code, you’ll see the production app being deployed.

![Production app was deployed](https://cdn-images-1.medium.com/max/2000/0*9OnBktw3MIHBmykg)
_Production app was deployed_

And with that, your changes are now in production for all of your users to enjoy. Nice work!

![Updated demo app with new changes in production](https://cdn-images-1.medium.com/max/2940/0*Y0rsYVX5EGgYdHmI)
_Updated demo app with new changes in production_

---

## Conclusion

What a journey we’ve been through! In this short time together, we’ve configured everything we need for an enterprise-ready CI/CD solution.

If you’d like to use a different CI/CD tool like GitLab CI/CD, GitHub Actions — or whatever else you may prefer — Heroku supports that as well.

But if you don’t want to reach for a third-party CI/CD provider, now you can **do it all** within Heroku with Heroku Flow.
