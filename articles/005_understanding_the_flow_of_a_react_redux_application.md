# Understanding the Flow of a React + Redux Application

[React](https://reactjs.org/) is currently the most popular JavaScript library for building user interfaces, and [Redux](https://redux.js.org/) (used in conjunction with [React Redux](https://react-redux.js.org/)) is the most widely-used state management library for React apps.

Understanding how data flows in apps like these is crucial if you are a front-end engineer working with React.

Let’s walk through it together!

---

## The Core Pieces of a React + Redux Application

There are a few important concepts that make up a React + Redux app that are essential to understand:

### Redux store

The Redux store is an object that holds the state of your application. The store is the source of truth for data and is available to any component in your application that is hooked up to it through the `connect` method.

### Components

Components are the building blocks that make up the UI. Components can be as small as a button or an avatar, or they can be as large as a container or a page (or even the top-level component, which contains the whole app).

Components connected to the store are able to read the global state of the application and also trigger action creators, which we’ll cover next.

### Action creators

Action creators are functions that return a plain object called an `action`. Action creators are generally invoked when a user interacts with the UI (for example, when clicking a button) or at specific points in a component’s lifecycle (for example, when a component mounts).

By default, action creators are synchronous, but you can use Redux middleware like [Redux Thunk](https://github.com/reduxjs/redux-thunk) or [Redux Saga](https://redux-saga.js.org/) to handle asynchronous action creators as well. For now we’ll just focus on synchronous code.

### Actions

Actions, as mentioned above, are plain objects. Actions have a `type` property that is just a string constant that identifies the action.

Actions can contain any other data as well, so you could include a `payload` property or a `userId` property or whatever you’d like.

### Reducers

Reducers are pure functions that take a previous state and an action and then return an updated copy of the state.

---

## The Flow of a React + Redux Application

Now that you know the important pieces of a React + Redux app, a diagram can be helpful to visualize the flow of a React + Redux app.

![React+Redux application flow](https://miro.medium.com/max/1116/1*2Ga4iOJs58AbXBw9c43yjw.png)

Note that the flow here is *unidirectional*: it only goes in one direction. This is incredibly helpful in thinking through how your app works and when you need to do some troubleshooting to track down a pesky bug.

---

## Example Workflow

Let’s look at what a typical workflow might look like.

Let’s say that you have a very simple counter application. There is a button on the page that you can click to increment the counter, and the counter’s current value is displayed on the page as well.

The workflow looks like this:

1. The counter value is held in the *store*.
2. The button *component* is connected to the *store* so that when the user clicks the button, the `onClick` handler can trigger an *action creator*, which is a simple function that we’ll name `incrementCounter`.
3. This `incrementCounter` *action creator* then returns an *action*, which is a plain object that looks like: `{ type: INCREMENT_COUNTER }`.
4. The *reducer* then handles that action. The reducer knows that when it receives an action with the type `INCREMENT_COUNTER`, it needs to increase the value of the `counter` property in the state by one.
5. The state in the *store* is then updated, and the counter’s value goes from `0` to `1`.
6. The counter display in the UI is connected to the *store*, so when the state changes, the UI updates to reflect those changes. So, the user now sees the value `1` on the screen.

If the user were to click the button again, this whole process would repeat, incrementing the counter value to `2`.

---

## Demo Code

An example of a simple React + Redux app can be found in this [GitHub repo](https://github.com/thawkin3/redux-example). You can also view the [live demo here](http://tylerhawkins.info/redux-example/build/).

Thanks for reading!
