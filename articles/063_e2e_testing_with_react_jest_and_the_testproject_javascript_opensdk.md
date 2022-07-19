# E2E Testing with React, Jest, and the TestProject JavaScript OpenSDK

With a long list of end-to-end (e2e) test frameworks available to choose from, it’s hard to know which one you should be using. [Cypress](https://www.cypress.io/) and [Selenium](https://www.selenium.dev/) are leading the market as the most widely used options, but there’s also [Appium](https://appium.io/) for mobile app testing, [Puppeteer](https://pptr.dev/) for automating tasks in Chrome, and [Protractor](https://www.protractortest.org/) for Angular and AngularJS applications, just to name a few.

Recently a newcomer has joined the pack: [TestProject](https://testproject.io/), a free, open-source test automation platform for e2e testing that helps simplify web, mobile, and API testing. The TestProject SDK has language support for Java, C#, Python, and, most recently, JavaScript.

In this article we’ll show how we can use the [TestProject JavaScript OpenSDK](https://github.com/testproject-io/javascript-opensdk) to test a React app with [Jest](https://jestjs.io/) as our test framework.

Ready to get started?

---

## App Overview

To begin, let’s take a look at the demo app that we’ll be testing. This app is relatively straightforward: just a simple request form in which a user can enter their first name, last name, and email address.

![Demo app: request form](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/paikwyp9f97o613rftie.png)
<figcaption>Demo app: request form</figcaption>

If the form is submitted without being properly filled out, error messages are shown below each invalid input.

![Demo app: invalid inpu](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ma8wulszlamio34pfccj.png)
<figcaption>Demo app: invalid input</figcaption>

Upon successful form submission, the app shows some confirmation text.

![Demo app: filling out the form](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/khmnf4b2h88n7gohx4nt.png)
<figcaption>Demo app: filling out the form</figcaption>

![Demo app: confirmation page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4on8gsk19pnfm86rb3c3.png)
<figcaption>Demo app: confirmation page</figcaption>

Simple enough, right? If you’d like to see the demo in action, you can [find the demo app hosted here](http://tylerhawkins.info/testproject-demo/build/) or [view the source code on GitHub](https://github.com/thawkin3/testproject-demo).

Now, let’s look at how the app was made.

---

## Creating the React App

As noted above, this app is written in React. To simplify the boilerplate code and dev tooling, I used the [create-react-app](https://github.com/facebook/create-react-app) tool to bootstrap the app.

```sh
npx create-react-app testproject-demo
```

With the skeleton app generated, I then removed the default app content and wrote a simple form component in a file called `RequestForm.js`. Here’s the request form code reproduced in full:

```js
import React, { useState } from 'react'
import './RequestForm.css'

export const RequestForm = () => {
  const [firstName, setFirstName] = useState('')
  const [lastName, setLastName] = useState('')
  const [email, setEmail] = useState('')

  const handleFirstNameChange = e => {
    setFirstName(e.target.value)
  }

  const handleLastNameChange = e => {
    setLastName(e.target.value)
  }

  const handleEmailChange = e => {
    setEmail(e.target.value)
  }

  const [firstNameError, setFirstNameError] = useState('')
  const [lastNameError, setLastNameError] = useState('')
  const [emailError, setEmailError] = useState('')

  const [submitted, setSubmitted] = useState(false)

  const handleSubmit = e => {
    e.preventDefault()

    setFirstNameError(firstName ? '' : 'First Name field is required')
    setLastNameError(lastName ? '' : 'Last Name field is required')
    setEmailError(email ? '' : 'Email field is required')

    if (firstName && lastName && email) {
      setSubmitted(true)
    }
  }

  return submitted ? (
    <p id="submissionConfirmationText">
      Thank you! We will be in touch with you shortly.
    </p>
  ) : (
    <form className="requestForm" onSubmit={handleSubmit}>
      <div className={`formGroup${firstNameError ? ' error' : ''}`}>
        <label htmlFor="firstName">First Name</label>
        <input
          name="firstName"
          id="firstName"
          data-testid="firstName"
          value={firstName}
          onChange={handleFirstNameChange}
        />
      </div>
      {firstNameError && (
        <p className="errorMessage" id="firstNameError">
          {firstNameError}
        </p>
      )}
      <div className={`formGroup${lastNameError ? ' error' : ''}`}>
        <label htmlFor="lastName">Last Name</label>
        <input
          name="lastName"
          id="lastName"
          data-testid="lastName"
          value={lastName}
          onChange={handleLastNameChange}
        />
      </div>
      {lastNameError && (
        <p className="errorMessage" id="lastNameError">
          {lastNameError}
        </p>
      )}
      <div className={`formGroup${emailError ? ' error' : ''}`}>
        <label htmlFor="email">Email</label>
        <input
          type="email"
          name="email"
          id="email"
          data-testid="email"
          value={email}
          onChange={handleEmailChange}
        />
      </div>
      {emailError && (
        <p className="errorMessage" id="emailError">
          {emailError}
        </p>
      )}
      <button type="submit" id="requestDemo">
        Request Demo
      </button>
    </form>
  )
}
```

As you can see, we have a function component that displays three inputs for the user’s first name, last name, and email address. There is a “Request Demo” submit button at the bottom of the form. When the form is submitted, error messages are shown if there are any invalid inputs, and a confirmation message is shown if the form is submitted successfully.

That’s really all there is to the app. Now, on to the fun part. How can we configure our end-to-end tests with TestProject?

---

## Getting Started with TestProject

To begin, we’ll need to first [create a free TestProject account](https://testproject.io/). After that, we can [download the TestProject agent](https://app.testproject.io/#/download). There are options to either download the agent for desktop or for Docker. Which one you choose is up to you, but I chose to download the desktop app for Mac. You then need to [register your agent](https://app.testproject.io/#/agents) to link your agent to your TestProject account.

Next, we’ll [generate a developer token](https://app.testproject.io/#/integrations/sdk) to use in our project. Once we have a developer token, we’ll create an `.env` file in the root directory of our project and add the following line of code to store our token in the `TP_DEV_TOKEN` environment variable:

```sh
TP_DEV_TOKEN=<YOUR DEV TOKEN HERE>
```

You’ll note that we tell Git in our `.gitignore` file to ignore our `.env` file so that our token or other environment secrets don’t get committed into our version control and accidentally shared with others.

Finally, we’ll need to install a couple npm packages as `devDependencies` to use the TestProject JavaScript OpenSDK in our app:

```sh
yarn add --dev @tpio/javascript-opensdk selenium-webdriver
```

With that, we’ve laid most of the groundwork to begin using TestProject with our e2e tests.

---

## Configuring Jest

Next up, we need to configure Jest. Since we used create-react-app to bootstrap our app, our project uses [react-scripts](https://www.npmjs.com/package/react-scripts) to run Jest and [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) with some default configuration options. However, it’d be nice if we could configure Jest and add a few more npm scripts to be able to run unit tests and e2e tests separately.

To do this, I added the following npm scripts to the “scripts” section of my `package.json` file. Each one contains some specific Jest CLI configuration options:

```json
"scripts": {
  ...other scripts here
  "start": "react-scripts start",
  "test:e2e": "wait-on http://localhost:3000/testproject-demo/build/ && react-scripts test --testPathPattern=\"(\\.|/)e2e\\.(test|spec)\\.[jt]sx?$\" --testTimeout=30000 --runInBand --watchAll=false",
  "test:e2e:ci": "run-p start test:e2e",
  "test:e2e:watch": "wait-on http://localhost:3000/testproject-demo/build/ && react-scripts test --testPathPattern=\"(\\.|/)e2e\\.(test|spec)\\.[jt]sx?$\" --testTimeout=30000 --runInBand",
  "test:unit": "react-scripts test --testPathPattern=\"(\\.|/)unit.(test|spec)\\.[jt]sx?$\" --watchAll=false",
  "test:unit:coverage": "react-scripts test --testPathPattern=\"(\\.|/)unit.(test|spec)\\.[jt]sx?$\" --watchAll=false --coverage",
  "test:unit:watch": "react-scripts test --testPathPattern=\"(\\.|/)unit.(test|spec)\\.[jt]sx?$\""
},
```

That’s a lot to take in! Let’s break down each of these commands while highlighting some of the key pieces of this code.

First, we see the `start` script. That one is easy enough: it runs our app locally in development mode. This is important because e2e tests require the app to be running in order to work properly.

Next, we see the `test:e2e script`. This command waits for the app to be running locally on port 3000 before attempting to run any tests. It then uses the react-scripts test command to run our app’s tests but with several Jest CLI configuration options applied. The `testPathPattern` option tells Jest to only run our tests that end in `e2e.test.js` (and a few other variations). The `testTimeout` option increases Jest’s default timeout of 5 seconds per test to 30 seconds per test since e2e tests take a little longer to run than simple unit tests. The `runInBand` option tells Jest to run our test files serially instead of in parallel since we only have one TestProject agent installed on our machine. And finally, the `watchAll=false` option makes it so that the tests do not run in “watch” mode, which is the default setting for Jest with react-scripts. Whew, that was a lot!

The third script is `test:e2e:ci`. This command is a combination of the start and `test:e2e` commands to help simplify the testing process. In order to use the original `test:e2e` command, we first must be running the app locally. So we’d need to first run `yarn start` and then run `yarn test:e2e`. That’s not a huge deal, but now we have an even simpler process in which we can just run `yarn test:e2e:ci` to both start the app and run the e2e tests.

The fourth script, `test:e2e:watch`, is very similar to the `test:e2e` script but runs the tests in “watch” mode in case you want your tests to be continuously running in the background as you make changes to your app.

The last three scripts are for running unit tests. The `test:unit` script runs the unit tests with Jest and React Testing Library and only looks for tests that end in `unit.test.js` (and a few other variations). The `test:unit:coverage` script runs those same unit tests but also includes a test coverage report. And finally, the `test:unit:watch script` runs the unit tests in watch mode.

This may seem like a lot of information to take in, but the takeaway here is that we’ve now created several helpful npm scripts that allow us to easily run our unit and e2e tests with short and simple commands. All of the hard configuration work is out of the way, so now we can focus on writing the actual tests.

---

## Writing Tests with the JavaScript OpenSDK

We now have Jest and TestProject configured for our project, so we’re ready to write our first e2e test. End-to-end tests typically focus on critical workflows of the app represented by user journeys.

For our request form, I can think of two important user journeys: when a user tries to submit an invalid form and when a user successfully submits a properly filled out form. Let’s write an e2e test for each workflow.

Our complete `App.e2e.test.js` file looks like this:

```js
import { By } from 'selenium-webdriver'
import { Builder } from '@tpio/javascript-opensdk'

describe('App', () => {
  const testUrl = 'http://localhost:3000/testproject-demo/build/'

  let driver

  beforeEach(async () => {
    driver = await new Builder()
      .forBrowser('chrome')
      .withProjectName('TestProject Demo')
      .withJobName('Request Form')
      .build()
  })

  afterEach(async () => {
    await driver.quit()
  })

  it('allows the user to submit the form when filled out properly', async () => {
    await driver.get(testUrl)
    await driver.findElement(By.css('#firstName')).sendKeys('John')
    await driver.findElement(By.css('#lastName')).sendKeys('Doe')
    await driver.findElement(By.css('#email')).sendKeys('john.doe@email.com')
    await driver.findElement(By.css('#requestDemo')).click()

    await driver
      .findElement(By.css('#submissionConfirmationText'))
      .isDisplayed()
  })

  it('prevents the user from submitting the form when not filled out properly', async () => {
    await driver.get(testUrl)
    await driver.findElement(By.css('#requestDemo')).click()

    await driver.findElement(By.css('#firstNameError')).isDisplayed()
    await driver.findElement(By.css('#lastNameError')).isDisplayed()
    await driver.findElement(By.css('#emailError')).isDisplayed()
  })
})
```

In our first test, we ensure that a user can successfully submit the form. We navigate to our app’s url, use the `sendKeys` method to enter text into the three input fields, and then click the submit button. We then wait for the confirmation text to appear on the screen to validate that our submission was successful.

You’ll note that all of the selectors look just like normal Selenium selectors. You will typically find elements using CSS selectors or using the XPath selector.

In our second test, we ensure that a user is prevented from submitting the form when there are invalid inputs on the page. We first navigate to our app’s url and then immediately click the submit button without filling out any of the input fields. We then verify that the three error messages are displayed on the screen.

You’ll also note that we’ve extracted out some of the shared test setup and teardown into the `beforeEach` and `afterEach` blocks. In the `beforeEach` block, we create our web driver for Chrome. In the `afterEach` block, we quit the driver.

---

## Running Our E2E Tests

Here’s the moment of truth: Let’s try running our end-to-end tests. In our terminal, we’ll run `yarn test:e2e:ci` to start the app and run the e2e tests. And… the two tests pass! You should see the app open on the Chrome browser, see the steps for each test be executed, and then see the test results back in the terminal:

![Running our e2e tests in the terminal](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q72cp6hecp9neju6f2zh.png)
<figcaption>Running our e2e tests in the terminal</figcaption>

TestProject even provides its own free, built-in report dashboards as well as local HTML and PDF reports so that you can see the test results within the browser. This is perfect when viewing tests that are run as part of a CI pipeline. Here is my report after running the test suite twice:

![TestProject report dashboard](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6q3eoromqlmf4jl10azq.png)
<figcaption>TestProject report dashboard</figcaption>

---

## Conclusion

Well, we did it! We successfully wrote and ran end-to-end tests using React, Jest, and the TestProject JavaScript OpenSDK. Mission accomplished.

The exciting thing is that this is just the tip of the iceberg. TestProject is full of other hidden gems like the ability to [write tests directly within the browser using a GUI](https://docs.testproject.io/using-the-smart-test-recorder/web-testing/creating-a-web-test-using-the-testproject-recorder), a plethora of [add-ons](https://addons.testproject.io/) for commonly needed test actions, and [integrations with apps like Slack](https://docs.testproject.io/testproject-integrations/integration-to-slack) for sending test report notifications.

Who knows what the future will hold for the world of end-to-end test automation, but TestProject is certainly a platform worth keeping your eye on.
