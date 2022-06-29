# React Clean Code - Simple ways to write better and cleaner code

Clean code is more than just working code. Clean code is easy to read, simple to understand, and neatly organized. In this article we’ll look at eight ways we can write cleaner React code.

In going through these suggestions, it’s important to remember that that’s exactly what these are: suggestions. If you disagree with any of them, that’s completely fine. However, these are practices that I’ve found helpful in writing my own React code. Let’s dive in!

---

## 1. Conditional rendering only for one condition

If you need to conditionally render something when a condition is `true` and not render anything when a condition is `false`, don’t use a ternary operator. Use the `&&` operator instead.

Bad example:

{% gist https://gist.github.com/thawkin3/92611f9bb8ca5144ec4abfc44e570a96 %}

Good example:

{% gist https://gist.github.com/thawkin3/c4f65efa27b6fa63c00c48a6c46ff43f %}

---

## 2. Conditional rendering on either condition

If you need to conditionally render one thing when a condition is `true` and render a different thing when the condition is `false`, use a ternary operator.

Bad example:

{% gist https://gist.github.com/thawkin3/bd9adc0ba4f4d5c50410982cecffe1d5 %}

Good example:

{% gist https://gist.github.com/thawkin3/af3b5d2f222fd49cfc27945352e6e559 %}

---

## 3. Boolean props

A truthy prop can be provided to a component with just the prop name without a value like this: `myTruthyProp`. Writing it like `myTruthyProp={true}` is unnecessary.

Bad example:

{% gist https://gist.github.com/thawkin3/d7e61939998ec48635688f4cad309a87 %}

Good example:

{% gist https://gist.github.com/thawkin3/23bfe751607f8e46f1d8634bb6c35cfc %}

---

## 4. String props

A string prop value can be provided in double quotes without the use of curly braces or backticks.

Bad example:

{% gist https://gist.github.com/thawkin3/398035c9759321691f3baa3e65753a7a %}

Good example:

{% gist https://gist.github.com/thawkin3/4e38874197fce8a1b48f1e5b10e64853 %}

---

## 5. Event handler functions

If an event handler only takes a single argument for the `Event` object, you can just provide the function as the event handler like this: `onChange={handleChange}`. You don't need to wrap the function in an anonymous function like this: `onChange={e => handleChange(e)}`.

Bad example:

{% gist https://gist.github.com/thawkin3/83418a680e9b16e29931d0a544871e7a %}

Good example:

{% gist https://gist.github.com/thawkin3/c9b7d8fb91a73b1fc66546eea0b4f650 %}

---

## 6. Passing components as props

When passing a component as a prop to another component, you don’t need to wrap this passed component in a function if the component does not accept any props.

Bad example:

{% gist https://gist.github.com/thawkin3/b8001d07e7ae0954b7615db2bba38b0f %}

Good example:

{% gist https://gist.github.com/thawkin3/392a296cc67b2f6ed17f12e8104110d2 %}

---

## 7. Undefined props

Undefined props are excluded, so don’t worry about providing an `undefined` fallback if it's ok for the prop to be undefined.

Bad example:

{% gist https://gist.github.com/thawkin3/9f433a9a55982ba8118bc4c0428f910e %}

Good example:

{% gist https://gist.github.com/thawkin3/4c8044e583b555d095a152220f0f3211 %}

---

## 8. Setting state that relies on the previous state

Always set state as a function of the previous state if the new state relies on the previous state. React state updates can be batched, and not writing your updates this way can lead to unexpected results.

Bad example:

{% gist https://gist.github.com/thawkin3/5792a384ca7de3ff0b57d82f8fce2af2 %}

Good example:

{% gist https://gist.github.com/thawkin3/aa1d931a35968069a2ebad52fcde66de %}

---

## Honorable Mention

The following practices are not React-specific but rather are good practices for writing clean code in JavaScript (and any programming language, for that matter).

* Extract complex logic into clearly-named functions
* Extract magic numbers into constants
* Use clearly named variables

Happy coding!
