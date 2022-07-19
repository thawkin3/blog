# Developing and Deploying Micro-Frontends with single-spa

Micro-frontends are the future of frontend web development. Inspired by microservices, which allow you to break up your backend into smaller pieces, micro-frontends allow you to build, test, and deploy pieces of your frontend app independently of each other. Depending on the micro-frontend framework you choose, you can even have multiple micro-frontend apps -- written in React, Angular, Vue, or anything else -- coexisting peacefully together in the same larger app!

In this article, we're going to develop an app composed of micro-frontends using [single-spa](https://single-spa.js.org/) and deploy it to [Heroku](https://www.heroku.com/). We'll set up continuous integration using [Travis CI](https://travis-ci.org/). Each CI pipeline will bundle the JavaScript for a micro-frontend app and then upload the resulting build artifacts to [AWS S3](https://aws.amazon.com/s3/). Finally, we'll make an update to one of the micro-frontend apps and see how it can be deployed to production independently of the other micro-frontend apps.

---

## Overview of the Demo App

![Demo app - end result](https://dev-to-uploads.s3.amazonaws.com/i/g3ews2z98cbwu7d08ls7.png)
<figcaption>Demo app - end result</figcaption>

Before we discuss the step-by-step instructions, let's get a quick overview of what makes up the demo app. This app is composed of four sub-apps:

1. A [container app](https://github.com/thawkin3/single-spa-demo-root-config) that serves as the main page container and coordinates the mounting and unmounting of the micro-frontend apps
2. A [micro-frontend navbar app](https://github.com/thawkin3/single-spa-demo-nav) that's always present on the page
3. A [micro-frontend "page 1" app](https://github.com/thawkin3/single-spa-demo-page-1) that only shows when active
4. A [micro-frontend "page 2" app](https://github.com/thawkin3/single-spa-demo-page-2) that also only shows when active

These four apps all live in separate repos, available on GitHub, which I've linked to above.

The end result is fairly simple in terms of the user interface, but, to be clear, the user interface isn't the point here. If you're following along on your own machine, by the end of this article you too will have all the underlying infrastructure necessary to get started with your own micro-frontend app!

Alright, grab your scuba gear, because it's time to dive in!

---

## Creating the Container App

To generate the apps for this demo, we're going to use a command-line interface (CLI) tool called [create-single-spa](https://single-spa.js.org/docs/create-single-spa/). The version of create-single-spa at the time of writing is 1.10.0, and the version of single-spa installed via the CLI is 4.4.2.

We'll follow these steps to create the container app (also sometimes called the root config):

```sh
mkdir single-spa-demo

cd single-spa-demo

mkdir single-spa-demo-root-config

cd single-spa-demo-root-config

npx create-single-spa
```

We'll then follow the CLI prompts:

1. Select "single spa root config"
2. Select "yarn" or "npm" (I chose "yarn")
3. Enter an organization name (I used "thawkin3," but it can be whatever you want)

Great! Now, if you check out the `single-spa-demo-root-config` directory, you should see a skeleton root config app. We'll customize this in a bit, but first let's also use the CLI tool to create our other three micro-frontend apps.

---

## Creating the Micro-Frontend Apps

To generate our first micro-frontend app, the navbar, we'll follow these steps:

```sh
cd ..

mkdir single-spa-demo-nav

cd single-spa-demo-nav

npx create-single-spa
```

We'll then follow the CLI prompts:

1. Select "single-spa application / parcel"
2. Select "react"
3. Select "yarn" or "npm" (I chose "yarn")
4. Enter an organization name, the same one you used when creating the root config app ("thawkin3" in my case)
5. Enter a project name (I used "single-spa-demo-nav")

Now that we've created the navbar app, we can follow these same steps to create our two-page apps. But, we'll replace each place we see "single-spa-demo-nav" with "single-spa-demo-page-1" the first time through and then with "single-spa-demo-page-2" the second time through.

At this point we've generated all four apps that we need: one container app and three micro-frontend apps. Now it's time to hook our apps together.

---

## Registering the Micro-Frontend Apps with the Container App

As stated before, one of the container app's primary responsibilities is to coordinate when each app is "active" or not. In other words, it handles when each app should be shown or hidden. To help the container app understand when each app should be shown, we provide it with what are called "activity functions." Each app has an activity function that simply returns a boolean, true or false, for whether or not the app is currently active.

Inside the `single-spa-demo-root-config` directory, in the `activity-functions.js` file, we'll write the following activity functions for our three micro-frontend apps.

{% gist https://gist.github.com/thawkin3/200ad0a0020a187b530824e48e0b42a0 %}

Next, we need to register our three micro-frontend apps with single-spa. To do that, we use the `registerApplication` function. This function accepts a minimum of three arguments: the app name, a method to load the app, and an activity function to determine when the app is active.

Inside the `single-spa-demo-root-config` directory, in the `root-config.js` file, we'll add the following code to register our apps:

{% gist https://gist.github.com/thawkin3/046076b06053ce1bb89dfa60bcef6b10 %}

Now that we've set up the activity functions and registered our apps, the last step before we can get this running locally is to update the local import map inside the `index.ejs` file in the same directory. We'll add the following code inside the `head` tag to specify where each app can be found when running locally:

{% gist https://gist.github.com/thawkin3/50013fbf1a5b536b26c80eeff19d7088 %}

Each app contains its own startup script, which means that each app will be running locally on its own development server during local development. As you can see, our navbar app is on port 9001, our page 1 app is on port 9002, and our page 2 app is on port 9003.

With those three steps taken care of, let's try out our app!

---

## Test Run for Running Locally

To get our app running locally, we can follow these steps:

1. Open four terminal tabs, one for each app
2. For the root config, in the `single-spa-demo-root-config` directory: `yarn start` (runs on port 9000 by default)
3. For the nav app, in the `single-spa-demo-nav` directory: `yarn start --port 9001`
4. For the page 1 app, in the `single-spa-demo-page-1` directory: `yarn start --port 9002`
5. For the page 2 app, in the `single-spa-demo-page-2` directory: `yarn start --port 9003`

Now, we'll navigate in the browser to http://localhost:9000 to view our app. We should see... some text! Super exciting.

![Demo app - main page](https://dev-to-uploads.s3.amazonaws.com/i/nr9agnkrbwcyjrmv295o.png)
<figcaption>Demo app - main page</figcaption>

On our main page, the navbar is showing because the navbar app is always active.

Now, let's navigate to http://localhost:9000/page1. As shown in our activity functions above, we've specified that the page 1 app should be active (shown) when the URL path begins with "page1." So, this activates the page 1 app, and we should see the text for both the navbar and the page 1 app now.

![Demo app - page 1 route](https://dev-to-uploads.s3.amazonaws.com/i/lyt8u1okwd5h1pdafbvc.png)
<figcaption>Demo app - page 1 route</figcaption>

One more time, let's now navigate to http://localhost:9000/page2. As expected, this activates the page 2 app, so we should see the text for the navbar and the page 2 app now.

![Demo app - page 2 route](https://dev-to-uploads.s3.amazonaws.com/i/pfa0a7fbabacvngfutgx.png)
<figcaption>Demo app - page 2 route</figcaption>

---

## Making Minor Tweaks to the Apps

So far our app isn't very exciting to look at, but we do have a working micro-frontend setup running locally. If you aren't cheering in your seat right now, you should be!

Let's make some minor improvements to our apps so they look and behave a little nicer.

--

## Specifying the Mount Containers

First, if you refresh your page over and over when viewing the app, you may notice that sometimes the apps load out of order, with the page app appearing above the navbar app. This is because we haven't actually specified where each app should be mounted. The apps are simply loaded by [SystemJS](https://github.com/systemjs/systemjs), and then whichever app finishes loading fastest gets appended to the page first.

We can fix this by specifying a mount container for each app when we register them.

In our `index.ejs` file that we worked in previously, let's add some HTML to serve as the main content containers for the page:

{% gist https://gist.github.com/thawkin3/ef67c1a5ac9abea0d28abc05d148b01e %}

Then, in our `root-config.js` file where we've registered our apps, let's provide a fourth argument to each function call that includes the DOM element where we'd like to mount each app:

{% gist https://gist.github.com/thawkin3/79809aad325b4e970090e771ebc57053 %}

Now, the apps will always be mounted to a specific and predictable location. Nice!

---

## Styling the App

Next, let's style up our app a bit. Plain black text on a white background isn't very interesting to look at.

In the `single-spa-demo-root-config` directory, in the `index.ejs` file again, we can add some basic styles for the whole app by pasting the following CSS at the bottom of the `head` tag:

{% gist https://gist.github.com/thawkin3/7211599b6a3244fb03e1c0492eed58cf %}

Next, we can style our navbar app by finding the `single-spa-demo-nav` directory, creating a `root.component.css` file, and adding the following CSS:

{% gist https://gist.github.com/thawkin3/c511650cf5b5d09dbcc1cc11c75a8238 %}

We can then update the `root.component.js` file in the same directory to import the CSS file and apply those classes and styles to our HTML. We'll also change the navbar content to actually contain two links so we can navigate around the app by clicking the links instead of entering a new URL in the browser's address bar.

{% gist https://gist.github.com/thawkin3/2d591275cd9d423f9292811d6b43dc73 %}

We'll follow a similar process for the page 1 and page 2 apps as well. We'll create a `root.component.css` file for each app in their respective project directories and update the `root.component.js` files for both apps too.

For the page 1 app, the changes look like this:

{% gist https://gist.github.com/thawkin3/8ea170130707e7e80699ba20f4e80c4f %}

{% gist https://gist.github.com/thawkin3/3a5da5686246c3ee8e7d8a6989f8deb6 %}

And for the page 2 app, the changes look like this:

{% gist https://gist.github.com/thawkin3/164632738b0360ac690b039582d449d4 %}

{% gist https://gist.github.com/thawkin3/49d58a959b86883bc1de23cebfa36193 %}

---

## Adding React Router

The last small change we'll make is to add [React Router](https://reacttraining.com/react-router/) to our app. Right now the two links we've placed in the navbar are just normal anchor tags, so navigating from page to page causes a page refresh. Our app will feel much smoother if the navigation is handled client-side with React Router.

To use React Router, we'll first need to install it. From the terminal, in the `single-spa-demo-nav` directory, we'll install React Router using yarn by entering `yarn add react-router-dom`. (Or if you're using npm, you can enter `npm install react-router-dom`.)

Then, in the `single-spa-demo-nav` directory in the `root.component.js` file, we'll replace our anchor tags with React Router's Link components like so:

{% gist https://gist.github.com/thawkin3/6df6afdf134cfaef137b41acc8d5ce0f %}

Cool. That looks and works much better!

![Demo app - styled and using React Router](https://dev-to-uploads.s3.amazonaws.com/i/lv4emo7oli8bwuxeojt5.png)
<figcaption>Demo app - styled and using React Router</figcaption>

---

## Getting Ready for Production

At this point we have everything we need to continue working on the app while running it locally. But how do we get it hosted somewhere publicly available? There are several possible approaches we can take using our tools of choice, but the main tasks are 1) to have somewhere we can upload our build artifacts, like a CDN, and 2) to automate this process of uploading artifacts each time we merge new code into the master branch.

For this article, we're going to use AWS S3 to store our assets, and we're going to use Travis CI to run a build job and an upload job as part of a continuous integration pipeline.

Let's get the S3 bucket set up first.

---

## Setting up the AWS S3 Bucket

It should go without saying, but you'll need an AWS account if you're following along here. If we are the root user on our AWS account, we can create a new IAM user that has programmatic access only. This means we'll be given an access key ID and a secret access key from AWS when we create the new user. We'll want to store these in a safe place since we'll need them later. Finally, this user should be given permissions to work with S3 only, so that the level of access is limited if our keys were to fall into the wrong hands.

AWS has some great resources for [best practices with access keys](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html) and [managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) that would be well worth checking out if you're unfamiliar with how to do this.

Next we need to create an S3 bucket. S3 stands for Simple Storage Service and is essentially a place to upload and store files hosted on Amazon's servers. A bucket is simply a directory. I've named my bucket "single-spa-demo," but you can name yours whatever you'd like. You can follow the AWS guides for [how to create a new bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html) for more info.

![AWS S3 bucket](https://dev-to-uploads.s3.amazonaws.com/i/l5hiudaew4i3u9vigng7.png)
<figcaption>AWS S3 bucket</figcaption>

Once we have our bucket created, it's also important to make sure the bucket is public and that [CORS (cross-origin resource sharing) is enabled for our bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html#how-do-i-enable-cors) so that we can access and use our uploaded assets in our app. In the permissions for our bucket, we can add the following CORS configuration rules:

{% gist https://gist.github.com/thawkin3/b4324fdc504b26ae43120e100dc146a2 %}

In the AWS console, it ends up looking like this after we hit Save:

![CORS configuration](https://dev-to-uploads.s3.amazonaws.com/i/ytk9vsjy30b1v3bdwv6g.png)
<figcaption>CORS configuration</figcaption>

---

## Creating a Travis CI Job to Upload Artifacts to AWS S3

Now that we have somewhere to upload files, let's set up an automated process that will take care of uploading new JavaScript bundles each time we merge new code into the master branch for any of our repos.

To do this, we're going to use [Travis CI](https://travis-ci.org/). As mentioned earlier, each app lives in its own repo on GitHub, so we have four GitHub repos to work with. We can [integrate Travis CI with each of our repos](https://docs.travis-ci.com/user/tutorial/#to-get-started-with-travis-ci-using-github) and set up continuous integration pipelines for each one.

To configure Travis CI for any given project, we create a `.travis.yml` file in the project's root directory. Let's create that file in the `single-spa-demo-root-config` directory and insert the following code:

{% gist https://gist.github.com/thawkin3/f2b3fa29c2110ad5e4370dc7995440a8 %}

This implementation is what I came up with after reviewing the [Travis CI docs for AWS S3 uploads](https://docs.travis-ci.com/user/deployment-v2/providers/s3/) and a [single-spa Travis CI example config](https://github.com/single-spa/import-map-deployer/blob/master/examples/ci-for-javascript-repo/travis-digital-ocean-spaces/.travis.yml).

Because we don't want our AWS secrets exposed in our GitHub repo, we can store those as environment variables. You can place environment variables and their secret values within the Travis CI web console for anything that you want to keep private, so that's where the `.travis.yml` file gets those values from.

Now, when we commit and push new code to the master branch, the Travis CI job will run, which will build the JavaScript bundle for the app and then upload those assets to S3. To verify, we can check out the AWS console to see our newly uploaded files:

![Uploaded files as a result of a Travis CI job](https://dev-to-uploads.s3.amazonaws.com/i/4v0niu9j31ww2kwcopdn.png)
<figcaption>Uploaded files as a result of a Travis CI job</figcaption>

Neat! So far so good. Now we need to implement the same Travis CI configuration for our other three micro-frontend apps, but swapping out the directory names in the `.travis.yml` file as needed. After following the same steps and merging our code, we now have four directories created in our S3 bucket, one for each repo.

![Four directories within our S3 bucket](https://dev-to-uploads.s3.amazonaws.com/i/7hpyxvrk70w1bj3n19ut.png)
<figcaption>Four directories within our S3 bucket</figcaption>

---

## Creating an Import Map for Production

Let's recap what we've done so far. We have four apps, all living in separate GitHub repos. Each repo is set up with Travis CI to run a job when code is merged into the master branch, and that job handles uploading the build artifacts into an S3 bucket. With all that in one place, there's still one thing missing: How do these new build artifacts get referenced in our container app? In other words, even though we're pushing up new JavaScript bundles for our micro-frontends with each new update, the new code isn't actually used in our container app yet!

If we think back to how we got our app running locally, we used an import map. This import map is simply JSON that tells the container app where each JavaScript bundle can be found. But, our import map from earlier was specifically used for running the app locally. Now we need to create an import map that will be used in the production environment.

If we look in the `single-spa-demo-root-config` directory, in the `index.ejs` file, we see this line:

{% gist https://gist.github.com/thawkin3/82211b82741ab322fd7029d46d134540 %}

Opening up that URL in the browser reveals an import map that looks like this:

{% gist https://gist.github.com/thawkin3/4cf8e4c253aed198631a4c35dad79e2b %}

That import map was the default one provided as an example when we used the CLI to generate our container app. What we need to do now is replace this example import map with an import map that actually references the bundles we're using.

So, using the original import map as a template, we can create a new file called `importmap.json`, place it *outside of our repos* and add JSON that looks like this:

{% gist https://gist.github.com/thawkin3/b01efe47a627bd932e890fbed447afda %}

You'll note that the first three imports are for shared dependencies: react, react-dom, and single-spa. That way we don't have four copies of React in our app causing bloat and longer download times. Next, we have imports for each of our four apps. The URL is simply the URL for each uploaded file in S3 (called an "object" in AWS terminology).

Now that we have this file created, we can manually upload it to our bucket in S3 through the AWS console. (This is a pretty important and interesting caveat when using single-spa: The import map doesn't actually live anywhere in source control or in any of the git repos. That way, the import map can be updated on the fly without requiring checked-in changes in a repo. We'll come back to this concept in a little bit.)

![Import map manually uploaded to the S3 bucket](https://dev-to-uploads.s3.amazonaws.com/i/vy3jft4kv3aelzveydg5.png)
<figcaption>Import map manually uploaded to the S3 bucket</figcaption>

Finally, we can now reference this new file in our `index.ejs` file instead of referencing the original import map.

{% gist https://gist.github.com/thawkin3/840f488a7c2ca5fa8290fa4442976d35 %}

---

## Creating a Production Server

We are getting closer to having something up and running in production! We're going to host this demo on Heroku, so in order to do that, we'll need to create a simple Node.js and [Express](https://expressjs.com/) server to serve our file.

First, in the `single-spa-demo-root-config` directory, we'll install express by running `yarn add express` (or `npm install express`). Next, we'll add a file called `server.js` that contains a small amount of code for starting up an express server and serving our main `index.html` file.

{% gist https://gist.github.com/thawkin3/85b2d7da77a2afd9f7c7e3d156eece02 %}

Finally, we'll update the NPM scripts in our `package.json` file to differentiate between running the server in development mode and running the server in production mode.

{% gist https://gist.github.com/thawkin3/21ae536a04f138d3074e62d76514326f %}

---

## Deploying to Heroku

Now that we have a production server ready, let's get this thing deployed to Heroku! In order to do so, you'll need to have a [Heroku account created](https://signup.heroku.com/), the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) installed, and be logged in. Deploying to Heroku is as easy as 1-2-3:

1. In the `single-spa-demo-root-config` directory: `heroku create thawkin3-single-spa-demo` (changing that last argument to a unique name to be used for your Heroku app)
2. `git push heroku master`
3. `heroku open`

And with that, we are up and running in production! Upon running the `heroku open` command, you should see your app open in your browser. Try navigating between pages using the nav links to see the different micro-frontend apps mount and unmount.

![Demo app - up and running in production](https://dev-to-uploads.s3.amazonaws.com/i/zbupb8uv5byk697w0b8s.png)
<figcaption>Demo app - up and running in production</figcaption>

---

## Making Updates

At this point, you may be asking yourself, "All that work for this? Why?" And you'd be right. Sort of. This is a lot of work, and we don't have much to show for it, at least not visually. But, we've laid the groundwork for whatever app improvements we'd like! The setup cost for any microservice or micro-frontend is often a lot higher than the setup cost for a monolith; it's not until later that you start to reap the rewards.

So let's start thinking about future modifications. Let's say that it's now five or ten years later, and your app has grown. A lot. And, in that time, a hot new framework has been released, and you're dying to re-write your entire app using that new framework. When working with a monolith, this would likely be a years-long effort and may be nearly impossible to accomplish. But, with micro-frontends, you could swap out technologies one piece of the app at a time, allowing you to slowly and smoothly transition to a new tech stack. Magic!

Or, you may have one piece of your app that changes frequently and another piece of your app that is rarely touched. While making updates to the volatile app, wouldn't it be nice if you could just leave the legacy code alone? With a monolith, it's possible that changes you make in one place of your app may affect other sections of your app. What if you modified some stylesheets that multiple sections of the monolith were using? Or what if you updated a dependency that was used in many different places? With a micro-frontend approach, you can leave those worries behind, refactoring and updating one app where needed while leaving legacy apps alone.

But, how do you make these kinds of updates? Or updates of any sort, really? Right now we have our production import map in our `index.ejs` file, but it's just pointing to the file we manually uploaded to our S3 bucket. If we wanted to release some new changes right now, we'd need to push new code for one of the micro-frontends, get a new build artifact, and then manually update the import map with a reference to the new JavaScript bundle.

Is there a way we could automate this? Yes!

---

## Updating One of the Apps

Let's say we want to update our page 1 app to have different text showing. In order to automate the deployment of this change, we can update our CI pipeline to not only build an artifact and upload it to our S3 bucket, but to also update the import map to reference the new URL for the latest JavaScript bundle.

Let's start by updating our `.travis.yml` file like so:

{% gist https://gist.github.com/thawkin3/1e673f7c513dc25f2d28c07bf9f5f59e %}

The main changes here are adding a global environment variable, installing the AWS CLI, and adding an `after_deploy` script as part of the pipeline. This references an `after_deploy.sh` file that we need to create. The contents will be:

{% gist https://gist.github.com/thawkin3/b34d8dbbb624ca73109e12f85ecc3b78 %}

This file downloads the existing import map from S3, modifies it to reference the new build artifact, and then re-uploads the updated import map to S3. To handle the actual updating of the import map file's contents, we use a custom script that we'll add in a file called `update-importmap.mjs`.

{% gist https://gist.github.com/thawkin3/86a518cb2666ded09a8673551ecbbd12 %}

Note that we need to make these changes for these three files in all of our GitHub repos so that each one is able to update the import map after creating a new build artifact. The file contents will be nearly identical for each repo, but we'll need to change the app names or URL paths to the appropriate values for each one.

---

## A Side Note on the Import Map

Earlier I mentioned that the import map file we manually uploaded to S3 doesn't actually live anywhere in any of our GitHub repos or in any of our checked-in code. If you're like me, this probably seems really odd! Shouldn't everything be in source control?

The reason it's not in source control is so that our CI pipeline can handle updating the import map with each new micro-frontend app release. If the import map were in source control, making an update to one micro-frontend app would require changes in two repos: the micro-frontend app repo where the change is made, and the root config repo where the import map would be checked in. This sort of setup would invalidate one of micro-frontend architecture's main benefits, which is that each app can be deployed completely independent of the other apps. In order to achieve some level of source control on the import map, we can always use S3's versioning feature for our bucket.

---

## Moment of Truth

With those modifications to our CI pipelines in place, it's time for the final moment of truth: Can we update one of our micro-frontend apps, deploy it independently, and then see those changes take effect in production without having to touch any of our other apps?

In the `single-spa-demo-page-1` directory, in the `root.component.js` file, let's change the text from "Page 1 App" to "Page 1 App - UPDATED!" Next, let's commit that change and push and merge it to master. This will kick off the Travis CI pipeline to build the new page 1 app artifact and then update the import map to reference that new file URL.

If we then navigate in our browser to https://thawkin3-single-spa-demo.herokuapp.com/page1, we'll now see... drum roll please... our updated app!

![Demo app - successfully updating one of the micro-frontend apps](https://dev-to-uploads.s3.amazonaws.com/i/a2gc7n42d9aooj9ue3c0.png)
<figcaption>Demo app - successfully updating one of the micro-frontend apps</figcaption>

---

## Conclusion

I said it before, and I'll say it again: **Micro-frontends are the future of frontend web development.** The benefits are massive, including independent deployments, independent areas of ownership, faster build and test times, and the ability to mix and match various frameworks if needed. There are some drawbacks, such as the initial set up cost and the complexity of maintaining a distributed architecture, but I strongly believe the benefits outweigh the costs.

Single-spa makes micro-frontend architecture easy. Now you, too, can go break up the monolith!
