# I Built an App That Uses All 7 New Features in JavaScript ES2020

The world of web development moves fast, especially in the JavaScript ecosystem. New features, frameworks, and libraries are constantly emerging, and the minute you stop learning is the minute your skill set starts to become obsolete.

One important part of keeping your JavaScript skills sharp is staying current on the latest features in JavaScript. So, I thought it would be fun to build an app that incorporates all seven of the new features in JavaScript ES2020.

---

I recently did a bit of bulk shopping at Costco to stock up on some food essentials. Like most stores, their price tags display the unit price for each item, so you can assess and compare the quality of each deal. Do you go with the small bag or the large bag? (Who am I kidding? It's Costco. Go large!) 

But what if the unit price wasn't displayed? 

In this article, I'll build a unit price calculator app using vanilla JavaScript for the front end and [Node.js](https://nodejs.org/en/) with [Express.js](https://expressjs.com/) for the back end. I'll deploy the app on [Heroku](https://www.heroku.com/), which is an easy place to [quickly deploy a node.js app](https://devcenter.heroku.com/articles/getting-started-with-nodejs).

---

## What's New in JavaScript ES2020?

The JavaScript programming language conforms to a specification known as ECMAScript. Starting with the release of ES2015 (or ES6), a new version of JavaScript has been released each year. As of right now, the latest version is ES2020 (ES11). ES2020 is packed with seven exciting new features that JavaScript developers have been waiting for quite some time to see. The new features are:

1. Promise.allSettled()
2. Optional Chaining
3. Nullish Coalescing
4. globalThis
5. Dynamic Imports
6. String.prototype.matchAll()
7. BigInt

You should note that not all browsers support these features — yet. If you want to start using these features now, make sure you provide appropriate polyfills or use a transpiler like Babel to ensure your code is compatible with older browsers.

---

## Getting Started

If you want to follow along with your own copy of the code, first create a Heroku account and install the Heroku CLI on your machine. See this [Heroku guide](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up) for installation instructions. 

Once you’ve done that, you can create and deploy the project easily using the CLI. All of the source code needed to run this example app is [available on GitHub](https://github.com/thawkin3/unit-price-calculator). 

Below are step-by-step instructions on how to clone the repo and deploy to Heroku:


```
git clone https://github.com/thawkin3/unit-price-calculator.git
cd unit-price-calculator 
heroku create
git push heroku master
heroku open
```

---

## System Overview

My unit price calculator app is fairly simple: it lets you compare various price and weight options for fictional products and then calculates the unit price. When the page loads, it fetches product data from the server by hitting two API endpoints. You can then choose your product, your preferred unit of measurement, and a price/weight combination. The unit price calculation is done once you hit the submit button.

![Unit Price Calculator App](https://dev-to-uploads.s3.amazonaws.com/i/ttfubgrqzdtts4mh30f3.png)
<figcaption>Unit Price Calculator App</figcaption>


Now that you've seen the app, let's take a look at how I used all seven of those ES2020 features. For each feature, I'll discuss exactly what it is, how it's useful, and how I used it.

---

## 1. Promise.allSettled()

When a user first visits the calculator app, three API requests are kicked off to fetch product data from the server. We wait for all three requests to finish by using `Promise.allSettled()`:

{% gist https://gist.github.com/thawkin3/4b53a7392009d205effe5d7af3b681a9 %}

`Promise.allSettled()` is a new feature that improves upon the existing `Promise.all()` functionality. Both of these methods allow you to provide an array of promises as an argument, and both methods return a promise.

The difference is that `Promise.all()` will short-circuit and reject itself early if any of the promises are rejected. On the other hand, `Promise.allSettled()` waits for all of the promises to be settled, regardless of whether they are resolved or rejected, and then resolves itself.

So if you want the results from all your promises, even if some of the promises are rejected, then start using `Promise.allSettled()`.

Let's look at another example with `Promise.all()`:

{% gist https://gist.github.com/thawkin3/7b0a2e2afce540b2358097bb288448c3 %}

And now let's look at another example with `Promise.allSettled()` to note the difference in behavior when a promise gets rejected:

{% gist https://gist.github.com/thawkin3/47d0f57dd115816cee4d1e87d24dc234 %}

---

## 2. Optional Chaining

Once the product data is fetched, we handle the response. The data coming back from the server contains an array of objects with deeply-nested properties. In order to safely access those properties, we use the new optional chaining operator:

{% gist https://gist.github.com/thawkin3/7e5455226c03ba9e4c9903ac2003fb34 %}

Optional chaining is the feature I'm most excited about in ES2020. The optional chaining operator -- `?.` -- allows you to safely access deeply-nested properties of an object without checking for the existence of each property.

For example, prior to ES2020, you might write code that looks like this in order to access the `street` property of some `user` object:

{% gist https://gist.github.com/thawkin3/f63144ad4ab85aee50f13bf4476a549e %}

In order to safely access the `street` property, you first must make sure that the `user` object exists and that the `address` property exists, and then you can try to access the `street` property.

With optional chaining, the code to access the nested property is much shorter:

{% gist https://gist.github.com/thawkin3/91e89ad6e1963a4f0bc4a40c2d19d03d %}

If at any point in your chain a value does not exist, `undefined` will be returned. Otherwise, the return value will be the value of the property you wanted to access, as expected.

---

## 3. Nullish Coalescing

When the app loads, we also fetch the user's preference for their unit of measurement: kilograms or pounds. The preference is stored in local storage, so the preference won't yet exist for first-time visitors. To handle either using the value from local storage or defaulting to using kilograms, we use the nullish coalescing operator:

{% gist https://gist.github.com/thawkin3/5a2fe55491391d70ab999a2370cbd749 %}

The nullish coalescing operator -- `??` -- is a handy operator for when you specifically want to use a variable's value as long as it is not `undefined` or `null`. You should use this operator rather than a simple OR -- `||` -- operator if the specified variable is a boolean and you want to use its value even when it's `false`.

For example, say you have a toggle for some feature setting. If the user has specifically set a value for that feature setting, then you want to respect his or her choice. If they haven't specified a setting, then you want to default to enabling that feature for their account.

Prior to ES2020, you might write something like this:

{% gist https://gist.github.com/thawkin3/07e450fa5e7bb7381369246fb52f647f %}

With the nullish coalescing operator, your code is much shorter and easier to understand:

{% gist https://gist.github.com/thawkin3/8318366aac732febe5e03b8cc460e1c5 %}

---

## 4. globalThis

As mentioned above, in order to get and set the user's preference for unit of measurement, we use local storage. For browsers, the local storage object is a property of the `window` object. While you can just call `localStorage` directly, you can also call it with `window.localStorage`. In ES2020, we can also access it through the `globalThis` object (also note the use of optional chaining again to do some feature detection to make sure the browser supports local storage):

{% gist https://gist.github.com/thawkin3/2506b79de84ff181d20caca291abba93 %}

The `globalThis` feature is pretty simple, but it solves many inconsistencies that can sometimes bite you. Simply put, `globalThis` contains a reference to the global object. In the browser, the global object is the `window` object. In a node environment, the global object is literally called `global`. Using `globalThis` ensures that you always have a valid reference to the global object no matter what environment your code is running in. That way, you can write portable JavaScript modules that will run correctly in the main thread of the browser, in a web worker, or in the node environment.

{% gist https://gist.github.com/thawkin3/b5779bcdc00864cc6d1e5402bef77807 %}

---

## 5. Dynamic Imports

Once the user has chosen a product, a unit of measurement, and a weight and price combination, he or she can click the submit button to find the unit price. When the button is clicked, a JavaScript module for calculating the unit price is lazy loaded. You can check the network request in the browser's dev tools to see that the second file isn't loaded until you click the button:

{% gist https://gist.github.com/thawkin3/b8da327897e875866122cf16669da336 %}

Prior to ES2020, using an `import` statement in your JavaScript meant that the imported file was automatically included inside the parent file when the parent file was requested.

Bundlers like [webpack](https://webpack.js.org/) have popularized the concept of "code splitting," which is a feature that allows you to split your JavaScript bundles into multiple files that can be loaded on demand. React has also implemented this feature with its `React.lazy()` method.

Code splitting is incredibly useful for single page applications (SPAs). You can split your code into separate bundles for each page, so only the code needed for the current view is downloaded. This significantly speeds up the initial page load time so that end users don't have to download the entire app upfront.

Code splitting is also helpful for large portions of rarely-used code. For example, say you have an "Export PDF" button on a page in your app. The PDF download code is large, and including it when the page loads reduces overall load time. However, not every user visiting this page needs or wants to export a PDF. To increase performance, you can make your PDF download code be lazy loaded so that the additional JavaScript bundle is only downloaded when the user clicks the "Export PDF" button.

In ES2020, dynamic imports are baked right into the JavaScript specification!

Let's look at an example setup for the "Export PDF" functionality without dynamic imports:

{% gist https://gist.github.com/thawkin3/90a9b6890b9c09b55fd425e94e7f6ff3 %}

And now let's look at how you could use a dynamic import to lazy load the large PDF download module:

{% gist https://gist.github.com/thawkin3/78534ce6be2125a3ad3e0b780bff21cc %}

---

## 6. String.prototype.matchAll()

When calling the `calculateUnitPrice` method, we pass the product name and the price/weight combination. The price/weight combination is a string that looks like "$200 for 10 kg". We need to parse that string to get the price, weight, and unit of measurement. (There's certainly a better way to architect this app to avoid parsing a string like this, but I'm setting it up this way for the sake of demonstrating this next feature.) To extract the necessary data, we can use `String.prototype.matchAll()`:

{% gist https://gist.github.com/thawkin3/585de3c3bd5e8c4cc38b2ed43d56588f %}

There's a lot going on in that one line of code. We look for matches in our string based on a regular expression that is searching for digits and the strings "lb" or "kg". It returns an iterator, which we can then spread into an array. This array ends up with three elements in it, one element for each match (200, 10, and "kg").

This feature is probably the most difficult to understand, particularly if you're not well-versed in regular expressions. The short and simple explanation of `String.prototype.matchAll()` is that it's an improvement on the functionality found in `String.prototype.match()` and `RegExp.prototype.exec()`. This new method allows you to match a string against a regular expression and returns an iterator of all the matching results, including capture groups.

Did you get all that? Let's look at another example to help solidify the concept:

{% gist https://gist.github.com/thawkin3/1bb2a4513bb6c98c6e78ddd863cad1d3 %}

---

## 7. BigInt

Finally, we'll make the unit price calculation by simply dividing the price by the weight. You can do this with normal numbers, but when working with large numbers, ES2020 introduces the `BigIn`t which allows you to do calculations on large integers without losing precision. In the case of our app, using `BigInt` is overkill, but who knows, maybe our API endpoint will change to include some crazy bulk deals!

{% gist https://gist.github.com/thawkin3/2210ca57341cd03c52f60db7d2ae47f6 %}

If you've ever worked with data that contains extremely large numbers, then you know what a pain it can be to ensure the integrity of your numeric data while performing JavaScript math operations. Prior to ES2020, the largest whole number you could safely store was represented by `Number.MAX_SAFE_INTEGER`, which is 2^53 - 1.

If you tried to store a number larger than that value in a variable, sometimes the number wouldn't be stored correctly:

{% gist https://gist.github.com/thawkin3/b75906dc581e5bdfe701685f1a0975d9 %}

The new `BigInt` data type helps solve this problem and allows you to work with much larger integers. To make an integer a `BigInt`, you simply append the letter `n` to the end of the integer or call the function `BigInt()` on your integer:

{% gist https://gist.github.com/thawkin3/afa508bd12ade35450966685a1eb890d %}

---

## Conclusion

That's it! Now that you know all about the new ES2020 features, what are you waiting for? Get out there and start writing new JavaScript today!
