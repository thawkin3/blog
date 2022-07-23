# Using CodeSee Maps for Refactoring

Refactoring is the act of restructuring code without impacting the current functionality. Refactoring does not fix bugs or add new features, but it does improve the design of the code and the health of the codebase. Good developers know this and refactor their code frequently.

But what if you’re new to a codebase and don’t have a good grasp of the architecture yet? How do you confidently make changes to your code without the fear of breaking things?

Let’s explore two tools that help us refactor with confidence: test suites and CodeSee Maps.

---

## Refactoring and Test Suites

Ideally you can rely on your tests to know if you’ve broken something. A good test suite will have high test coverage and thoroughly test not just the happy path but also edge cases and error states. With a good test suite in place, if you make a change that breaks current functionality, your tests will let you know, thus preventing you from merging broken code.

But what if your codebase has no tests? Or maybe you’ve recently started writing tests but still haven’t quite reached the level of code coverage that you’d like. In either of these cases, refactoring your code is fairly high risk, as you may not know if you’ve broken something without careful manual testing.

The other thing to consider is that even with a good set of tests in place, the feedback is given after you’ve made your changes. What if you needed help knowing which areas of your codebase needed refactoring in the first place?

Or what if you’d like to know everywhere a component is used and all the downstream and upstream effects of modifying this component? Up until now, finding this info would require you to search in your IDE for the occurrences of the component and then build up your own mental map of where and how this component is used.

---

## Visualizing Your Codebase with CodeSee Maps

CodeSee Maps help you visualize your codebase before you start making changes. They allow you to explore your codebase, view any file, and see the upstream and downstream relationships for that file. This makes it incredibly easy to understand exactly what parts of your app will be impacted when modifying any given file.

To give you a better idea of what we mean, let’s look at an example app with some decent complexity. It’s a Reddit clone endearingly named Readit, and it’s an app for booklovers to discuss their favorite books. This app is written in React and uses GraphQL. You can [find the GitHub repo here](https://github.com/thawkin3/reddit-clone) or [view the demo app here](http://tylerhawkins.info/reddit-clone/build/).

![Reddit clone - Readit](https://lh6.googleusercontent.com/D8lHNRQtSDw6PhtvK55e0aOkuE0zSaz4idMwe0AhBFRnS8WL_n4itZCHj1wv1-otRj4pBQNBqcPPZZ9eEqVYY2L9MXXohMmNCAYGCzg0JfZvsIkuVUkV2rXvSMtZmGn73LFEG1i0_u741tMvSQ)
*Reddit clone - Readit*

This app, like Reddit, features a few simple pages: a Home page, a User page, a Subreadit page, and a Post page. Each of these pages rely on shared components, like a loading spinner, an error message, or a card.

But enough talking! Let’s use a CodeSee Map to visualize the code relationships in this repo.

![CodeSee Map for Readit codebase](https://lh6.googleusercontent.com/uN4qIZm0nUU2QmjerDGTaeX6QU6CNy1M-1dN2ioMbgAWeBdLCbSFUrcvDD7DTdCQY1Ok7fBAmFQUiDBvi9k2MMxIx907yx_ojdn-lOZ2BjKm9gQJMkeGLVd7UPQCGKeqaDJRABNgt3IBxiOZ4A)
*CodeSee Map for Readit codebase*

Here for example, we see the `SubreaditPage.js` file is currently selected. This component is rendered by the `App` component in the `App.js` file. The `SubreaditPage` component makes use of a CSS file as well as components for the `ErrorMessage`, `Subreddit`, `Post`, and `LoadingSpinner`.

You’ll note that some of these files are shown in pink or blue. These are custom labels that we’ve added to the CodeSee Map to help further clarify the app architecture. CodeSee Maps can be customized with labels, notes, tours, and more!

We can click on any of these files to drill down further to see what files they rely on. For example, we can see that the `Subreadit.js` file uses the `Card.js` file. Let’s look at the `Card.js` file to see what else uses it:

![Drill down view on the Card.js file](https://lh3.googleusercontent.com/greykgBEAqjgmrtYrmMOtITltM8TOwS8-sqkYUrg74YmWV318p0CbM5MdRktGiutM8G-ynkco2rNoOTFslc4tRZRyvoqbxtNOpOwJBZvhyEcA8flvb8y6DdZFh1G8kU_9Cp64E1Yuaozv4wlbg)
*Drill down view on the Card.js file*

Here we see that the `Card` component is used by many other components: `Comment`, `User`, `Subreadit`, and `Post`. So if we were to make changes to our `Card` component, we’d want to carefully validate all the places in our app where it’s used to ensure that we didn’t break any existing functionality.

Perhaps we might decide that the `Subreadit` component is too special of a use case for the `Card` component and that we’d like to sever that relationship. We could refactor our `Subreadit` component to no longer rely on the `Card` component, and this change would then be reflected in our CodeSee Map once we’ve merged the change.

As an added bonus to the CodeSee Map configured for your repo, any time you create a new pull request, a [Review Map](https://learn.codesee.io/codesee-releases-review-maps/) will be generated for you that highlights the changes you made. Now you can refactor with confidence and review code with confidence as well.

![CodeSee Review Map in a pull request](https://lh4.googleusercontent.com/9vM4ssLWlIUaoaYnn49Sq6TFTCioFHGRb3HEj57zranhQji3i9XERRjc3qNpIPFO6babnhDV9GNPWAZ8nKfE37ZR0zy9QIrJr41w7lCP5tBXNMPeT6PFCtYG5b2SJfsBasBcYmi5J1Jj6FFrmQ)
*CodeSee Review Map in a pull request*

---

## Conclusion

CodeSee Maps give us more insight into the architecture of our codebase. They show us the interaction between files. They help us determine which refactors are relatively safe and which are more risky. They can even help you in architecture efforts to split up your monolith!

CodeSee Maps make it easy to find the bounded contexts and natural lines in your codebase where separate domains exist, which provides a great starting point for identifying potential microservices you could make.

With a good set of tests and a CodeSee Map in your tool belt, refactoring with confidence is easy.
