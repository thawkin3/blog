# Test-Driven Development for Building User Interfaces

Test-driven development, or TDD, is a programming paradigm in which you write your tests first and your source code second. TDD is perfect when you’re writing code that has clear inputs and outputs, like pure functions or API endpoints.

But what about when building user interfaces? Can TDD be done for UI development?

You’re about to find out!

In this article we’ll explore a few questions:

* ***Can*** we use TDD to build UIs?

* If so, ***how*** do we do it?

* And finally, ***should*** we use TDD to build UIs?

---

## Background Motivation

When discussing test-driven development with frontend developers, the conversation usually goes something like this:

“Yeah TDD is great for simple functions or backend work, but it just doesn’t make sense for frontend work. When I’m building my UI, I don’t know what code I’ll end up writing. I have no idea if I’ll end up using a `div` or a `span` or a `p` element here. TDD for UIs just isn’t feasible.”

However, I’d like to argue that using TDD to build UIs isn’t as hard as we may think.

---

## Ideal Conditions for TDD

Ideally, we’d use TDD to write our code when the following two conditions are true:

1. We have clear project requirements
2. We have clear inputs and outputs

If those two requirements are not met, it’s difficult or nearly impossible to use TDD. So let’s examine those two requirements in the context of frontend development.

---

### Clear Project Requirements

When you’re developing a new feature, you’re typically given mockups by a UX designer. These mockups show you how the feature should look and how the feature should behave. For example, “when the user clicks this button, a dialog modal appears on the screen.”

Good mockups will clarify various details such as how inputs will look when in a hover or focus state, how empty states will look when content is missing, and how the page layout will change for desktop, laptop, and mobile screen sizes.

As you may have already guessed, the mockups provide the project requirements! We know exactly how our UI should look and behave. If there’s anything unclear in the mockups, engineers should ask clarifying questions with their UX designer or product manager so that the requirements are absolutely clear.

---

### Clear Inputs and Outputs

Now, what about clear inputs and outputs?

Most frontend engineers these days use a UI library or framework like React or Angular. A UI library like React allows you to build reusable components to create small building blocks of functionality that you can piece together to make an app.

Now, what is a component? Well, in React, it’s a function! Components are simply functions of props and state that return a piece of UI. So we have clear inputs and outputs!

Given the same props and state, a component will always render the same thing. Components are deterministic, and as long as they don’t kick off side effects like making an API request, they are pure functions.

---

## Practical Considerations

So, in theory, using TDD to build UIs *should work*. Both of our ideal conditions are met.

But what about the unknowns? As mentioned above, we still might not know a few things:

1. Component props and state we’ll use
2. Names we’ll give our methods and functions
3. HTML elements we’ll use

But we *do* know how the UI should look and behave. I’d argue that the unknown implementation details actually don’t matter.

This outdated way of thinking about testing implementation details largely stems from Airbnb’s testing library [Enzyme](https://enzymejs.github.io/enzyme/). Enzyme allowed you to dive into the internals of your React components, trigger class component methods, and manually update a component’s props and state.

However, none of those are things that a user can do. A user can only interact with your app through the interface that you provide. For example, the user might click on a button or fill out a form field.

[React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)’s core philosophy is that we should write our tests in such a way that we simulate user behavior. By testing what the user can actually do, our tests focus less on implementation details and more on the actual user interface, which leads to less brittle tests and a more reliable test suite.

The key here is that React Testing Library actually facilitates using TDD to build UIs by taking the focus away from the implementation details.

Remember: the unknown implementation details don’t matter. What matters is how the UI looks and behaves.

---

## Want to Learn More?

If you'd like to see an in-depth real life demo for how we can use TDD to build UIs, [check out my followup article here](https://blog.testproject.io/2021/03/23/test-driven-development-for-building-user-interfaces/). We'll walk through how we can turn UX mockups into test cases, how we can adapt the "red, green, refactor" cycle for UI development, and see just how feasible using TDD to build UIs really is.

Happy coding!
