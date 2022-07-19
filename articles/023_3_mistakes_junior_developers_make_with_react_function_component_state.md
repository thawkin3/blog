# 3 Mistakes Junior Developers Make with React Function Component State

A few weeks ago I wrote an article all about mistakes that developers sometimes make when working with React component state. All of the examples I provided were using class components and the `setState` method.

{% link https://dev.to/thawkin3/3-react-mistakes-junior-developers-make-with-component-state-1bhd %}

I was asked several times if these same principles applied to function components and hooks. The answer is yes!

By popular demand, in this article we'll explore those same concepts, but this time with function components using the `useState` hook. We'll look at three common mistakes and how to fix them.

---

## 1. Modifying state directly

When changing a component's state, it's important that you return a new copy of the state with modifications, not modify the current state directly. If you incorrectly modify a component's state, React's diffing algorithm won't catch the change and your component won't update properly.

Let's look at an example. Say that you have some state that looks like this:

```js
const initialState = ['red', 'blue', 'green']
let [colors] = useState(initialState)
```

And now you want to add the color "yellow" to this array. It may be tempting to do this:

```js
colors.push('yellow')
```

Or even this:

```js
colors = [...colors, 'yellow']
```

But both of those approaches are incorrect! When updating state in a function component, you always need to use the setter method provided by the `useState` hook, and you should always be careful not to mutate objects. The setter method is the second element in the array that `useState` returns, so you can destructure it just like you do for the state value.

Here's the right way to add the element to the array:

```js
// Initial setup
const initialState = ['red', 'blue', 'green']
const [colors, setColors] = useState(initialState)

// Later, modifying the state
setColors(colors => [...colors, 'yellow'])
```

And this leads us right into mistake number two.

---

## 2. Setting state that relies on the previous state without using a function

There are two ways to use the setter method returned by the `useState` hook. The first way is to provide a new value as an argument. The second way is to provide a function as an argument. So, when would you want to use one over the other?

If you were to have, for example, a button that can be enabled or disabled, you might have a piece of state called `isDisabled` that holds a boolean value. If you wanted to toggle the button from enabled to disabled, it might be tempting to write something like this, using a value as the argument:

```js
// Initial setup
const [isDisabled, setIsDisabled] = useState(false)

// Later, modifying the state
setIsDisabled(!isDisabled)
```

So, what's wrong with this? The problem lies in the fact that React state updates can be batched, meaning that multiple state updates can occur in a single update cycle. If your updates were to be batched and you had multiple updates to the enabled/disabled state, the end result may not be what you expect.

A better way to update the state here would be to provide a function of the previous state as the argument:

```js
// Initial setup
const [isDisabled, setIsDisabled] = useState(false)

// Later, modifying the state
setIsDisabled(isDisabled => !isDisabled)
```

Now, even if your state updates are batched and multiple updates to the enabled/disabled state are made together, each update will rely on the correct previous state so that you always end up with the result you expect.

The same is true for something like incrementing a counter.

Don't do this:

```js
// Initial setup
const [counterValue, setCounterValue] = useState(0)

// Later, modifying the state
setCounterValue(counterValue + 1)
```

Do this:

```js
// Initial setup
const [counterValue, setCounterValue] = useState(0)

// Later, modifying the state
setCounterValue(counterValue => counterValue + 1)
```

The key here is that if your new state relies on the value of the old state, you should always use a function as the argument. If you are setting a value that does not rely on the value of the old state, then you can use a value as the argument.

---

## 3. Forgetting that the setter method from `useState` is asynchronous

Finally, it's important to remember that the setter method returned by the `useState` hook is an asynchronous method. As an example, let's imagine that we have a component with a state that looks like this:

```js
const [name, setName] = useState('John')
```

And then we have a method that updates the state and then logs the state to the console:

```js
const setNameToMatt = () => {
  setName('Matt')
  console.log(`The name is now... ${name}!`)
}
```

You may think that this would log `'Matt'` to the console, but it doesn't! It logs `'John'`!

The reason for this is that, again, the setter method returned by the `useState` hook is asynchronous. That means it's going to kick off the state update when it gets to the line that calls `setName`, but the code below it will continue to execute since asynchronous code is non-blocking.

If you have code that you need to run after the state is updated, React provides the `useEffect` hook, which allows you to write code that gets run after any of the dependencies specified are updated.

(This is a bit different from the way you'd do it with a callback function provided to the `setState` method in a class component. For whatever reason, the `useState` hook does not support that same API, so callback functions don't work here.)

A correct way to log the current state after the update would be:

```js
useEffect(() => {
  if (name !== 'John') {
    console.log(`The name is now... ${name}!`)
  }
}, [name])

const setNameToMatt = () => setName('Matt')
```

Much better! Now it correctly logs `'Matt'` as expected.

(Note that in this case I've added the `if` statement here to prevent the console log from happening when the component first mounts. If you want a more general solution, the recommendation is to [use the useRef hook](https://stackoverflow.com/questions/53253940/make-react-useeffect-hook-not-run-on-initial-render) to hold a value that updates after the component mounts, and this will successfully prevent your `useEffect` hooks from running when the component first mounts.)

---

## Conclusion

There you have it! Three common mistakes and how to fix them. Remember, it's OK to make mistakes. You're learning. I'm learning. We're all learning. Let's continue to learn and get better together.

If you'd like to check out some live demos for the examples used here (and more), visit [http://tylerhawkins.info/react-component-state-demo/build/](http://tylerhawkins.info/react-component-state-demo/build/).

You can also [find the code on GitHub](https://github.com/thawkin3/react-component-state-demo).
