# CI for APIs with the Kong Insomnia CLI and GitHub Actions

Insomnia is a desktop app from [Kong](https://konghq.com/?utm_source=guest&utm_medium=devspotlight&utm_campaign=community) that’s great for building, debugging, and testing backend APIs. While ad hoc manual testing is nice, wouldn’t it be even better to include our API tests in our continuous integration (CI) pipelines? With Inso, Kong Insomnia’s CLI tool, we can!

Inso allows you to run your automated API tests directly from the command line, which means setting up a workflow with GitHub Actions is a snap.

In this article, we’ll create a simple server with Node.js and [Express](https://expressjs.com/), write API tests using [Kong Insomnia](https://docs.insomnia.rest/), and then run these tests in our CI pipeline with [Inso](https://docs.insomnia.rest/inso-cli/introduction) and [GitHub Actions](https://resources.github.com/devops/tools/automation/actions).

---

## Demo App: A Nintendo Game Database

We’ve built a game database that contains info about every [NES](https://en.wikipedia.org/wiki/Nintendo_Entertainment_System) game ever published. The app is a server that implements a REST API with endpoints to get data about the games, categories, developers, publishers, and release years.

You can [find the complete code on GitHub](https://github.com/thawkin3/nes-games-api).

---

## Using Kong Insomnia for Manual Testing

While developing an API, rapid feedback cycles help ensure your API works the way you want and returns the data you expect. Kong Insomnia is perfect for this kind of ad hoc testing.

To get started with our NES game API, we created a new [Design Document](https://docs.insomnia.rest/insomnia/design-documents) inside Kong Insomnia. We left the information in the Design tab blank and headed over to the Debug tab to start making requests. Below, we have requests for each API endpoint our server provides. We can run each request inside Kong Insomnia, and the resulting data is displayed in the UI.

![Example API request in Insomnia](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5l4g9yoioswm9401t5l0.png)
<figcaption>Example API request in Insomnia</figcaption>

---

## Writing Tests with Kong Insomnia

Manually hitting our API endpoints is great for ad hoc testing and debugging, but ultimately what we want is an automated test suite that ensures our app is behaving correctly. [Kong Insomnia allows you to write tests](https://docs.insomnia.rest/insomnia/unit-testing) in the Test tab within the desktop app.

The tests are written by selecting one of the requests from the Debug tab and then making assertions about the data the server returns. You can run individual tests or an entire suite of tests.

As you can see below, we’ve written tests for each of our API endpoints for a total of 11 tests in our test suite.

![API tests in Insomnia](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/m6ijo4qkm8nxourrzsi1.png)
<figcaption>API tests in Insomnia</figcaption>

These tests (and the information in our Design Document) can be synced with Git and included in our code repo. That way, anyone with the Kong Insomnia desktop app can also run these requests and tests.

To [sync Kong Insomnia with Git](https://docs.insomnia.rest/insomnia/git-sync), simply click the “Setup Git Sync” button at the top of the app.

![Setup Git Sync button in Insomnia](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xisvr8oit6md60kvftza.png)
<figcaption>Setup Git Sync button in Insomnia</figcaption>

From there, you can provide the relevant details to connect Kong Insomnia with your project’s Git repo.

![Linking Insomnia with your GitHub repo](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zzx2u3gne0i39i2i3z3i.png)
<figcaption>Linking Insomnia with your GitHub repo</figcaption>

Syncing Kong Insomnia with your Git repo requires an authentication token. You can easily [create a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) within your account settings in GitHub.

![Creating a personal access token in GitHub](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ulbgxo3cy509xr35qvtv.png)
<figcaption>Creating a personal access token in GitHub</figcaption>

---

## Running Tests from the Command Line with Inso

We now have a set of requests and a suite of tests within Kong Insomnia that help us with our debugging and testing. But we haven’t automated the running of these tests yet, so let’s do that now. This is where Kong Insomnia’s CLI, Inso, comes into play.

Inso is installable as an npm package, so we’ve added that as a dev dependency to our project: `yarn add --dev insomnia-inso`.

To make running our test suite simple, we’ve created an npm script in our `package.json` file. The following script [runs our tests with Inso](https://docs.insomnia.rest/inso-cli/cli-command-reference/inso-run-test): `"test": "inso run test \"NES Games API Test Suite\""`. That means we can simply run `yarn test` to run our tests from the command line any time we want.

---

## Integrating Inso with GitHub Actions

Now for the culmination of everything we’ve set up so far: running our automated tests as part of a continuous integration pipeline. It’s nice that we can run our tests from the command line, but right now, we don’t have them running automatically for us anywhere. We really want our test suite to be run on every new pull request. Doing so will ensure that the tests pass before merging any new code into the master branch. This is continuous integration at its finest.

[GitHub Actions](https://docs.github.com/en/actions/automating-builds-and-tests/) allow us to configure any continuous integration we’d like by using YAML files. We’ve created a `.github/workflows` directory and a `unit-tests.yml` file inside it. This file is reproduced in full below:

```yaml
name: NES Games API CI

on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - master

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout branch
        uses: actions/checkout@v2

      - name: Use Node.js 14.x
        uses: actions/setup-node@v2
        with:
          node-version: 14.x
          cache: 'yarn'

      - name: Install dependencies
        run: yarn install --frozen-lockfile

      - name: Start server and run unit tests
        run: yarn ci:start-and-test
```

As you can see, our workflow checks out our current branch, uses Node v14, installs our dependencies with yarn, and then runs a custom script to start our app’s server and run the API tests. This workflow is triggered whenever code is pushed to the master branch or a new pull request is opened against the master branch.

This `ci:start-and-test` script is another npm script we’ve added to our `package.json` file. Using the npm packages [wait-on](https://www.npmjs.com/package/wait-on) and [concurrently](https://www.npmjs.com/package/concurrently), we use this script to start our server, run the API tests, and then kill the server once the tests finish. The full list of npm scripts in our `package.json` file is reproduced below:

```json
"scripts": {
  "ci:start-and-test": "concurrently -k -s=first \"yarn start\" \"yarn ci:test\"",
  "ci:test": "wait-on http://localhost:3000 && yarn test",
  "format": "prettier --write .",
  "format-watch": "onchange . -- prettier --write {{changed}}",
  "start": "node index.js",
  "test": "inso run test \"NES Games API Test Suite\""
},
```

And now, we have a beautiful working CI pipeline!

We can test that everything is working properly by creating a small pull request in our repo. After making the pull request, we can see that our CI pipeline is running:

![CI pipeline is running for our pull request in GitHub](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2ukvjwbnizb5thhmgn2j.png)
<figcaption>CI pipeline is running for our pull request in GitHub</figcaption>

If we click the Details button to see more info, we can see the individual steps of our workflow being run:

![CI pipeline steps](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ibn3hyay32b8ndewlwbw.png)
<figcaption>CI pipeline steps</figcaption>

Once the job passes, the results are reported back to the pull request as seen below:

![CI pipeline has passed for our pull request](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1kxmwivmrmy9ql0yb6zu.png)
<figcaption>CI pipeline has passed for our pull request</figcaption>

We did it! All checks have passed.

The last thing we could do for completeness is require that all checks pass in GitHub before pull requests can be merged. We can do this in the repo’s settings within GitHub:

![Require status checks to pass before merging pull requests in GitHub](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2se30tcuzznjytlrawjf.png)
<figcaption>Require status checks to pass before merging pull requests in GitHub</figcaption>

If you’d like to explore another potential CI setup, the Kong team also has their own [example workflow](https://docs.insomnia.rest/inso-cli/continuous-integration/) you can use that installs Inso as part of a job in the CI pipeline with their [setup-inso GitHub Action](https://github.com/marketplace/actions/setup-inso).

---

## Conclusion

So, what have we accomplished today? Quite a lot, actually! We’ve created a server using Node.js and Express. We’ve created ad hoc requests within Kong Insomnia to help with the development and debugging of our API. We’ve written tests inside Kong Insomnia and learned to run them from the command line using Inso. And finally, we’ve created a CI pipeline with GitHub Actions to run our API tests as part of every pull request for our repo.

Kong Insomnia, Inso, and GitHub Actions have worked together to help us develop our app with confidence. Our continuous integration pipeline has prepared us to deploy our app at any time. Success!

To go further, we could merge this workflow into a broader strategy for managing the entire API development lifecycle — design and development within Kong Insomnia, deployment with Inso integrated with our CI pipeline, and management and maintenance with a tool like [Kong Gateway](https://konghq.com/kong/?utm_source=guest&utm_medium=devspotlight&utm_campaign=community).

Thanks for reading, and happy coding!
