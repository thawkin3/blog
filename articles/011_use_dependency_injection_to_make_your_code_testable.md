# Use Dependency Injection to Make Your Code Testable

Have you ever wanted to write unit tests for your code, but you’ve found that it’s difficult to do so? Often this is the result of not writing code with testing in mind. An easy way to solve this is through utilizing [test-driven development](https://medium.com/swlh/a-case-for-test-driven-development-8e36df33dc21?source=friends_link&sk=b1756de0b41e2956ac1012faba43aa1e), a development process in which you write your tests *before* your app code.

But, even if you’re not a fan of test-driven development, you can still make your code easier to test by employing a simple technique, **dependency injection**, which we’ll discuss in this article.

---

## What is Dependency Injection?

Dependency injection is a pretty straightforward yet incredibly powerful technique. In short, rather than a function having its dependencies hard-coded into it, the function instead allows the developer using the function to pass it any needed dependencies through arguments.

To help solidify the concept, let’s look at an example together.

---

## Parsing a Cookie String

![Cookies on a tray](https://miro.medium.com/max/14062/0*0cSdUZyS5vAn_RbB)
<figcaption>Photo by John Dancy on Unsplash</figcaption>

Let’s say you want to write a JavaScript function that can parse individual cookie key-value pairs out of the `document.cookie` string.

For example, say you want to check if there is a cookie called `enable_cool_feature`, and if its value is `true`, then you want to enable some cool feature for that user browsing your site.

Unfortunately, the `document.cookie` string is absolutely terrible to work with in JavaScript. It’d be nice if we could just look up a property value with something like `document.cookie.enable_cool_feature`, but alas, we cannot.

So, we’ll resort to writing our own cookie-parsing function that will provide a simple facade over some potentially complicated underlying code.

(For the record, there are several JavaScript libraries and packages out there that have done exactly this, so don’t feel the need to re-write this function yourself in your own app unless you want to.)

---

As a first pass, we might want to have a simple function defined like this:

```js
function getCookie(cookieName) { /* body here */ }
```

This function would allow us to find a specific cookie’s value by calling it like this:

```js
getCookie('enable_cool_feature')
```

---

## A Sample Solution

![Idea lightbulb](https://miro.medium.com/max/7600/0*7pdT1bf8-mF3hMIc)
<figcaption>Photo by AbsolutVision on Unsplash</figcaption>

A Google search on “how to parse the cookie string in JavaScript” reveals many different solutions from various developers. For this article, we’ll look at the solution provided by [W3Schools](https://www.w3schools.com/js/js_cookies.asp). It looks like this:

```js
export function getCookie(cookieName) {
  var name = cookieName + '='
  var decodedCookie = decodeURIComponent(document.cookie)
  var ca = decodedCookie.split(';')

  for (var i = 0; i < ca.length; i++) {
    var c = ca[i]
    while (c.charAt(0) == ' ') {
      c = c.substring(1)
    }

    if (c.indexOf(name) == 0) {
      return c.substring(name.length, c.length)
    }
  }

  return ''
}
```

---

## Criticism of the Sample Solution

![Judging cat](https://miro.medium.com/max/7986/0*eT2HVGYPL7h3LRHC)
<figcaption>Photo by FuYong Hua on Unsplash</figcaption>

Now, what’s wrong with this? We won’t criticize the main body of the code itself, but rather we’ll look at this one line of code:

```js
var decodedCookie = decodeURIComponent(document.cookie)
```

The function `getCookie` has a dependency on the `document` object and on the `cookie` property! This may not seem like a big deal at first, but it does have some drawbacks.

---

First, what if for whatever reason our code didn’t have access to the `document` object? For instance, in the Node environment, the `document` is `undefined`. Let’s look at some sample test code to illustrate this.

Let’s use Jest as our testing framework and then write two tests:

```js
import { getCookie } from './get-cookie-bad'

describe('getCookie - Bad', () => {
  it('can correctly parse a cookie value for an existing cookie', () => {
    document.cookie = 'key2=value2'
    expect(getCookie('key2')).toEqual('value2')
  })

  it('can correctly parse a cookie value for a nonexistent cookie', () => {
    expect(getCookie('bad_key')).toEqual('')
  })
})
```

Now let’s run our tests to see the output.

```
ReferenceError: document is not defined
```

Oh no! In the Node environment, the `document` is not defined. Luckily, we can change our Jest config in our `jest.config.js` file to specify that our environment should be `jsdom`, and that will create a DOM for us to use in our tests.

```js
module.exports = {
  testEnvironment: 'jsdom'
}
```

Now if we run our tests again, they pass. But, we still have a bit of a problem. We’re modifying the `document.cookie` string globally, which means our tests are now interdependent. This can make for some odd test cases if our tests run in different orders.

For instance, if we were to write `console.log(document.cookie)` in our second test, it would still output `key2=value2` . Oh no! That’s not what we want. Our first test is affecting our second test. In this case, the second test still passes, but it’s very possible to get into some confusing situations when you have tests that are not isolated from one another.

To solve this, we could do a bit of cleanup after our first test’s `expect` statement:

```js
it('can correctly parse a cookie value for an existing cookie', () => {
  document.cookie = 'key2=value2'
  expect(getCookie('key2')).toEqual('value2')
  document.cookie = 'key2=; expires = Thu, 01 Jan 1970 00:00:00 GMT'
})
```

(Generally I’d advise you do some cleanup in an `afterEach` method, which runs the code inside it after each test. But, deleting cookies isn’t as simple as just saying `document.cookie = ''` unfortunately.)

---

A second problem with the W3Schools’ solution presents itself if you wanted to parse a cookie string not currently set in the `document.cookie` property. How would you even do that? In this case, you can’t!

---

## There is a Better Way

![Man jumping with joy](https://miro.medium.com/max/12000/0*g7E9ewBP2mgkFPo4)
<figcaption>Photo by Cam Adams on Unsplash</figcaption>

Now that we’ve explored one possible solution and two of its problems, let’s look at a better way to write this method. We’ll use dependency injection!

Our function signature will look a little different from our initial solution. This time, it will accept two arguments:

```js
function getCookie(cookieString, cookieName) { /* body here */ }
```

So we can call it like this:

```js
getCookie(<someCookieStringHere>, 'enable_cool_feature')
```

A sample implementation might look like this:

```js
export function getCookie(cookieString, cookieName) {
  var name = cookieName + '='
  var decodedCookie = decodeURIComponent(cookieString)
  var ca = decodedCookie.split(';')

  for (var i = 0; i < ca.length; i++) {
    var c = ca[i]
    while (c.charAt(0) == ' ') {
      c = c.substring(1)
    }

    if (c.indexOf(name) == 0) {
      return c.substring(name.length, c.length)
    }
  }

  return ''
}
```

Note that the only difference between this function and the original function is that the function now accepts two arguments, and it uses the argument for the `cookieString` when decoding the cookie on line 3.

---

Now let’s write two tests for this function. These two tests will test the same things that our original two tests did:

```js
import { getCookie } from './get-cookie-good'

describe('getCookie - Good', () => {
  it('can correctly parse a cookie value for an existing cookie', () => {
    const cookieString = 'key1=value1;key2=value2;key3=value3'
    const cookieName = 'key2'
    expect(getCookie(cookieString, cookieName)).toEqual('value2')
  })

  it('can correctly parse a cookie value for a nonexistent cookie', () => {
    const cookieString = 'key1=value1;key2=value2;key3=value3'
    const cookieName = 'bad_key'
    expect(getCookie(cookieString, cookieName)).toEqual('')
  })
})
```

Note how we can completely control the cookie string that our method uses now.

We don’t have to rely on the environment, we don’t run into any testing hangups, and we don’t have to assume that we’re always parsing a cookie directly from `document.cookie`.

Much better!

---

## Conclusion

That’s it! Dependency injection is incredibly simple to implement, and it will greatly improve your testing experience by making your tests easy to write and your dependencies easy to mock. (Not to mention it helps decouple your code, but that’s a topic for another day.)

Thanks for reading!
