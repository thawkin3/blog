# How to Unit Test HTML and Vanilla JavaScript Without a UI Framework

Recently I was curious about something: **Is it possible to write unit tests for front end code that doesn’t use any sort of UI framework or developer tooling?**

In other words, no React, Angular, or Vue. No webpack or rollup. No build tools of any kind. Just a plain old `index.html` file and some vanilla JavaScript.

Could a setup like this be tested?

This article and its [accompanying GitHub repo](https://github.com/thawkin3/dom-testing-demo) are the result of that question.

---

## Prior Experience

In my professional life, I’ve done a good bit of testing. I’m primarily a front end software engineer, so my areas of expertise include writing unit tests with Jest as my test framework and either [Enzyme](https://enzymejs.github.io/enzyme/) or [React Testing Library](https://testing-library.com/docs/react-testing-library/intro) as my test library when working with React. I’ve also done end-to-end testing using [Cypress](https://www.cypress.io/) or [Selenium](https://www.selenium.dev/).

Generally I choose to build user interfaces with React. When testing these interfaces, I started out using Enzyme years ago, but I’ve since come to favor React Testing Library and the philosophy that you should test your app in the same way that users use your app rather than test implementation details.

Kent C. Dodds’ React Testing Library is built on top of his [DOM Testing Library](https://testing-library.com/docs/dom-testing-library/intro), which, as its name implies, is a library that helps you test the DOM. I thought this might be a good starting point.

---

## Initial Research

![Research](https://miro.medium.com/max/1400/0*fxFGfgxMOKSWMDj8)
<figcaption>Photo by Michael Longmire on Unsplash</figcaption>

It’s very rare in the world of software engineering that you are the first person to attempt something. Nearly everything has been done before in one form or another. For this reason, Google, Stack Overflow, and developer forums are your friend.

I thought that surely someone else has tried this before and has written about it. After doing some research though, it seemed that a few people had tried this in the past but had hit a dead end. One developer [asked back in August of 2019](https://spectrum.chat/testing-library/help-dom/test-plain-html-vanilla-js~9f56a169-ea3f-481b-b1cc-dd9fc70dbeaf) for help but received no replies. Another developer wrote a [helpful article](https://dev.to/snowleo208/things-i-learned-after-writing-tests-for-js-and-html-page-4lja) on what they came up with, but unfortunately they ended up testing implementation details, which is something I wanted to avoid.

So, with the information I was able to gain from their attempts, I got started making my own demo project.

---

## Demo App

As noted above, you can find the [code for my demo app here](https://github.com/thawkin3/dom-testing-demo). You can also view the [app in action hosted here](http://tylerhawkins.info/dom-testing-demo/src/index.html). It’s small and simple since this is, after all, just a proof of concept.

Demo apps don’t need to be boring though, so I’ve created a pun generator for your entertainment. Here’s what it looks like:

![Demo app](https://miro.medium.com/max/1400/1*DgnFEqdAzeTW1jwpTH3gqQ.png)
<figcaption>Demo app</figcaption>

When viewing the source code, there are two important files to be aware of:

- `src/index.html`: This is the entire app. No other files, just one HTML file with a script tag in it.
- `src/index.test.js`: This is the test file. I’m using Jest and DOM Testing Library.

Both files are small, so I’ve included them below:

---

### Source File: `index.html`

{% gist https://gist.github.com/thawkin3/dbb4c2c3f2dac405c5679378c5edfe12 %}

---

### Test File: `index.test.js`

{% gist https://gist.github.com/thawkin3/f0841c6fd63e7a211f4cbe447a5397e1 %}

---

## Overview of the Source File

As you can see in the `index.html` file, there’s nothing special about it. If you were learning how to create a simple web page for the first time, your result would most likely look pretty similar to this with some basic HTML, CSS, and JavaScript. For simplicity, I’ve included the CSS and JavaScript inline in the file rather than linking to additional source files.

The JavaScript creates an array of puns, adds a click event listener to the button, and then inserts a new pun on the screen each time the button is clicked. Easy enough, right?

---

## Diving into the Test File

Since this is an article about testing, the test file is the key here. Let’s look at some of the more interesting snippets together.

---

### Retrieving the HTML File

The first question I had was how to import the HTML file into the test file. If you were testing a JavaScript file, generally you’d import the exported methods from the file you wanted to test like this:

```js
import { methodA, methodB } from './my-source-file'
```

However, that approach doesn’t work with an HTML file in my case. Instead, I used the built-in `fs` Node module to read the HTML file and store it in a variable:

```js
const html = fs.readFileSync(path.resolve(__dirname, './index.html'), 'utf8');
```

---

### Creating the DOM

Now that I had a string containing the HTML contents of the file, I needed to render it somehow. By default, Jest uses [jsdom](https://github.com/jsdom/jsdom) to emulate a browser when running tests. If you need to configure jsdom, you can also explicitly import it in your test file, which is what I did:

```js
import { JSDOM } from 'jsdom'
```

Then, in my `beforeEach` method, I used jsdom to render my HTML so that I could test against it:

```js
let dom
let container

beforeEach(() => {
  dom = new JSDOM(html, { runScripts: 'dangerously' })
  container = dom.window.document.body
})
```

---

### Running Scripts Inside the jsdom Environment

The most crucial piece to getting this working properly is contained in the configuration options passed to jsdom:

```js
{ runScripts: 'dangerously' }
```

Because I’ve told jsdom to run the scripts dangerously, it will actually interpret and execute the code contained in my `index.html` file’s `script` tag. Without this option enabled, the JavaScript is never executed, so testing the button click events wouldn’t work.

![I also like to live dangerously](https://miro.medium.com/max/960/0*aoDPElJkFCslYInK.gif)

**Disclaimer**: It’s important to note that you should [never run untrusted scripts](https://github.com/jsdom/jsdom/#executing-scripts) here. Since I control the HTML file and the JavaScript inside it, I can consider this safe, but if this script were to be from a third-party or if it included user input, it would not be wise to take this approach to configuring jsdom.

---

## Moment of Truth

Now, after completing the setup described above, when I ran `yarn test`, it… worked! The proof of concept was a great success, and there was much rejoicing.

![Passing tests](https://miro.medium.com/max/1400/1*uSsUMQJGmj6jaEtVkVLylQ.png)
<figcaption>All tests pass</figcaption>

---

## Conclusion

So, back to the initial question: Is it possible to write unit tests for front end code that doesn’t use any sort of UI framework or developer tooling?

The answer: **Yes!**

While my demo app certainly does not reflect what a production-ready app would look like, testing user interfaces this way if needed does seem like a viable option.

Thanks for reading!
