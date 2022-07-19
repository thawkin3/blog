# Callbacks, Promises, and Async/Await in JavaScript

JavaScript is single-threaded, which means that only one thing can happen at a time. **Synchronous code** is executed from top to bottom in the order that the code is written. Synchronous code is also "blocking" –– each line of code waits for the previous line of code to be executed before it runs.

In contrast, **asynchronous code** is "non-blocking" code that allows long-running requests to not block the main JavaScript thread. When the request is finished, additional code can then be executed. This is generally done in one of three ways:

1. Callbacks
2. Promises
3. Async/await

Let's look at a few examples to see how we can write asynchronous code using these three approaches.

---

## Callbacks

A callback function is a function that you pass to an asynchronous function as an argument. The callback function is executed once the asynchronous part of the work is done.

Let's simulate waiting for an API request to return a response by using the `setTimeout` method. A callback approach might look like this:

```js
function myAsyncMethod(callback) {
  console.log('myAsyncMethod was executed')
  setTimeout(callback, 1000)
}

function myCallbackMethod() {
  console.log('myCallbackMethod was executed')
}

myAsyncMethod(myCallbackMethod)
```

This code will first log to the console the text "myAsyncMethod was executed." It will then wait one second before it logs to the console the text "myCallbackMethod was executed."

---

## Promises

Promises are another way to write asynchronous code that help you avoid deeply nested callback functions, also known as "callback hell." A promise can be in one of three states: pending, resolved, or rejected. Once a promise is resolved, you can handle the response using the `promise.then()` method. If a promise is rejected, you can handle the error using the `promise.catch()` method.

We can re-write our previous example using promises like this:

```js
function myAsyncMethod() {
  console.log('myAsyncMethod was executed')
  
  return new Promise((resolve, reject) => {
    setTimeout(resolve, 1000) 
  }) 
}

function myPromiseThenMethod() {
  console.log('myPromiseThenMethod was executed')
}

myAsyncMethod().then(myPromiseThenMethod)
```

Just as before, this code will first log to the console the text "myAsyncMethod was executed." It will then wait one second before it logs to the console the text "myPromiseThenMethod was executed."

---

## Async/await

Async/await is a new syntax that was introduced in ES2017. It allows you to write asynchronous code in a way that looks synchronous, even though it's not. This makes the code easier to understand.

Let's re-write our example again, this time using async/await:

```js
function myAsyncMethod() {
  console.log('myAsyncMethod was executed')
  
  return new Promise((resolve, reject) => {
    setTimeout(resolve, 1000) 
  })
}

function myAwaitMethod() {
  console.log('myAwaitMethod was executed')
}

async function init() {
  await myAsyncMethod()
  myAwaitMethod()
}

init()
```

Once again, this code will first log to the console the text "myAsyncMethod was executed." It will then wait one second before it logs to the console the text "myAwaitMethod was executed."

Note how we defined the `init` function using the `async` keyword. We then used the `await` keyword before our call to the `myAsyncMethod` function to tell our code that we don't want to run the next line of code calling `myAwaitMethod` until *after* `myAsyncMethod` has finished running.

Now we have synchronous-looking code that actually runs asynchronously! Async/await gives us the best of both worlds: non-blocking code that is still easy to read and reason about.
