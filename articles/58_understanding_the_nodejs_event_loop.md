# Understanding the Node.js Event Loop

JavaScript is single-threaded, so how does it handle asynchronous code without blocking the main thread while it waits for an action to complete? The key to understanding the asynchronous nature of JavaScript is understanding the event loop.

In the browser, the [event loop](https://www.educative.io/edpresso/what-is-an-event-loop-in-javascript) coordinates the execution of code between the call stack, web APIs, and the callback queue. Node.js, however, implements its own “[Node.js event loop](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/),” which is different from the regular “JavaScript event loop.” How confusing!

The Node.js event loop follows many of the same patterns as the JavaScript event loop but works slightly differently, as it doesn’t interact with the DOM but does deal with things like input and output (I/O).

In this article, we’ll dive into the theory behind the Node.js event loop and then look at a few examples using `setTimeout`, `setImmediate`, and `process.nextTick`. We'll even deploy some working code to [Heroku](https://www.heroku.com) (an easy way to quickly deploy apps) to see it all in action.

---

## The Node.js Event Loop

The Node.js event loop coordinates the execution of operations from timers, callbacks, and I/O events. This is how Node.js handles asynchronous behavior while still being single-threaded. Let’s look at a diagram of the event loop below to get a better understanding of the order of operations:

![The Node.js event loop’s order of operations (Source: Node.js docs)](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/79kfp84x5v333be5ub1s.png)
<figcaption>The Node.js event loop’s order of operations (Source: Node.js docs)</figcaption>

As you can see, there are [six main phases](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/#phases-overview) in the Node.js event loop. Let’s briefly look at what happens in each phase:

* **Timers**: callbacks scheduled by `setTimeout` and `setInterval` are executed during this phase

* **Pending callbacks**: I/O callbacks that were previously deferred to the next loop iteration are executed during this phase

* **Idle, prepare**: this phase is only used internally by Node.js

* **Poll**: new I/O events are retrieved and I/O callbacks are executed during this phase (except for callbacks scheduled by timers, callbacks scheduled by `setImmediate`, and close callbacks, because those are all handled in different phases)

* **Check**: callbacks scheduled by `setImmediate` are executed during this phase

* **Close callbacks**: close callbacks, like when a socket connection is destroyed, are executed during this phase

It’s interesting to note that `process.nextTick` isn't mentioned anywhere in any of these phases. That's because it's a special method that's not technically part of the Node.js event loop. Instead, whenever the `process.nextTick` method is called, it places its callbacks into a queue, and those queued callbacks are then "processed after the current operation is completed, regardless of the current phase of the event loop" (Source: [Node.js event loop docs](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/#process-nexttick)).

---

## Event Loop Example Scenarios

Now, if you’re like me, those explanations of each phase of the Node.js event loop may still seem a little abstract. I learn by seeing and by doing, so [I created this demo app on Heroku](https://nodejs-event-loop-demo.herokuapp.com/) for running various code snippet examples. In the app, clicking on any of the example buttons sends an API request to the server. The code snippet for the selected example is then executed by Node.js on the backend, and the response is returned to the frontend via the API. You can [view the full code on GitHub](https://github.com/thawkin3/nodejs-event-loop-demo).

![Node.js event loop demo app](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/c83daa1sa98t78l4emho.png)
<figcaption>Node.js event loop demo app</figcaption>

Let’s look at some examples to better understand the order of operations in the Node.js event loop.

---

### Example 1

We’ll start with an easy one:

![Example 1 — synchronous code](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vw0e9sub3h1z5n8qs18b.png)
<figcaption>Example 1 — synchronous code</figcaption>

Here we have three synchronous functions called one after the other. Because these functions are all synchronous, the code is simply executed from top to bottom. So because we call our functions in the order `first`, `second`, `third`, the functions are executed in the same order: `first`, `second`, `third`.

---

### Example 2

Next, we’ll introduce the concept of `setTimeout` with our second example:

![Example 2 — setTimeout](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zajzeasm7lt984hruv5s.png)
<figcaption>Example 2 — setTimeout</figcaption>

Here we call our `first` function, then schedule our `second` function using `setTimeout` with a delay of 0 milliseconds, then call our `third` function. The functions are executed in this order: `first`, `third`, `second`. Why is that? Why is the `second` function executed last?

There are a couple key principles to understand here. The first principle is that using the `setTimeout` method and providing a delay value *doesn't* mean that the callback function will be executed *exactly after* that number of milliseconds. Rather, that value represents the *minimum* amount of time that needs to elapse before the callback will be executed.

The second key principle to understand is that using `setTimeout` schedules the callback to be executed at a later time, which will always be at least during the next iteration of the event loop. So during this first iteration of the event loop, the `first` function was executed, the `second` function was scheduled, and the `third` function was executed. Then, during the second iteration of the event loop, the minimum delay of 0 milliseconds had been reached, so the `second` function was executed during the “timers” phase of this second iteration.

---

### Example 3

Next up, we’ll introduce the concept of `setImmediate` with our third example:

![Example 3 — setImmediate vs. setTimeout](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/i7qda0rh2jtpvheiw4v9.png)
<figcaption>Example 3 — setImmediate vs. setTimeout</figcaption>

In this example, we execute our `first` function, schedule our `second` function using `setTimeout` with a delay of 0 milliseconds, and then schedule our `third` function using `setImmediate`. This example begs the question: Which type of scheduling takes precedence in this scenario? `setTimeout` or `setImmediate`?

We’ve already discussed how `setTimeout` works, so we should give a brief background on the `setImmediate` method. The `setImmediate` method executes its callback function during the "check" phase of the next iteration of the event loop. So if `setImmediate` is called during the first iteration of the event loop, its callback method will be scheduled and then will be executed during the second iteration of the event loop.

As you can see from the output, the functions in this example are executed in this order: `first`, `third`, `second`. So in our case, the callback scheduled by `setImmediate` was executed before the callback scheduled by `setTimeout`.

It’s interesting to note that the behavior you see with `setImmediate` and `setTimeout` may vary depending on the context in which these methods are called. When these methods are called directly from the main module in a Node.js script, [the timing depends on the performance of the process](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/#setimmediate-vs-settimeout), so the callbacks could actually be executed in either order each time you run the script. However, when these methods are called within an I/O cycle, the `setImmediate` callback is always invoked before the `setTimeout` callback. Since we are invoking these methods as part of a response in an API endpoint in our example, our `setImmediate` callback always gets executed before our `setTimeout` callback.

---

### Example 4

As a quick sanity check, let’s run one more example using `setImmediate` and `setTimeout`.

![Example 4 — setImmediate versus setTimeout again](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7co9fwxnzkes4h72pdrj.png)
<figcaption>Example 4 — setImmediate versus setTimeout again</figcaption>

In this example, we schedule our `first` function using `setImmediate`, execute our `second` function, and then schedule our `third` function using `setTimeout` with a delay of 0 milliseconds. As you might have guessed, the functions are executed in this order: `second`, `first`, `third`. This is because the `first` function is scheduled, the `second` function is immediately executed, and then the `third` function is scheduled. During the second iteration of the event loop, the `second` function is executed since it was scheduled by `setImmediate` and we're in an I/O cycle, and then the `third` function is executed now that we're in the second iteration of the event loop and the specified delay of 0 milliseconds has passed.

Are you starting to get the hang of it?

---

### Example 5

Let’s look at one last example. This time we’ll introduce another method called `process.nextTick`.

![Example 5 — process.nextTick](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yyp3hzgf2drub9twbcir.png)
<figcaption>Example 5 — process.nextTick</figcaption>

In this example, we schedule our `first` function using `setImmediate`, schedule our `second` function using `process.nextTick`, schedule our `third` function using `setTimeout` with a delay of 0 milliseconds, and then execute our `fourth` function. The functions end up being called in the following order: `fourth`, `second`, `first`, `third`.

The fact that the `fourth` function was executed first shouldn't be a surprise. This function was called directly without being scheduled by any of our other methods. The `second` function was executed second. This is the one that was scheduled with `process.nextTick`. The `first` function was executed third, followed by the `third` function last, which shouldn't be a surprise to us either since we already know that callbacks scheduled by `setImmediate` get executed before callbacks scheduled by `setTimeout` when inside an I/O cycle.

So why did the `second` function scheduled by `process.nextTick` get executed before the `first` function scheduled by `setImmediate`? The method names are misleading here! You would think that a callback from `setImmediate` would get executed *immediately* while a callback from `process.nextTick` would get executed on the *next tick* of the event loop. However, [it's actually the other way around](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/#process-nexttick-vs-setimmediate). Confusing, right?

It turns out that a callback from `process.nextTick` gets executed immediately during *the same phase* as it was scheduled. A callback from `setImmediate` gets executed during the next iteration or tick of the event loop. So in our example, it makes sense that the `second` function scheduled by `process.nextTick` was executed before the `first` function scheduled by `setImmediate`.

---

## Conclusion

By now you should be a little more familiar with the Node.js event loop as well as with methods like `setTimeout`, `setImmediate`, and `process.nextTick`. You can certainly get by without digging into the internals of Node.js and the order of operations in which commands are processed. However, when you begin to understand the Node.js event loop, Node.js becomes a little less of a black box.

If you want to see these examples live in action again, you can always [check out the demo app](https://nodejs-event-loop-demo.herokuapp.com/) or [view the code on GitHub](https://github.com/thawkin3/nodejs-event-loop-demo). You can even [deploy the code to Heroku yourself by clicking here](https://heroku.com/deploy?template=https://github.com/thawkin3/nodejs-event-loop-demo).

Thanks for reading!
