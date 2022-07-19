# Why I Re-Wrote the focus-trap-react Test Suite Using React Testing Library

One of my 2021 goals is to contribute more to open source. I got a head start in December of 2020 by re-writing the test suite for the [focus-trap-react](https://github.com/focus-trap/focus-trap-react) npm package using [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/). Here’s my merged [pull request](https://github.com/focus-trap/focus-trap-react/pull/222).

If you haven’t heard of this package, `focus-trap-react` is one of the best solutions out there for temporarily trapping keyboard focus in a specific area of your application, like while a modal or panel is open. We wouldn’t want users tabbing outside the modal into content in the background, now would we?

In this article we’ll go over my motivations for re-writing the test suite and some of the benefits that writing tests with React Testing Library brings.

---

## Pitfalls of the original test suite

It’s worth noting that some tests did exist for this package before I got started. These tests were written using [react-dom/test-utils](https://reactjs.org/docs/test-utils.html), the built-in test utils for React.

But, here’s the thing: no one actually uses the built-in React test utils directly. Even the React docs themselves suggest you use React Testing Library or Enzyme!

So to make the tests more maintainable and more familiar to the rest of the React programming world, I switched the test suite over to React Testing Library.

(Although, `react-dom/test-utils` does have some pretty funky method names that we’re missing out on now. If you don’t believe me, see for example: [scryRenderedDOMComponentsWithClass](https://reactjs.org/docs/test-utils.html#scryrendereddomcomponentswithclass).)

Second, the existing tests included mock methods to simulate creating a real focus trap. There was a `_createFocusTrap` prop implemented in the component specifically used for testing purposes only. The fact that we weren’t instantiating a new focus trap component the same way a developer would when using the component in their application is a pretty big red flag.

Third, the tests were mainly making assertions that certain functions were called or not called when the focus trap was activated or deactivated. The assertions looked like this:

```js
expect(mockCreateFocusTrap).toHaveBeenCalledTimes(1);
```

The problem here is that while we’re asserting that this function was called, we’re really not saying anything about the actual user experience or what is happening in the UI. The fact that this function was called is simply an implementation detail that our users — and quite frankly our tests — don’t need to know about.

So, how we can fix these problems? Is there a better way?

---

## React Testing Library is a better way to test

The core philosophy of React Testing Library is that you should test your components in the same way that your user would interact with the UI.

No more forcing updates to a component’s state or asserting that functions are called. Instead, we do things that a user would do: click on a button, type into a text input, submit a form.

So, in regard to the `focus-trap-react` package, that’s exactly what I did. I wrote the tests by interacting with the UI doing only things that a user could do.

For example, one test for the default behavior of the focus trap does the following:

1. Renders a component that contains a button which toggles the focus trap content when clicked
2. Clicks the button to show the focus trap content and activate the trap
3. Verifies that focus is moved to the first element inside the focus trap
4. Tabs through the tabbable elements in the focus trap and then verifies that the focus is in fact trapped and wraps back to the beginning when you tab from the last item in the focus trap
5. Clicks a button to deactivate the focus trap
6. Verifies that focus is returned to the original trigger button that opened the focus trap content at the beginning

This is so much better! Not only are we testing in the same way that a user would interact with the UI, but we’re making assertions about those behaviors in our tests. This gives us much better confidence that things work properly, which is, after all, the whole purpose of having a test suite to begin with.

---

## Conclusion

Testing your application in the same way that a user interacts with your application is a better way to test. Your tests become more realistic, giving you more peace of mind that your app works properly for your users.

And, as an added bonus, look at the beautiful resulting code coverage!

![Test suite and code coverage for focus-trap-react](https://dev-to-uploads.s3.amazonaws.com/i/8lmhc0heupg6fgtxfaef.png)
<figcaption>Test suite and code coverage for focus-trap-react</figcaption>
