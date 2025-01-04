# Heroku for ChatOps: Start and Monitor Deployments from Slack

![Heroku ChatOps Slack integration](https://cdn-images-1.medium.com/max/2000/0*iHNAprO51g3Jr5A1)
_Heroku ChatOps Slack integration_

In our last two articles, we explored how to [configure CI/CD for Heroku using Heroku pipelines](https://dzone.com/articles/how-i-finally-got-all-my-cicd-in-one-place-getting). When viewing a pipeline within the Heroku dashboard, you can easily [start a deployment or promote your code from one environment to the next](https://dzone.com/articles/heroku-flow-with-gitflow) with the click of a button. From the dashboard, you can monitor the deployment and view its progress.

This all works really well, assuming that you have Heroku open in your browser. But, what if you wanted to do it all from Slack?

Software engineers use a lot of apps at work. Throughout the day, we are constantly bouncing between Zoom meetings, Jira tasks, Slack conversations, GitHub, email, our calendar, and our IDE. This context switching can be exhausting and also lead to a lot of visual clutter on our monitors.

Sometimes, it’s nice to just live in Slack, and that’s why many tools offer Slack integrations. With these Slack integrations, you can monitor various processes and even use shortcut commands to trigger actions.

[Heroku ChatOps](https://devcenter.heroku.com/articles/chatops), the Heroku Slack integration, allows you to start and monitor deployments directly from Slack. In this article, we’ll explore some of the Slack commands it offers.

---

## Getting Started

If you’d like to follow along throughout this tutorial, you’ll need a Heroku account and a GitHub account. You can [create a Heroku account here](https://www.heroku.com/), and you can [create a GitHub account here](https://github.com).

The demo app that we will use with our Heroku pipeline in this article is deployed to Heroku, and [the code is hosted on GitHub](https://github.com/thawkin3/heroku-flow-demo).

---

## Create Our Heroku Pipeline

We won’t go through the step-by-step process for creating a Heroku pipeline in this article. Refer to these articles for a walkthrough of creating a Heroku pipeline:

- [How to create a Heroku pipeline with a staging and production app and a single main branch](https://dev.to/thawkin3/how-i-finally-got-all-my-cicd-in-one-place-getting-my-cicd-act-together-with-heroku-flow-4fo2)
- [How to create a Heroku pipeline with a staging and production app with a dev branch and a main branch](https://dev.to/thawkin3/going-with-the-flow-for-cicd-heroku-flow-with-gitflow-28oj)

You can also read the [Heroku docs for Heroku pipelines](https://devcenter.heroku.com/articles/pipelines).

Configuring your Heroku pipeline includes the following steps:

1. Create a GitHub repo.
2. Create a Heroku pipeline.
3. Connect the GitHub repo to the Heroku pipeline.
4. Add a staging app to the pipeline.
5. Add a production app to the pipeline.

The other activities that you’ll see in those articles, such as configuring review apps, Heroku CI, or automated deployments are optional. In fact, for the purposes of this demo, I recommend _not_ configuring automated deployments, since we’ll be using some Slack commands to start the deployments.

When you’re done, you should have a Heroku pipeline that looks something like this:

![Example Heroku pipeline](https://cdn-images-1.medium.com/max/3200/0*MnHBCqIYIjHIirqU)
_Example Heroku pipeline_

---

## Connect to Slack

Now that you have your Heroku pipeline created, it’s time for the fun part: integrating with Slack. You can [install the Heroku ChatOps Slack app here](https://chatops.heroku.com/auth/slack_install).

Clicking that link will prompt you to grant the Heroku ChatOps app permission to access your Slack workspace:

![Grant Heroku ChatOps access to your Slack workspace](https://cdn-images-1.medium.com/max/2336/0*Vbf6azQJlDeP6cED)
_Grant Heroku ChatOps access to your Slack workspace_

After that, you can add the Heroku ChatOps app to any Slack channel in your workspace.

![Add the Heroku ChatOps app](https://cdn-images-1.medium.com/max/2712/0*azlU40eMqSsiiQSU)
_Add the Heroku ChatOps app_

After adding the app, type `/h login` and hit Enter. This will prompt you to connect your Heroku and GitHub accounts. You’ll see several Heroku OAuth and GitHub OAuth screens where you confirm connecting these accounts.

_(As a personal anecdote, I found that it took me several tries to connect my Heroku account and my GitHub account. It may be due to having several Slack workspaces to choose from, but I’m not sure.)_

After connecting your Heroku account and your GitHub account, you’re ready to start using Heroku in Slack.

![Connect your Heroku and GitHub accounts](https://cdn-images-1.medium.com/max/2000/0*Nd2dNlSyBi1jDz_X)
_Connect your Heroku and GitHub accounts_

---

## View All Pipelines

To view all deployable pipelines, you can type `/h pipelines`:

![View all pipelines](https://cdn-images-1.medium.com/max/2124/0*0nDjOeqgRjYkZ3ZZ)
_View all pipelines_

---

## View Pipeline Info

To see information about any given pipeline, type `/h info <PIPELINE_NAME>`. (Anything you see in angle brackets throughout this article should be replaced by an actual value. In this case, the value would be the name of a pipeline — for example, “heroku-flow-demo-pipeline”.)

![View pipeline info](https://cdn-images-1.medium.com/max/2084/0*zBHJ_H_YF1uGi_Ul)
_View pipeline info_

---

## View Past Releases

To view a history of past releases for any given pipeline, type `/h releases <PIPELINE_NAME>`.

![View past releases](https://cdn-images-1.medium.com/max/2000/0*GVnkgKVKQrWCor0s)
_View past releases_

This command defaults to showing you past releases for the production app, so if you want to see the past releases for the staging app, you can type `/h releases <PIPELINE_NAME> in <STAGE_NAME>`, where `<STAGE_NAME>` is “staging”.

![View past staging releases](https://cdn-images-1.medium.com/max/2000/0*DZYAjXrhMGc07b1A)
_View past staging releases_

---

## Deploy to Staging

Now that we know which pipelines are available, we can see information about any given pipeline along with when the code was last released for that pipeline. We’re ready to trigger a deployment.

Most engineering organizations have a Slack channel (or channels) where they monitor deployments. Imagine being able to start a deployment right from that channel and monitor it as it goes out! That’s exactly what we’ll do next.

To start a deployment to your staging environment, type `/h deploy <PIPELINE_NAME> to <STAGE_NAME>`, where `<STAGE_NAME>` is “staging”.

![Deploy to staging](https://cdn-images-1.medium.com/max/2000/0*iHNAprO51g3Jr5A1)
_Deploy to staging_

After running that command, an initial message is posted to communicate that the app is being deployed. Shortly after, you’ll also see several more messages, this time in a Slack thread on the original message:

![Slack messages sent when deploying to staging](https://cdn-images-1.medium.com/max/2176/0*VScPY4AOSEj3vNH0)
_Slack messages sent when deploying to staging_

If you want to verify what you’re seeing in Slack, you can always check the Heroku pipeline in your Heroku dashboard. You’ll see the same information: The staging app has been deployed!

![Staging app shown in the Heroku dashboard](https://cdn-images-1.medium.com/max/2000/0*caKIp9pXSbh4shxQ)
_Staging app shown in the Heroku dashboard_

---

## Promote to Production

Now, let’s promote our app to production. Without the Slack commands, we _could_ navigate to our Heroku pipeline, click the “Promote to production” button, and then confirm that action in the modal dialog that appears. However, we’d prefer to stay in Slack.

To promote the app to production from Slack, type `/h promote <PIPELINE_NAME>`.

![Promote to production](https://cdn-images-1.medium.com/max/2224/0*c_5ZWfI5cTDhCmov)
_Promote to production_

Just like with the staging deployment, an initial Slack message will be sent, followed by several other messages as the production deployment goes out:

![Slack messages sent when promoting to production](https://cdn-images-1.medium.com/max/2212/0*6dP4zxqb3r59zTCq)
_Slack messages sent when promoting to production_

And — voilà — the latest changes to the app are now in production!

---

## Conclusion

Now you can start and monitor Heroku app deployments all from Slack — no need to context switch or move between multiple apps.

For more use cases and advanced setups, you can also [check out the docs](https://devcenter.heroku.com/articles/chatops).

Happy deploying!
