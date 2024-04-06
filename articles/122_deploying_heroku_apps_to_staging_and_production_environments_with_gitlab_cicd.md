# Deploying Heroku Apps to Staging and Production Environments with GitLab CI/CD

In a [previous article](https://medium.com/gitconnected/deploying-to-heroku-with-gitlab-ci-cd-56d9a07046b1), we explored how to automate deployments to Heroku using GitLab CI/CD. That setup deployed the app to its production environment every time we pushed code to the `main` branch.

In this article, we’ll consider a slightly more nuanced approach: What if we have multiple environments? Most engineering organizations use at least three environments: a local development environment, a staging environment, and a production environment.

Additionally, some engineering teams follow a [Gitflow branching strategy](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), where they have a `dev` branch and a `main` branch. This strategy has since fallen out of favor and been replaced by [trunk-based development](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development), but it’s not uncommon to find organizations still following this practice.

Today, we will look at how to configure GitLab CI/CD to deploy our app to our staging environment when we push to the `dev` branch and deploy our app to our production environment when we push to the `main` branch.

---

## Getting Started

Before we begin, we’ll need two things: a Heroku account and a GitLab account.

Heroku is a great place to host and deploy your apps. As a platform as a service (PaaS), Heroku allows you to focus on building cool things while abstracting away much of the infrastructure complexity. You can [create a Heroku account here](https://signup.heroku.com/).

GitLab is a great place to store your code. Beyond just being a source code management tool, GitLab also offers native CI/CD capabilities so you can set up pipelines to test and deploy your code without requiring another third-party tool. You can [create a GitLab account here](https://gitlab.com/users/sign_up).

The demo app shown in this article uses both GitLab and Heroku. You can [find all the code in the GitLab repo here](https://gitlab.com/tylerhawkins1/heroku-gitflow-staging-production-gitlab-cicd-demo).

---

## Running Our App Locally

You can run the app locally by cloning the repo, installing dependencies, and running the start command. In your terminal, do the following:

```sh
$ git clone https://gitlab.com/tylerhawkins1/heroku-gitflow-staging-production-gitlab-cicd-demo.git
$ cd heroku-gitflow-staging-production-gitlab-cicd-demo
$ npm install
$ npm start
```

After starting the app, visit [http://localhost:5001/](http://localhost:5001/) in your browser, and you’ll see the app running locally:

![Demo Heroku app](https://cdn-images-1.medium.com/max/3200/0*L6oy8fhcqK3Mgkcv)_Demo Heroku app_

---

## Deploying Our App to Heroku

Now that the app is running locally, let’s deploy it to Heroku so that you can access it anywhere, not just on your machine.

Remember that we are going to deploy your app to both a staging environment and a production environment. That means that we’ll have two Heroku apps based on the same GitLab repo.

If you don’t already have the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) installed on your machine, you’ll need to install that before moving on.

After installing the Heroku CLI, run the following commands from your terminal to check out the `main` branch, create a new production Heroku app, deploy it to your production environment, and open it in your browser:

```sh
$ git checkout main
$ heroku create heroku-gitlab-ci-cd-production --remote heroku-production
$ git push heroku-production main
$ heroku open --remote heroku-production
```

With that, you should see the same app, but this time running on a Heroku URL instead of on localhost. Nice work — you’ve deployed your Heroku app to production!

But, we’re not done yet. We also need to configure and deploy your staging app. To do this, run this similar set of commands shown below:

```sh
$ git checkout dev
$ heroku create heroku-gitlab-ci-cd-staging --remote heroku-staging
$ git push heroku-staging main
$ heroku open --remote heroku-staging
```

Now you’ll have the same app deployed again, but this time to a different URL that will serve as your staging environment. Now we have both of your environments configured!

Note the differences and similarities between the commands for the production app and the staging app:

1. The production app uses the `main` branch, and the staging app uses the `dev` branch.
2. The production app is called `heroku-gitlab-ci-cd-production`, and the staging app is called `heroku-gitlab-ci-cd-staging`.
3. The production app’s git remote is called `heroku-production`, and the staging app’s git remote is called `heroku-staging`.
4. Both the production app and the staging app’s git remotes use a `main` branch, since [Heroku only deploys the app when code is pushed to its main branch](https://devcenter.heroku.com/articles/multiple-environments).

Keep in mind that _this_ `main` branch is different from _your_ set of `main` and `dev` branches. The `main` branch that you’re pushing to here is the `main` branch on your git remote — in this case, Heroku.

So when you’re on your local `main` branch and run `git push heroku-production main`, you’re pushing your `main` branch to the Heroku production app’s `main` branch. And when you’re on your local `dev` branch and run `git push heroku-staging main`, you’re pushing your `dev` branch to the Heroku staging app’s `main` branch.

---

## Making Changes to Our App

Now that we have our Heroku app up and running, what if we want to make some changes? Following a rough approximation of the Gitflow branching strategy, we could do the following:

1. Check out the `dev` branch
2. Make changes to the code
3. Add, commit, and push those changes to the `dev` branch
4. Deploy those changes to the staging Heroku app by running `git push heroku-staging main`
5. Check out the `main` branch
6. Merge the `dev` branch into the `main` branch
7. Deploy those changes to the production Heroku app by running `git push heroku-production main`

(If we were truly following Gitflow, we’d have created a feature branch to merge into `dev`, and a release branch to merge into `main`, but we’ve omitted those steps to keep things simple.)

Now, wouldn’t it be nice if we could automate the deployment to either of our environments instead of having to deploy them manually all the time?

This is where GitLab CI/CD comes into play.

---

## Continuous Integration / Continuous Deployment

Continuous integration (CI) is all about committing often and keeping the build in a good state at all times. Typically you would verify that the build is in a good state by running checks in a CI pipeline. These checks might include linters, unit tests, and/or end-to-end tests.

Continuous deployment (CD) is all about deploying frequently. If the checks in the CI pipeline pass, then the build gets deployed. If the checks in the CI pipeline fail, then the build does not get deployed.

With GitLab CI/CD, we can configure our CI pipeline to do exactly this — run our tests and then deploy our app to Heroku if the tests all pass. Most importantly for our setup, we can configure rules within our CI pipeline to specify where the app should be deployed.

---

## Configuring GitLab CI/CD

We can create a CI pipeline in GitLab programmatically using a `.gitlab-ci.yml` file. Our file looks like this:

```yaml
image: node:20.10.0

cache:
  paths:
    - node_modules/

before_script:
  - node -v
  - npm install

stages:
  - test
  - deploy

unit-test-job:
  stage: test
  script:
    - npm test

deploy-job:
  stage: deploy
  variables:
    HEROKU_APP_NAME: $HEROKU_APP_NAME_PRODUCTION
  rules:
    - if: $CI_COMMIT_REF_NAME == $CI_DEFAULT_BRANCH
      variables:
        HEROKU_APP_NAME: $HEROKU_APP_NAME_PRODUCTION
    - if: $CI_COMMIT_REF_NAME =~ /dev/
      variables:
        HEROKU_APP_NAME: $HEROKU_APP_NAME_STAGING
  script:
    - apt-get update -yq
    - apt-get install -y ruby-dev
    - gem install dpl
    - dpl --provider=heroku --app=$HEROKU_APP_NAME --api-key=$HEROKU_API_KEY
```

GitLab CI pipelines consist of stages and jobs. Each stage can contain one or more jobs. In our pipeline, we have two stages: `test` and `deploy`. In the `test` stage, we run our unit tests in the `unit-test-job`. In the `deploy` stage, we deploy our app to Heroku in the `deploy-job`.

We can only progress to the next stage if all the jobs in the previous stage pass. That means if the unit tests fail, then the app won’t be deployed, which is a good thing! We wouldn’t want to deploy our app if it was in a bad state.

You’ll note that the `deploy` stage has a `rules` section with some conditional logic in place. This is where we specify to which environment our app should be deployed. If we’re on the `main` branch, then we deploy our production app. If we’re on the `dev` branch, then we deploy our staging app.

You’ll also note that the `deploy` stage references several variables called `$HEROKU_APP_NAME_PRODUCTION`, `$HEROKU_APP_NAME_STAGING`, and `$HEROKU_API_KEY`. These are stored as [CI/CD variables within GitLab](https://docs.gitlab.com/ee/ci/variables/index.html).

If you’re setting this up in your own GitLab account, you’ll need to first find your API key in your Heroku account. Within your Heroku account settings, you should see a section for your API key. If you haven’t generated an API key yet, generate one now.

![Heroku API key](https://cdn-images-1.medium.com/max/3092/0*nJiIjusi0rtXuBrg)_Heroku API key_

Next, in your GitLab project, click on **Settings** > **CI/CD** > **Variables**. Expand that section and add three new variables:

1. The value for `$HEROKU_API_KEY` will be the API key from your Heroku account.
2. The value for `$HEROKU_APP_NAME_PRODUCTION` will be the name of your production Heroku app. My production app’s name is `heroku-gitlab-ci-cd-production`, but since Heroku app names are universally unique, yours will be something different.
3. The value for `$HEROKU_APP_NAME_STAGING` will be the name of your staging Heroku app. My staging app’s name is `heroku-gitlab-ci-cd-staging`. Again, since Heroku app names are universally unique, yours will be something different.

![GitLab CI/CD variables](https://cdn-images-1.medium.com/max/4644/1*ELavVzd2YVT79Nx0fScWxA.png)_GitLab CI/CD variables_

With that, your GitLab CI pipeline is ready to go! Let’s test it out.

---

## Deploying Our Heroku App to the Staging Environment

Let’s check out the `dev` branch and make a change to the code in our app. I made a simple change to the heading text and added several new lines to the text in the UI. You can make a similar change in your code.

Now add, commit, and push that change to the `dev` branch. This will start the GitLab CI pipeline. You can view the pipeline within GitLab and see the progress in real time.

![GitLab CI pipeline build for the dev branch](https://cdn-images-1.medium.com/max/3200/0*Snjy6oE4zGEe8YTo)_GitLab CI pipeline build for the dev branch_

If everything goes well, you should see that both the `test` and `deploy` stages have passed. Now, go check out your hosted Heroku app at the staging environment URL. The GitLab CI pipeline took care of deploying the app for you, so you’ll now see your changes live in the staging environment!

![Demo Heroku app with updated copy](https://cdn-images-1.medium.com/max/3200/0*yVYp0F6ZK0vwfX7O)_Demo Heroku app with updated copy_

With a staging environment set up, you now have a great place to manually test your code in a hosted environment. This is also a perfect spot for having QA testers or product managers verify changes before they go to production.

Now, check out your production app URL. You’ll note that it’s still showing the old UI without the most recent changes. This is because the GitLab CI pipeline only deployed the changes to the staging environment, not the production environment.

We’ve verified that the code looks good in our staging environment, so let’s promote it to our production environment.

---

## Deploying Our Heroku App to the Production Environment

Let’s check out the `main` branch and then merge the `dev` branch into your `main` branch. You could do this through a pull request or by running `git merge dev` and then `git push`.

This will start the GitLab CI pipeline once again, but this time it will be preparing to deploy your production app.

![GitLab CI pipeline build for the main branch](https://cdn-images-1.medium.com/max/3200/0*mkZgKDHv9mSX5tT5)_GitLab CI pipeline build for the main branch_

You can also view all the pipeline runs on the Pipelines page in GitLab to see the various builds for your `dev` and `main` branches:

![All GitLab CI pipeline builds](https://cdn-images-1.medium.com/max/3200/0*tpUqjVh-BmvcgBhv)_All GitLab CI pipeline builds_

Once the pipeline finishes, visit the URL for your production Heroku app. You should now see your changes also deployed in production. Great work!

---

## Conclusion

A good CI pipeline allows you to ship new features quickly and confidently without manually managing the deployment process. Having multiple environments configured for local development, staging, and production gives you more control over where and when code is released.

Heroku and GitLab CI/CD allow you to automate all of this to make your DevOps processes a breeze!
