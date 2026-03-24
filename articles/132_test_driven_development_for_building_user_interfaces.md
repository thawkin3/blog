# Test-Driven Development for Building User Interfaces

Is it possible? Yes! Should you do it? Maybe!

![TDD for UI](https://cdn-images-1.medium.com/max/4280/1*6g9fkw2Bp2iPvNd5VrVdEQ.png)_TDD for UI_

Test-driven development, or TDD, is a programming paradigm in which you write your tests first and your source code second. TDD is perfect when you’re writing code that has clear inputs and outputs, like pure functions or API endpoints.

But what about when building user interfaces? Can TDD be done for UI development?

You’re about to find out!

In this article we’ll explore a few questions while building a demo UI:

- **_Can_** we use TDD to build UIs?
- If so, **_how_** do we do it?
- And finally, **_should_** we use TDD to build UIs?

_Note: This article was inspired by a conference talk I gave in January 2021 at THAT Conference Texas. This article was also written at that time. It’s been five years now, so some practices have changed. (Do most frontend engineers even know what Enzyme is anymore, or did they start out with React Testing Library?) However, I still think this is an interesting thought experiment and worth discussing, hence why I’m finally publishing this in its original form._

## Background Motivation

When discussing test-driven development with frontend developers, the conversation usually goes something like this:

“Yeah TDD is great for simple functions or backend work, but it just doesn’t make sense for frontend work. When I’m building my UI, I don’t know what code I’ll end up writing. I have no idea if I’ll end up using a `div` or a `span` or a `p` element here. TDD for UIs just isn’t feasible.”

However, I’d like to argue that using TDD to build UIs isn’t as hard as we may think.

## Ideal Conditions for TDD

Ideally, we’d use TDD to write our code when the following two conditions are true:

1. We have clear project requirements
2. We have clear inputs and outputs

If those two requirements are not met, it’s difficult or nearly impossible to use TDD. So let’s examine those two requirements in the context of frontend development.

### Clear Project Requirements

When you’re developing a new feature, you’re typically given mockups by a UX designer. These mockups show you how the feature should look and how the feature should behave. For example, “when the user clicks this button, a dialog modal appears on the screen.”

Good mockups will clarify various details such as how inputs will look when in a hover or focus state, how empty states will look when content is missing, and how the page layout will change for desktop, laptop, and mobile screen sizes.

As you may have already guessed, the mockups provide the project requirements! We know exactly how our UI should look and behave. If there’s anything unclear in the mockups, engineers should ask clarifying questions with their UX designer or product manager so that the requirements are absolutely clear.

### Clear Inputs and Outputs

Now, what about clear inputs and outputs?

Most frontend engineers these days use a UI library or framework like React or Angular. A UI library like React allows you to build reusable components to create small building blocks of functionality that you can piece together to make an app.

Now, what is a component? Well, in React, it’s a function! Components are simply functions of props and state that return a piece of UI. So we have clear inputs and outputs!

Given the same props and state, a component will always render the same thing. Components are deterministic, and as long as they don’t kick off side effects like making an API request, they are pure functions.

## Practical Considerations

So, in theory, using TDD to build UIs _should work_. Both of our ideal conditions are met.

But what about the unknowns? As mentioned above, we still might not know a few things:

1. Component props and state we’ll use
2. Names we’ll give our methods and functions
3. HTML elements we’ll use

But we _do_ know how the UI should look and behave. I’d argue that the unknown implementation details actually don’t matter.

This outdated way of thinking about testing implementation details largely stems from Airbnb’s testing library [Enzyme](https://enzymejs.github.io/enzyme/). Enzyme allowed you to dive into the internals of your React components, trigger class component methods, and manually update a component’s props and state.

However, none of those are things that a user can do. A user can only interact with your app through the interface that you provide. For example, the user might click on a button or fill out a form field.

[React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)’s core philosophy is that we should write our tests in such a way that we simulate user behavior. By testing what the user can actually do, our tests focus less on implementation details and more on the actual user interface, which leads to less brittle tests and a more reliable test suite.

The key here is that React Testing Library actually facilitates using TDD to build UIs by taking the focus away from the implementation details.

Remember: the unknown implementation details don’t matter. What matters is how the UI looks and behaves.

## Demo Time: Building a Request Form

Alright, enough theory. I hope by now I’ve presented a clear case that it should be possible to build a user interface using test-driven development.

But how does this actually look in practice?

To illustrate these concepts in a concrete way, we’ll build a simple UI in React using TDD. Let’s imagine that we need to create a request form that allows visitors on our site to request a demo of our new product. Here are the mockups that we’ll pretend we’ve been given:

![Mockup 1: Initial UI](https://cdn-images-1.medium.com/max/2000/0*sziVE-skkQqA-B_l)_Mockup 1: Initial UI_

As you can see, the form displays three inputs for a user’s first name, last name, and email address. There is a submit button at the bottom of the form.

If a user tries to submit the form without filling out all three fields, an error message will be displayed next to each invalid input:

![Mockup 2: Error messages appear on form submission if fields are left blank](https://cdn-images-1.medium.com/max/2000/0*bJ-4AvPyyuFUCC-S)_Mockup 2: Error messages appear on form submission if fields are left blank_

Finally, once the user properly fills out and submits the form, a confirmation screen is shown:

![Mockup 3: Confirmation text is shown after the form is successfully submitted](https://cdn-images-1.medium.com/max/2000/0*KxMcxuMB43xAGNcU)_Mockup 3: Confirmation text is shown after the form is successfully submitted_

## Getting Started

We’ll begin by bootstrapping a brand new app using `create-react-app`. Once my app was generated, I deleted and re-arranged some of the initial code to get to a good starting point. As you can see, we’ll start with a bare bones app that only returns a header element.

![App starting point](https://cdn-images-1.medium.com/max/2000/1*SDDnZ3T-U21qIXkTyT4q3A.png)_App starting point_

You can follow along here on the [demo/start](https://github.com/thawkin3/tdd-for-ui-demo/tree/demo/start) branch of my [GitHub repo](https://github.com/thawkin3/tdd-for-ui-demo).

## Test #1: Sanity Check

Let’s start by creating a `RequestForm.test.js` file. We’ll write our first test that asserts that our `RequestForm` component renders without crashing.

```jsx
import React from "react";
import { render, screen } from "@testing-library/react";
import { RequestForm } from "./RequestForm";

describe("RequestForm", () => {
  it("renders without crashing", () => {
    expect(() => render(<RequestForm />)).not.toThrow();
  });
});
```

As you might have guessed, this test fails because we don’t actually have a `RequestForm.js` file or a `RequestForm` component created yet. And that’s ok, because this is how TDD works — we write our tests first and then our source code.

![Our first test fails because the RequestForm.js file does not exist](https://cdn-images-1.medium.com/max/3156/1*ESEnVbSAzFTRdOtlnYtYNw.png)_Our first test fails because the RequestForm.js file does not exist_

Let’s now write some code to get this test to pass. We’ll create a `RequestForm.js` file that exports a `RequestForm` component. The `RequestForm` component will simply render a `form` HTML element.

```jsx
import React from "react";

export const RequestForm = () => <form />;
```

Now when we run our tests, they pass!

![Our first test passes now](https://cdn-images-1.medium.com/max/2000/1*1iQQL1S03VJj3UPjlrunxQ.png)_Our first test passes now_

## Test #2: First Name Input

Let’s move on to our second test. Let’s assert that our `RequestForm` component renders an input for the user to enter their first name.

```jsx
it("renders a first name text input", () => {
  render(<RequestForm />);
  expect(screen.getByLabelText("First Name")).toBeInTheDocument();
});
```

And now we’ll run our tests, and… this new test fails, as expected, because our `RequestForm` component doesn’t render any inputs yet. It’s still just an empty form.

![Our second test fails because the RequestForm component does not yet render an input for the first name](https://cdn-images-1.medium.com/max/2892/1*qEGYmlCynIhxlgxafWdXSQ.png)_Our second test fails because the RequestForm component does not yet render an input for the first name_

We can update our `RequestForm` component to render an `input` element and an accompanying `label` element that allows the user to enter their first name.

```jsx
import React from "react";

export const RequestForm = () => (
  <form>
    <label htmlFor="firstName">First Name</label>
    <input id="firstName" />
  </form>
);
```

Now when we run our tests, they all pass!

![Our second test passes now that the form renders an input for the first name](https://cdn-images-1.medium.com/max/2000/1*i5XzOM_LqJMgNl6urkAavA.png)_Our second test passes now that the form renders an input for the first name_

Here’s what our current app’s UI looks like:

![App renders a First Name input](https://cdn-images-1.medium.com/max/3320/1*90DRqk7AAMXBBkXwI7wWAg.png)_App renders a First Name input_

It’s not a very pretty app, but we’ll come back to style it up in a few minutes.

## Test #3: Last Name Input

Now let’s write another test that looks just like our test for the “first name” input, but this time let’s assert that there is a “last name” input on the page.

```jsx
it("renders a last name text input", () => {
  render(<RequestForm />);
  expect(screen.getByLabelText("Last Name")).toBeInTheDocument();
});
```

We’ll run our tests, and, you guessed it, this new test fails. No surprises there.

![Our third test fails because the RequestForm component does not yet render an input for the last name](https://cdn-images-1.medium.com/max/2832/1*dKtDF77Cd6BbTINu-cm_ZQ.png)_Our third test fails because the RequestForm component does not yet render an input for the last name_

So, just like before, let’s update our `RequestForm` component to render another `input` element and another `label` element, but this time for the user to enter their last name.

```jsx
import React from "react";

export const RequestForm = () => (
  <form>
    <label htmlFor="firstName">First Name</label>
    <input id="firstName" />
    <label htmlFor="lastName">Last Name</label>
    <input id="lastName" />
  </form>
);
```

We’ll run our tests again, and now they all pass. Nice!

![Our third test passes now that the form renders an input for the last name](https://cdn-images-1.medium.com/max/2000/1*-i2obH9RCH2xy0eWEexS2g.png)_Our third test passes now that the form renders an input for the last name_

Here’s what our app’s UI looks like now:

![App renders a First Name input and a Last Name input](https://cdn-images-1.medium.com/max/3332/1*AHGDiIrRABtsiU7XcAQm7w.png)_App renders a First Name input and a Last Name input_

Not very pretty, is it? With all of our tests currently passing, this seems like a great time for a small refactor.

## Refactor #1: Rendering Labels and Inputs on Individual Rows

Before we jump into our refactor, I’d like to call out a key principle here. When doing test-driven development, you follow what’s called a “red, green, refactor” cycle.

You start by writing a failing test, so you’re in the red. You then write the source code to make the test pass, which puts you in the green. Once you’re in the green, you can refactor your code all you want. At the end of your refactor, your existing tests must still pass, keeping you in the green.

![Red, green, refactor cycle by Nat Pryce](https://cdn-images-1.medium.com/max/2000/0*qa1bGIbpAL2P23qs)_Red, green, refactor cycle by Nat Pryce_

When it comes to UI development, we can consider style changes a refactor.

With that context provided, let’s now style up our app a little. It’d be nice to have our label-input pairs on separate rows. We could also add some breathing room between the label and the input text box.

To do this, we’ll add a `div` element with a `formGroup` class wrapped around our label-input pairs. We’ll also create a `RequestForm.css` file to include some CSS.

Here’s the `RequestForm.js` file:

```jsx
import React from "react";
import "./RequestForm.css";

export const RequestForm = () => (
  <form>
    <div className="formGroup">
      <label htmlFor="firstName">First Name</label>
      <input id="firstName" />
    </div>
    <div className="formGroup">
      <label htmlFor="lastName">Last Name</label>
      <input id="lastName" />
    </div>
  </form>
);
```

And here’s the accompanying `RequestForm.css` file that we import:

```css
.formGroup {
  margin-bottom: 1rem;
}

.formGroup label {
  padding-right: 1rem;
}
```

With those changes in place, our UI now looks like this:

![App with the label-input pairs on their own rows](https://cdn-images-1.medium.com/max/3316/1*TCNJNXSa7gselJXwGQZ-0w.png)_App with the label-input pairs on their own rows_

Much better! There is still more we can do style our form to make it match our mockups, but we’ll call it good for now.

Now, remember what we had learned about the red, green, refactor cycle? Refactors should happen while we’re in the green, and after we’re done with our refactor we should still be in the green. Let’s run our tests to see how things are looking:

![Our tests are still passing after our style refactor](https://cdn-images-1.medium.com/max/2000/1*HfiUXJnF7yPr82IVwE08mg.png)_Our tests are still passing after our style refactor_

Sweet! Our tests are still passing after our style refactor.

## Test #4: Email Address Input

Let’s move on to our next test. We have tests for the “first name” input and the “last name” input, so let’s now write one more test for the “email address” input.

```jsx
it("renders an email address text input", () => {
  render(<RequestForm />);
  expect(screen.getByLabelText("Email")).toBeInTheDocument();
});
```

This fails, as expected.

![Our fourth test fails because the RequestForm component does not yet render an input for the email address](https://cdn-images-1.medium.com/max/2784/1*1x7OSDqT5DXbRUoB1VRISA.png)_Our fourth test fails because the RequestForm component does not yet render an input for the email address_

Just like we did with the “first name” and “last name” inputs, we can add another `input` element and another `label` element so that the user can enter their email address.

```jsx
import React from "react";
import "./RequestForm.css";

export const RequestForm = () => (
  <form>
    <div className="formGroup">
      <label htmlFor="firstName">First Name</label>
      <input id="firstName" />
    </div>
    <div className="formGroup">
      <label htmlFor="lastName">Last Name</label>
      <input id="lastName" />
    </div>
    <div className="formGroup">
      <label htmlFor="email">Email</label>
      <input type="email" id="email" />
    </div>
  </form>
);
```

With that, our tests will all pass again.

![Our fourth test passes now that the form renders an input for the email address](https://cdn-images-1.medium.com/max/2000/1*DrVCY15ECsNtKrDSuLO39A.png)_Our fourth test passes now that the form renders an input for the email address_

Let’s check out the UI as it stands now:

![App renders inputs for first name, last name, and email address](https://cdn-images-1.medium.com/max/3324/1*K-7i1bsnRcyRVq_C-my_0Q.png)_App renders inputs for first name, last name, and email address_

Our inputs are still in rows, but the alignment on the “email” field could look better. It’d appear more uniform if all of the input boxes lined up nicely as if they were in a column. Let’s address this with a second refactor.

## Refactor #2: Lining Up the Columns

To better align our inputs, we can add a couple simple CSS rules to our existing styles. We’ll add a `width` and a `display` property to our `label` elements so that our full CSS file looks like this:

```css
.formGroup {
  margin-bottom: 1rem;
}

.formGroup label {
  padding-right: 1rem;
  display: inline-block;
  width: 6rem;
}
```

Now our UI looks just a little nicer:

![App with all form inputs aligned](https://cdn-images-1.medium.com/max/3292/1*TAqxkX-w8NzvF0mukZygBw.png)_App with all form inputs aligned_

And, the important part during any refactor: the tests all still pass.

![Our tests are still passing after our second style refactor](https://cdn-images-1.medium.com/max/2000/1*mBNnjc4Ozf62djYlwrDVfg.png)_Our tests are still passing after our second style refactor_

## Test #5: Submit Button

Let’s move on to our next test. This test will assert that a submit button with the text “Request Demo” appears in the form.

As I’m sure you can figure out by now, the test fails of course because we haven’t implemented the submit button yet.

![Our fifth test fails because the RequestForm component does not yet render a submit button](https://cdn-images-1.medium.com/max/3180/1*RoigHKxDg8EIHytBV2r9-A.png)_Our fifth test fails because the RequestForm component does not yet render a submit button_

We can easily add the submit button to our `RequestForm` component like so:

```jsx
import React from "react";
import "./RequestForm.css";

export const RequestForm = () => (
  <form>
    <div className="formGroup">
      <label htmlFor="firstName">First Name</label>
      <input id="firstName" />
    </div>
    <div className="formGroup">
      <label htmlFor="lastName">Last Name</label>
      <input id="lastName" />
    </div>
    <div className="formGroup">
      <label htmlFor="email">Email</label>
      <input type="email" id="email" />
    </div>
    <button type="submit">Request Demo</button>
  </form>
);
```

Now the tests pass again, putting us back in the green.

![Our fifth test passes now that the form renders a submit button](https://cdn-images-1.medium.com/max/2000/1*wrZmKtpTRWdO1UlW1yoUFA.png)_Our fifth test passes now that the form renders a submit button_

And the app’s UI now shows the “Request Demo” button too:

![App renders the three inputs and the submit button](https://cdn-images-1.medium.com/max/3280/1*Mpo_Fs6OGz2weZ1qCY3fwA.png)_App renders the three inputs and the submit button_

## Refactor #3: Centering and Styling the Form

If we look back at our design mockups, we’ll see that our form should be centered on the page and should have a thin black border around it. Since all our tests are passing and the form contents are complete, now seems like a good time to do a third refactor with some more style updates.

In our `RequestForm.js` file, we’ll add a `requestForm` class to our `form` element like so:

```jsx
<form className="requestForm">{/* the rest of the existing code here */}</form>
```

Then we’ll add some styles to that class in our `RequestForm.css` file:

```css
.requestForm {
  border: solid 0.0125rem #000;
  border-radius: 0.25rem;
  padding: 1rem;
  display: inline-block;
  text-align: left;
}
```

And finally, in our main `index.css` file that we haven’t touched at all during this exercise, we’ll add `text-align: center` on the `body` element. The resulting `index.css` file looks like this:

```css
html {
  margin: 0;
  padding: 0;
  font-size: 16px;
}

body {
  margin: 0;
  padding: 2rem;
  text-align: center;
  font-size: 100%;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto",
    "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans",
    "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```

And with that, our form is nice and centered on the page with a simple border:

![App renders a nice form centered on the page, matching the design mockups](https://cdn-images-1.medium.com/max/2000/1*QwBcVJpWuOi9LlqM7SLyTA.png)_App renders a nice form centered on the page, matching the design mockups_

And, as always, we made sure that our refactors did not break any of our existing tests:

![Our tests are still passing after our third style refactor](https://cdn-images-1.medium.com/max/2000/1*nasNpyapeLxkOs4vBuAHHw.png)_Our tests are still passing after our third style refactor_

## Test #6: Error Message for Invalid Inputs

Now that we have a nicely styled form on our page, it’s time to start focusing on the behavior of the form. As we can see in our second design mockup, we need error messages to display when the form is submitted if any of the input fields are not filled out.

Let’s first write a test to assert that error messages are shown when the form is submitted with invalid inputs.

At the top of our `RequestForm.test.js` file we’ll need to import a new method from React Testing Library called `fireEvent` so that we can click on the submit button as part of our test.

```jsx
import { render, screen, fireEvent } from "@testing-library/react";
```

Then we can write our new test:

```jsx
it("renders an error message for each of the inputs if none of them are filled out and the user submits the form", () => {
  render(<RequestForm />);
  fireEvent.click(screen.getByText("Request Demo"));

  expect(screen.getByText("First Name field is required")).toBeInTheDocument();

  expect(screen.getByText("Last Name field is required")).toBeInTheDocument();

  expect(screen.getByText("Email field is required")).toBeInTheDocument();
});
```

As you can see, we render the form, click the submit button, and then assert that all three input fields have an accompanying error message shown.

When we run our tests, this new test of course fails.

![Our sixth test fails because the RequestForm component does not yet handle the form submission or show error messages](https://cdn-images-1.medium.com/max/3560/1*Tub_v4t8UegTJ2INgg4Pww.png)_Our sixth test fails because the RequestForm component does not yet handle the form submission or show error messages_

To implement this functionality, we’ll make some fairly significant changes to our `RequestForm` component. First, we’ll change our uncontrolled inputs to be controlled inputs. In other words, we’ll let React manage each input’s `value` attribute and `onChange` handler. Second, we’ll add some error messages that are conditionally rendered when the user submits the form.

These changes result in the `RequestForm.js` file now looking like this:

```jsx
import React, { useState } from "react";
import "./RequestForm.css";

export const RequestForm = () => {
  const [firstName, setFirstName] = useState("");
  const [lastName, setLastName] = useState("");
  const [email, setEmail] = useState("");

  const handleChange = (e) => {
    const { name, value } = e.target;

    switch (name) {
      case "firstName":
        setFirstName(value);
        break;
      case "lastName":
        setLastName(value);
        break;
      case "email":
        setEmail(value);
        break;
      default:
        return;
    }
  };

  const [firstNameError, setFirstNameError] = useState("");
  const [lastNameError, setLastNameError] = useState("");
  const [emailError, setEmailError] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();

    setFirstNameError(firstName ? "" : "First Name field is required");
    setLastNameError(lastName ? "" : "Last Name field is required");
    setEmailError(email ? "" : "Email field is required");
  };

  return (
    <form className="requestForm" onSubmit={handleSubmit}>
      <div className="formGroup">
        <label htmlFor="firstName">First Name</label>
        <input
          name="firstName"
          id="firstName"
          data-testid="firstName"
          value={firstName}
          onChange={handleChange}
        />
      </div>
      {firstNameError && <p>{firstNameError}</p>}

      <div className="formGroup">
        <label htmlFor="lastName">Last Name</label>
        <input
          name="lastName"
          id="lastName"
          data-testid="lastName"
          value={lastName}
          onChange={handleChange}
        />
      </div>
      {lastNameError && <p>{lastNameError}</p>}

      <div className="formGroup">
        <label htmlFor="email">Email</label>
        <input
          type="email"
          name="email"
          id="email"
          data-testid="email"
          value={email}
          onChange={handleChange}
        />
      </div>
      {emailError && <p>{emailError}</p>}

      <button type="submit">Request Demo</button>
    </form>
  );
};
```

That was a lot to take in! Feel free to pause and take a minute to look through the code above if you need to.

Let’s look at our tests now that we’ve implemented the error message functionality:

![Our sixth test passes now that the form handles submission and validation](https://cdn-images-1.medium.com/max/3164/1*JWu9jleFOBR7bFfCsW44xA.png)_Our sixth test passes now that the form handles submission and validation_

They all pass! Beautiful! And how about the app’s UI? Here’s what the app looks like now after the user clicks the submit button without filling out any of the form fields:

![App displays unformatted error messages if the form is submitted with all empty fields](https://cdn-images-1.medium.com/max/2000/1*bpT1Jtf5YwFI74nCS9S8bA.png)_App displays unformatted error messages if the form is submitted with all empty fields_

As you can see, while the form is functional, it could use some style updates. That means it’s time for another refactor!

## Refactor #4: Styling the Error Messages

Looking back at our design mockups, we see that the error messages should be in red. The height of the form shouldn’t change when an error message is displayed either.

We can achieve this by adding a new class called `errorMessage` to our error message content. We will also conditionally add an `error` class to our `formGroup` element when the input is in an invalid state.

The resulting `RequestForm.js` file looks like this:

```jsx
import React, { useState } from "react";
import "./RequestForm.css";

export const RequestForm = () => {
  const [firstName, setFirstName] = useState("");
  const [lastName, setLastName] = useState("");
  const [email, setEmail] = useState("");

  const handleChange = (e) => {
    const { name, value } = e.target;
    switch (name) {
      case "firstName":
        setFirstName(value);
        break;
      case "lastName":
        setLastName(value);
        break;
      case "email":
        setEmail(value);
        break;
      default:
        return;
    }
  };

  const [firstNameError, setFirstNameError] = useState("");
  const [lastNameError, setLastNameError] = useState("");
  const [emailError, setEmailError] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    setFirstNameError(firstName ? "" : "First Name field is required");
    setLastNameError(lastName ? "" : "Last Name field is required");
    setEmailError(email ? "" : "Email field is required");
  };

  return (
    <form className="requestForm" onSubmit={handleSubmit}>
      <div className={`formGroup${firstNameError ? " error" : ""}`}>
        <label htmlFor="firstName">First Name</label>
        <input
          name="firstName"
          id="firstName"
          data-testid="firstName"
          value={firstName}
          onChange={handleChange}
        />
      </div>
      {firstNameError && <p className="errorMessage">{firstNameError}</p>}

      <div className={`formGroup${lastNameError ? " error" : ""}`}>
        <label htmlFor="lastName">Last Name</label>
        <input
          name="lastName"
          id="lastName"
          data-testid="lastName"
          value={lastName}
          onChange={handleChange}
        />
      </div>
      {lastNameError && <p className="errorMessage">{lastNameError}</p>}

      <div className={`formGroup${emailError ? " error" : ""}`}>
        <label htmlFor="email">Email</label>
        <input
          type="email"
          name="email"
          id="email"
          data-testid="email"
          value={email}
          onChange={handleChange}
        />
      </div>
      {emailError && <p className="errorMessage">{emailError}</p>}

      <button type="submit">Request Demo</button>
    </form>
  );
};
```

And the resulting `RequestForm.css` file looks like this:

```css
.formGroup {
  margin-bottom: 2rem;
}

.formGroup.error {
  margin-bottom: 0;
}

.formGroup label {
  padding-right: 1rem;
  display: inline-block;
  width: 6rem;
}
.requestForm {
  border: solid 0.0125rem #000;
  border-radius: 0.25rem;
  padding: 1rem;
  display: inline-block;
  text-align: left;
}

.errorMessage {
  color: red;
  margin-top: 0;
  line-height: 1;
}
```

Let’s check out our app’s UI after submitting an empty form again:

![App displays error messages in red and keeps the form the same height](https://cdn-images-1.medium.com/max/2000/1*hNk9DMMrOb7zyWNg1e3qQQ.png)_App displays error messages in red and keeps the form the same height_

Much better! The error messages are shown in red, and the form maintains a consistent height regardless of whether or not error messages appear on the screen.

As always, we’ll check our tests as well to make sure we didn’t break anything:

![Our tests are still passing after our fourth style refactor](https://cdn-images-1.medium.com/max/3180/1*lTxiUwNXstM2p8DYGieF5Q.png)_Our tests are still passing after our fourth style refactor_

Everything is still passing! Excellent. Another successful refactor.

## Test #7: Confirmation Message on Successful Form Submission

With that, we’re ready for our seventh and final test. We now want to assert that the form can be successfully submitted and that a confirmation screen is shown to the user.

We’ll write the following test:

```jsx
it("replaces the form with a confirmation message when submitted successfully", () => {
  render(<RequestForm />);
  fireEvent.change(screen.getByLabelText("First Name"), {
    target: { value: "Tyler" },
  });
  expect(screen.getByLabelText("First Name").value).toBe("Tyler");

  fireEvent.change(screen.getByLabelText("Last Name"), {
    target: { value: "Hawkins" },
  });
  expect(screen.getByLabelText("Last Name").value).toBe("Hawkins");

  fireEvent.change(screen.getByLabelText("Email"), {
    target: { value: "test@test.com" },
  });
  expect(screen.getByLabelText("Email").value).toBe("test@test.com");

  fireEvent.click(screen.getByText("Request Demo"));
  expect(
    screen.getByText("Thank you! We will be in touch with you shortly.")
  ).toBeInTheDocument();

  expect(screen.queryByLabelText("First Name")).not.toBeInTheDocument();
  expect(screen.queryByLabelText("Last Name")).not.toBeInTheDocument();
  expect(screen.queryByLabelText("Email")).not.toBeInTheDocument();
});
```

Walking through the test, we first render the form. We then fill out the form, one input at a time, and assert that the value of each input is what we would expect. Then we click the submit button to submit the form. We then assert that the confirmation message is shown on the screen and that the original form no longer appears.

When we run our tests, this test fails. (Shocker, right?)

![Our seventh test fails because the RequestForm component does not yet handle the successful form submission](https://cdn-images-1.medium.com/max/3404/1*vEiET_Nh26TxHDPISfcrUQ.png)_Our seventh test fails because the RequestForm component does not yet handle the successful form submission_

Now we can write our source code to implement the successful form submission functionality. We’ll add the confirmation text that is conditionally rendered when a new `submitted` piece of state is `true`. We’ll also break up our single `onChange` handler into separate `onChange` handlers just for fun.

The final `RequestForm.js` file looks like this:

```jsx
import React, { useState } from "react";
import "./RequestForm.css";

export const RequestForm = () => {
  const [firstName, setFirstName] = useState("");
  const [lastName, setLastName] = useState("");
  const [email, setEmail] = useState("");

  const handleFirstNameChange = (e) => {
    setFirstName(e.target.value);
  };

  const handleLastNameChange = (e) => {
    setLastName(e.target.value);
  };

  const handleEmailChange = (e) => {
    setEmail(e.target.value);
  };

  const [firstNameError, setFirstNameError] = useState("");
  const [lastNameError, setLastNameError] = useState("");
  const [emailError, setEmailError] = useState("");

  const [submitted, setSubmitted] = useState(false);

  const handleSubmit = (e) => {
    e.preventDefault();

    setFirstNameError(firstName ? "" : "First Name field is required");
    setLastNameError(lastName ? "" : "Last Name field is required");
    setEmailError(email ? "" : "Email field is required");

    if (firstName && lastName && email) {
      setSubmitted(true);
    }
  };

  return submitted ? (
    <p>Thank you! We will be in touch with you shortly.</p>
  ) : (
    <form className="requestForm" onSubmit={handleSubmit}>
      <div className={`formGroup${firstNameError ? " error" : ""}`}>
        <label htmlFor="firstName">First Name</label>
        <input
          name="firstName"
          id="firstName"
          data-testid="firstName"
          value={firstName}
          onChange={handleFirstNameChange}
        />
      </div>
      {firstNameError && <p className="errorMessage">{firstNameError}</p>}

      <div className={`formGroup${lastNameError ? " error" : ""}`}>
        <label htmlFor="lastName">Last Name</label>
        <input
          name="lastName"
          id="lastName"
          data-testid="lastName"
          value={lastName}
          onChange={handleLastNameChange}
        />
      </div>
      {lastNameError && <p className="errorMessage">{lastNameError}</p>}

      <div className={`formGroup${emailError ? " error" : ""}`}>
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
      {emailError && <p className="errorMessage">{emailError}</p>}

      <button type="submit">Request Demo</button>
    </form>
  );
};
```

And with that, we now have seven passing tests!

![Our seventh test passes now that the form handles successful submission and displaying a confirmation message](https://cdn-images-1.medium.com/max/3168/1*GCVcrVi6rvFx5T1uh7w6IA.png)_Our seventh test passes now that the form handles successful submission and displaying a confirmation message_

We can confirm the behavior manually in our app too by filling out the form fields:

![Filling out the form](https://cdn-images-1.medium.com/max/2000/1*Y1lUIQE9KE2gmxu5C-Egcg.png)_Filling out the form_

We’ll click the submit button and… voilà! A simple confirmation message appears on the screen.

![App displays a confirmation screen after successful form submission](https://cdn-images-1.medium.com/max/2000/1*949BaJYtO6TK0sx08RG5Dw.png)_App displays a confirmation screen after successful form submission_

We did it!

## Resulting Code Coverage

We’ve now finished building our “Request Demo” form. We’ve built out all the functionality requested by our product manager and UX designer, and the resulting UI matches the mockups perfectly.

But how did we do on our test coverage? Were the tests we wrote sufficient? We can generate the test coverage report by running `yarn test:coverage`. The output is below:

![100% code coverage. Nice!](https://cdn-images-1.medium.com/max/2824/0*WXXR79Udig9tCub5.png)_100% code coverage. Nice!_

Amazing! 100% test coverage. This is one of the major benefits of TDD: all of your code, in theory, should be covered by tests because the only source code you write is code to make your tests pass. And because the tests are derived from product requirements, you can be sure that they are testing the right things.

## Conclusion

So, what have we learned? In the beginning of this article, we were determined to answer three questions:

- **_Can_** we use TDD to build UIs?
- If so, **_how_** do we do it?
- And finally, **_should_** we use TDD to build UIs?

So, **_can_** we use TDD when building UIs? Yes! We absolutely can!

And **_how_** do we do this? By following a process similar to what we’ve outlined by building our demo app. We follow the red, green, refactor cycle and implement style changes during the “refactor” phase.

Now, **_should_** we use TDD when building UIs? Maybe. Test-driven development isn’t everyone’s cup of tea. But I strongly believe that all frontend developers should at least try it. Give it a shot and see if it works for you. As we’ve demonstrated, using TDD to build UIs isn’t as difficult as you may think. In fact, you may even find that TDD speeds up your development process while ensuring that you test the right things.

Happy coding!
