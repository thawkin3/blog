# 3 React Mistakes Junior Developers Make With Component State

One of my favorite things about web development is that there's always something new to learn. You could spend your whole life mastering various programming languages, libraries, and frameworks and still not know it all.

Because we're all learning, it also means we're all prone to making errors as well. This is ok. The goal is to get better and to be better. If you make a mistake and learn from it, you're doing great! But if you fail to learn anything new and continue to make the same mistakes repeatedly, well... then it sounds like you may be stagnating in your career.

In that spirit, here are three common mistakes I often see during code reviews that junior developers make when dealing with React component state. We'll take a look at each mistake and then discuss how to fix it.

---

## 1. Modifying state directly

When changing a component's state, it's important that you return a new copy of the state with modifications, not modify the current state directly. If you incorrectly modify a component's state, React's diffing algorithm won't catch the change, and your component won't update properly. Let's look at an example.

Say that you have some state that looks like this:

```js
this.state = {
  colors: ['red', 'green', 'blue']
}
```

And now you want to add the color "yellow" to this array. It may be tempting to do this:

```js
this.state.colors.push('yellow')
```

Or even this:

```js
this.state.colors = [...this.state.colors, 'yellow']
```

But both of those approaches are incorrect! When updating state in a class component, you always need to use the `setState` method, and you should always be careful not to mutate objects. Here's the right way to add the element to the array:

```js
this.setState(prevState => ({ colors: [...prevState.colors, 'yellow'] }))
```

And this leads us right into mistake number two.

---

## 2. Setting state that relies on the previous state without using a function

There are two ways to use the `setState` method. The first way is to provide an object as an argument. The second way is to provide a function as an argument. So, when would you want to use one over the other?

If you were to have, for example, a button that can be enabled or disabled, you might have a piece of state called `isDisabled` which holds a boolean value. If you wanted to toggle the button from enabled to disabled, it might be tempting to write something like this, using an object as the argument:

```js
this.setState({ isDisabled: !this.state.isDisabled })
```

So, what's wrong with this? The problem lies in the fact that React state updates can be batched, meaning that multiple state updates can occur in a single update cycle. If your updates were to be batched, and you had multiple updates to the enabled/disabled state, the end result may not be what you expect.

A more correct way to update the state here would be to provide a function of the previous state as the argument:

```js
this.setState(prevState => ({ isDisabled: !prevState.isDisabled }))
```

Now, even if your state updates are batched and multiple updates to the enabled/disabled state are made together, each update will rely on the correct previous state so that you always end up with the result you'd expect.

The same is true for something like incrementing a counter.

Don't do this:

```js
this.setState({ counterValue: this.state.counterValue + 1 })
```

Do this:

```js
this.setState(prevState => ({ counterValue: prevState.counterValue + 1 }))
```

The key here is that if your new state relies on the value of the old state, you should always use a function as the argument. If you are setting a value that does not rely on the value of the old state, then you can use an object as the argument.

---

## 3. Forgetting that `setState` is asynchronous

Finally, it's important to remember that `setState` is an asynchronous method. As an example, let's imagine that we have a component with state that looks like this:

```js
this.state = { name: 'John' }
```

And then we have a method that updates the state and then logs the state to the console:

```js
this.setState({ name: 'Matt' })
console.log(this.state.name)
```

You may think that this would log `'Matt'` to the console, but it doesn't! It logs `'John'`!

The reason for this is that, again, `setState` is asynchronous. That means it's going to kick off the state update when it gets to the line that calls setState, but the code below it will continue to execute since asynchronous code is non-blocking.

If you have code that you need to run after the state is updated, React allows you to provide a callback function that gets run once the update is complete.

A correct way to log the current state after the update would be:

```js
this.setState({ name: 'Matt' }, () => console.log(this.state.name))
```

Much better! Now it correctly logs `'Matt'` as expected.

---

## Conclusion

There you have it! Three common mistakes and how to fix them. Remember, it's ok to make mistakes. You're learning. I'm learning. We're all learning. Let's continue to learn and get better together.

*(Bonus points if you understood the cover image's reference.)*

---

*Edit: I was frequently asked if the same principles I've outlined in this article also apply to function components and hooks. I decided to write a followup article that focuses on exactly that! You can find it here:*

*https://dev.to/thawkin3/3-mistakes-junior-developers-make-with-react-function-component-state-88a*
