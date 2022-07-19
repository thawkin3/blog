# When to Write End-to-End Tests

When writing software, there are many different levels at which you can test your code: unit tests, integration tests, and end-to-end (e2e) tests.

So the question is: For any given piece of functionality, where and how should you test your code?

In this article we'll look at the different types of tests, the testing pyramid, and a real world example that ties it all together.

---

## Types of tests

**Unit tests** ensure that a single thing works properly on its own. You would generally write unit tests to validate something like a function, a backend API endpoint, or a UI component. Unit tests are perfect when the thing you're testing has clear inputs and outputs.

For example, pure functions are deterministic, always returning the same output when given the same input. You might write a unit test for a function that adds two numbers to verify that it returns the correct sum.

You might write a unit test for an API endpoint which takes a `userId` and returns an object containing the user's info to make sure it sends the correct response.

Or you might write a unit test for a React button component to ensure that the button text is shown and that the button responds appropriately when clicked.

---

**Integration tests** ensure that a few things work properly together. You're still excluding part of the system though or potentially mocking some data.

Kent Dodds' [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) is a good example of how to utilize integration tests. When you render a component using React Testing Library, it renders the full component tree. So if a component renders other child components, those child components are rendered and tested too. (This is in contrast to the concept of "shallow rendering" that is a common practice when testing components using [Enzyme](https://github.com/enzymejs/enzyme).) 

For example, maybe you have a simple form component which shows text inputs for a user's first name, last name, and email address. It also renders a Submit button. When you write tests for the form, you can verify that the button and all the inputs are rendered to screen, that you can fill out the form, and that clicking on the Submit button handles submitting the form.

However, there are still pieces of the app that are not being tested in this case. The form wouldn't really be hitting an API endpoint when it's submitted. And the entire app wouldn't be spun up since only the form component is being rendered.

---

**E2E tests** ensure that a full workflow functions properly. These workflows are often represented by "user journeys", or common tasks that a user might perform when using your app. E2E tests spin up your entire app and use a testing framework like [Cypress](https://www.cypress.io/) or [Selenium](https://www.selenium.dev/) to perform actual actions that a user would take.

For example, you might write an e2e test that verifies that users can create an account on your site. Your test would start your app, navigate to the sign-up page, fill out the form, and then submit it. This would hit a real API endpoint and insert an actual user into a real database. You'd then probably also verify that the user is navigated to a new page after signing up and that you see their user avatar or username somewhere on the page.

---

## The testing pyramid

Now that we understand what each type of test is, let's examine when we should write them. What proportion of your tests should be unit, integration, or e2e tests?

The generally agreed-on philosophy here is something called the testing pyramid. Take a look at the image below:

![The testing pyramid](https://dev-to-uploads.s3.amazonaws.com/i/bx092z53r76cvftlqayn.png)
<figcaption>The testing pyramid (<a href="https://medium.com/better-programming/the-test-pyramid-80d77535573">image found here</a>)</figcaption>

As you can see, the testing pyramid recommends that you have a large amount of unit tests, a medium amount of integration tests, and a small amount of e2e tests.

However, e2e tests are far superior in fully verifying that the entire workflow or user journey works properly.

Consider this one example gif that frequently circulates on Imgur and Reddit:

![Sliding door opening due to faulty lock installation](https://dev-to-uploads.s3.amazonaws.com/i/qmdla9f3piceur0z5zom.gif)
<figcaption>Why integration tests and e2e tests are important</figcaption>

The lock on its own functions properly, right? You can move it from an unlocked position on the left to a locked position on the right.

And the door functions properly on its own too. It can slide open and closed to allow people to enter and exit the room.

But, these two pieces don't function properly when used together! The lock assumes that the door it's placed on *swings* open and closed as opposed to *slides* open and closed. This evidently was a bad assumption, leading to a door that can't actually be locked.

A good integration test or e2e test would have caught that!

---

## Why not just always use e2e tests?

So, this example begs the question: Why not just always use e2e tests? They better represent how the app actually runs and don't rely on any assumptions that you might have been wrong about.

The answer, if you refer back to the testing pyramid image, is that e2e tests are slower and more expensive.

Since they use an actual app, they require a working server, frontend, backend, and database. If you run these tests as part of a continuous integration pipeline on every merge request (and you should!), then that means that for every new merge request, you have to provision resources in the cloud for your server and database. That can run up quite a large bill!

It also takes time to create new users, render the app, and wait for API requests to respond as you interact with the app. Unit tests and integration tests are much faster because it generally only takes a matter of milliseconds to execute a simple function.

Now, multiply that time by 1000. How much faster would 1000 unit tests be than 1000 e2e tests? The exact answer depends on the nature of the code and your app, but it's fairly safe to say that your unit tests could finish in about a minute, whereas the e2e tests would likely take an hour or more.

---

## What situations merit an e2e test?

The moral of the story is that you need to be selective in when you decide to write e2e tests. E2E tests should be reserved for critical workflows only.

For example, you definitely want to ensure that users can create new accounts on your site or that existing users can log into their account. If you are an e-commerce company, you'll absolutely want to ensure that a user can complete the checkout process to make a purchase on your site.

These user journeys are critical to your business, so they are worth the extra cost and time that e2e tests require.

What about verifying that certain content is rendered on the screen? Would you write an e2e test to make sure that the home page displays the correct welcome text? Probably not. That could be adequately tested using a unit test.

---

## Real world example: breadcrumbs

![Bread](https://dev-to-uploads.s3.amazonaws.com/i/e1kg12wzhnlr1hxg6wib.jpeg)
<figcaption>Photo by Pierre Bamin on Unsplash</figcaption>

Let's look at a real world example. Recently our team was re-designing how breadcrumbs in our app worked. The backend API was staying mostly the same, but the frontend UI was going to look and behave a little differently.

As we were working on this, we wrote tests for the following:

- Unit tests for the individual breadcrumb components (frontend)
- Integration tests for the breadcrumb UI as a whole (frontend)
- Unit tests for the API endpoint (backend)

With these tests, we could ensure that given some mock breadcrumb data, our frontend would look and behave as expected. We could also ensure that an API request with given request parameters would return the right breadcrumb response data.

What we couldn't promise though, was that the frontend and backend would work well together. What if the frontend components were expecting the data to be in a different format than what the backend was providing?

We were able to manually verify that the full experience was working of course, but we didn't have an e2e test in place to automatically make that verification for us.

We weighed the pros and cons of including or not including an e2e test.

Writing an e2e test would mean that our workflow would be 100% covered. But, that would also mean additional resource costs and additional time taken when running our test suite.

Not writing the e2e test would save us the extra time during our test pipeline job runs, but it would also leave open the possibility that the frontend and backend wouldn't work flawlessly together at some point in the future.

In the end, we decided that the breadcrumbs were not part of a critical user journey and therefore didn't merit writing an e2e test. We consciously accepted the risk that the frontend or backend API contracts might change in favor of not making our CI pipeline slower.

---

## Conclusion

It's tempting to think that adding one more e2e test will only add a few more seconds to your overall test suite run time, so why not just add it. However, as your engineering organization and app grows, those occurrences of "it's only one more e2e test" will quickly add up week over week.

If you're not being conscientious about when you add e2e tests, you'll soon be bogged down by a ridiculously slow test suite, costing your organization hours upon hours of lost time. Instead, tests should be written as low down on the testing pyramid as possible.

So remember: E2E tests are for critical workflows only.
