# An Introduction to Test-Driven Development

Test-driven development (TDD) is all the rage these days and has been a discussion topic for quite some time. If you are brand new to TDD, this article should serve as a good introduction to what TDD is, why it’s useful, what a typical TDD workflow looks like, and when to use TDD.

We’ll even go through an example in which we build a caesar cipher in JavaScript with Jest to run our unit tests.

Let’s get started.

---

## What is TDD?

What is test-driven development? In short, it’s a development strategy in which you write your tests first, then your app code.

In other words, product requirements are turned into very specific test cases, and then the software is improved so that the tests pass.

This is the opposite of a workflow in which you write your app code first and then your tests second. To be clear, either of these approaches are far superior to not having any unit tests at all.

---

## The “Red, Green, Refactor” Cycle

TDD can be more easily understood by looking at a simple diagram:

![The “red, green, refactor” cycle](https://dev-to-uploads.s3.amazonaws.com/i/ipbyvp0g0w7jk14yhodi.jpeg)
<figcaption>The “red, green, refactor” cycle, by Nat Pryce</figcaption>

1. **Write a failing test.** You’re writing your tests before your app code, so you know this test is going to fail. You haven’t implemented the feature yet! This is considered the “red” state (some tests failing).
2. **Make the test pass.** Here you write your app code, so you actually implement whatever functionality the test is looking at. Once you have your test passing, you’re now in the “green” state (all tests passing).
3. **Refactor.** Now that your test is passing, go make your code better. As the famous quote from Kent Beck goes: “Make it work. Make it right. Make it better. In that order.” Note that we don’t refactor until we’re in the “green” state. By having all our tests passing, we can refactor with confidence because if we break something, tests will start failing again, letting us know our refactor isn’t working perfectly yet.
4. **Repeat.** Go write another test now! Continue to loop through steps 1–3 as you write more tests and implement more functionality in your app.

---

## Why use TDD?

Ok, so that’s what TDD is and how the development process works. But why should you use it? There are several benefits to using TDD, but here are three:

1. Tests are built right into your development cycle, not as an afterthought.
2. It makes sure your tests are testing the right things.
3. Every branch is covered (in theory).

Let’s explore each of these benefits.

---

### Tests are built right into your development cycle, not as an afterthought.

Have you ever written all your app code, and then gone back and thought, “As a responsible developer I should probably write some tests for this code” and then dreaded the next few hours or days of writing tests?

Or has writing the tests ever felt pointless? After all, you’ve already manually tested your code, and it seems to be working fine, so why bother writing tests now? Let’s just ship it.

When you use TDD, however, tests are just part of your development process. Because you are writing them at the same time that you’re writing your app code, the tests feel more meaningful, and you know that they are actually serving some purpose.

(Side note: Tests are more for the future than for the present. While they’re helpful in verifying your functionality is working now, they’re even more useful when the next developer three months from now has to modify some of your code. If tests are in place, he or she can refactor with confidence. It’s especially nice when that developer is you!)

---

### It makes sure your tests are testing the right things.

If your tests are being written directly based on product requirements, then hopefully your tests are testing the core functionality that the end users care about. Simple enough.

By writing tests based on product requirements, you are also documenting the acceptance criteria for your app, in a way. Remember to avoid testing internal implementation details as much as possible!

---

### Every branch is covered (in theory).

If you’re writing your tests after your app code, do you ever find yourself struggling to hit those last few percentages of code coverage?

There are debates on how much code coverage is enough (80%? 90%? 95%? 100%?), but in general I feel like your code should have close to 100% code coverage. It’s nice to know that there are no hidden corners of your application that are lacking tests, especially if those spots are crucial parts of your application.

In practice, 100% code coverage probably isn’t always realistic or worth your time, but it’s a good ideal to strive for.

A nice side effect of using TDD is that it *should* result in 100% code coverage. If all the app code you're writing is to satisfy tests, then in theory you shouldn’t have written any app code that isn’t being tested.

If you have an if/else statement in your app code where two possible outcomes could occur, it’s very likely you had some sort of product requirement stating “if A, then B should happen; if C, then D should happen”.

For example, “If the user is logged in, they should be able to see this page content. If the user is not logged in, they should not be able to see this page content.”

---

## When should you use TDD?

It’s important to note that TDD is not a magic bullet. It won’t solve all your problems, and there are definitely cases where it doesn’t make sense to use TDD.

TDD is great when you:

- have clear project requirements
- have clear inputs and outputs (pure functions, API endpoints)
- are fixing bugs!

---

### Clear Project Requirements

It’s hard to write your test cases up front if you don’t have clear project requirements. However, if you know exactly what the acceptance criteria are and what the expected functionality is, go start writing your tests!

---

### Clear Inputs and Outputs

Same for when you have clear inputs and outputs. If you are writing a currency formatting function, and you know that `formatCurrency(3.50, 'USD')` should result in `$3.50`, then go write your tests first!

Think of all the weird input you could get and how would you handle it. What if your method was called with missing arguments, like `formatCurrency()`? What if a string was passed instead of a number, like `formatCurrency('sorry', 'CAD')`? How would you handle those cases? What would your method do? Return `undefined` or `null`? Throw an error?

Writing your tests upfront allows you to think about all those edge cases and then helps you write your function so that it behaves properly.

---

### Bug Fixes

Do you have a bug you’re fixing? Great! Write a test for what *should* be happening when the app is functioning properly. Now go fix the bug. Look at you! You just did some TDD!

A huge benefit to this approach is also that you’ve now written a test to help ensure this particular bug won’t creep back into your application again.

---

## When not to use TDD?

TDD probably doesn’t make sense for:

- exploratory programming
- UI development (debatable)

---

### Exploratory Programming

If you don’t know what exactly you're building, or if you don’t have clear requirements, then it’s really hard to write tests first, because you don’t know what the expected behavior should be! If you’re experimenting with something or trying out something new in your app, TDD might not make sense to use.

---

### UI Development

This one I’m still undecided on, so I would love to hear anyone’s thoughts and opinions on this. In the past, I’ve avoided doing TDD when developing UIs because most of the time, I don’t know perfectly beforehand what HTML elements I’m going to use, or what my app state will look like, or what functions I’ll end up writing.

That’s not to say that TDD is impossible in this case, it just takes a little more forethought. And maybe that’s not such a bad thing.

However, the new frontend testing philosophies that are starting to emerge seem like they would make TDD with UI development make much more sense.

In particular, the React world seems to be moving away from using [Enzyme](https://airbnb.io/enzyme/), which allows you to shallow render and to test implementation details of your components, and moving towards Kent Dodds’ [React Testing Library](https://testing-library.com/docs/react-testing-library/intro), which emphasizes testing things that a user could actually see and interact with.

So rather than writing tests stating that some function will be called and it will change the state in some way, you would test that some button is present on the page, and that when the button is clicked, some other text or data is present on the page after that.

This seems like a much more approachable way to do TDD while developing UIs.

---

## Demo

Demo time! Let’s see what a typical development process might look like when using TDD while creating a caesar cipher implemented in JavaScript.

All the code that follows can be [found on GitHub here](https://github.com/thawkin3/tdd-demo).

If you’re not familiar with what a caesar cipher is, it’s a very simple method of “encoding” a message, which the person receiving the message can then “decode”. The message is encoded by shifting each letter in the message by a specified amount and then decoded by shifting each letter in the opposite direction by that same amount.

![Caesar Cipher](https://dev-to-uploads.s3.amazonaws.com/i/rxyob8kg44boj1hrmopk.png)
<figcaption>Caesar Cipher (image from Wikipedia)</figcaption>

For example, if the un-encoded message is `Hello world!`, and the shift amount is 5, then the encoded message becomes `Mjqqt btwqi!`. The “H” moves forward 5 letters in the alphabet to “M”, the “e” moves forward 5 letters in the alphabet to “j”, and so on.

---

### Product Requirements

Ok, so what might our product requirements be? Let’s define them as:

1. takes a string and a shift value and returns a new string
2. shifts the A-Z characters by the correct amount
3. does not affect non-alphabetic characters
4. maintains case and handles uppercase and lowercase letters
5. handles wrapping past the end of the alphabet
6. handles shift values greater than 26
7. handles shift values less than 0
8. handles bad input

---

### Initial app code and test code

Let’s say that we’ll have two files: `caesar-cipher.js` and `caesar-cipher.test.js`.

The test file looks like this:

```js
import { encode } from './caesar-cipher'

describe('caesar cipher', () => {
  // TODO: write tests here
})
```

And the source code looks like this:

```js
export const encode = () => {}
```

---

### Requirement 1: takes a string and a shift value and returns a new string

Let’s write our test first by putting this test inside the `describe` block:

```js
it('takes a string and a shift value and returns a new string', () => {
  expect(typeof(encode('abc', 1))).toBe('string')
})
```

The test fails! And of course it will, because we just have an empty shell of an `encode` method right now that just returns `undefined`.

Now let’s write our source code to make the test pass:

```js
export const encode = (str, shiftAmount) => {
  return str
}
```

This is just returning the original string that the method is passed, but that’s ok for now. Our test is just concerned with the type of data the method returns, so with this test passing, we’re ready to move on to the next product requirement.

---

### Requirement 2: shifts the A-Z characters by the correct amount

Our test will be:

```js
it('shifts the A-Z characters by the correct amount', () => {
  expect(encode('abc', 1)).toBe('bcd')
  expect(encode('test', 2)).toBe('vguv')
})
```

The test fails! And that’s expected, because again, we’re not encoding the string yet. We’re just returning the original unmodified string.

Let’s write some source code to meet this requirement:

```js
export const encode = (str, shiftAmount) => {
  const encryptedMessage = str.split('').map((character, index) => {
    const code = str.charCodeAt(index)
    const shiftedCode = code + shiftAmount
    return String.fromCharCode(shiftedCode)
  })

  return encryptedMessage.join('')
}
```

This code splits the string into an array of characters and then loops over them. For each character, it finds the character code, adds the shift amount to it, and then gets the character from the new character code. It then joins the array back into a string and returns our encrypted message.

All tests are passing now, so we can move on to our next product requirement.

---

### Requirement 3: does not affect non-alphabetic characters

Let’s write a test:

```js
it('does not affect non-alphabetic characters', () => {
  expect(encode('abc123', 1)).toBe('bcd123')
})
```

The test fails! Our method is shifting the numbers as well as the letters. Let’s modify our source code to address this:

```js
export const encode = (str, shiftAmount) => {
  const encryptedMessage = str.split('').map((character, index) => {
    const code = str.charCodeAt(index)

    // 97-122 => a-z
    if (code >= 97 && code <= 122) {
      const shiftedCode = code + shiftAmount
      return String.fromCharCode(shiftedCode)
    }

    return character
  })

  return encryptedMessage.join('')
}
```

Now we’re only transforming characters that fall into the character code range of 97–122, which maps to the characters a-z.

Let’s move on to our next requirement.

---

### Requirement 4: maintains case and handles uppercase and lowercase letters

Here’s our test:

```js
it('maintains case', () => {
  expect(encode('aBc', 1)).toBe('bCd')
})
```

The test fails! Again, our method is only transforming characters in the 97–122 character code range, which means that while we’re handling lowercase a-z, we’re ignoring uppercase A-Z. Let’s also add the correct range for that, which is 65–90:

```js
export const encode = (str, shiftAmount) => {
  const encryptedMessage = str.split('').map((character, index) => {
    const code = str.charCodeAt(index)

    // 97-122 => a-z; 65-90 => A-Z
    if ((code >= 97 && code <= 122) || (code >= 65 && code <= 90)) {
      const shiftedCode = code + shiftAmount
      return String.fromCharCode(shiftedCode)
    }

    return character
  })

  return encryptedMessage.join('')
}
```

There we go, much better. On to the next requirement.

---

### Requirement 5: handles wrapping past the end of the alphabet

Let’s write our test:

```js
it('handles wrapping past the end of the alphabet', () => {
  expect(encode('xyz', 2)).toBe('zab')
})
```

The test fails! Rather than wrapping the letters at the end of the alphabet back to the beginning of the alphabet, the method is just moving each character to the shifted character code, even if that character code is outside of our range.

To fix this, we can make use of the modulo operator:

```js
export const encode = (str, shiftAmount) => {
  const encryptedMessage = str.split('').map((character, index) => {
    const code = str.charCodeAt(index)

    // 97-122 => a-z; 65-90 => A-Z
    if (code >= 65 && code <= 90) {
      const shiftedCode = ((code + shiftAmount - 65) % 26) + 65
      return String.fromCharCode(shiftedCode)
    } else if (code >= 97 && code <= 122) {
      const shiftedCode = ((code + shiftAmount - 97) % 26) + 97
      return String.fromCharCode(shiftedCode)
    }

    return character
  })

  return encryptedMessage.join('')
}
```

Now our messages will wrap properly around the ends of the alphabet. Next requirement.

---

### Requirement 6: handles shift values greater than 26

Similar to how letters near the end of the alphabet should wrap properly, using a large shift amount should wrap properly too. For example, even the letter “a” shifted by 28 moves further than the full length of the alphabet, so it needs to wrap to become “c”.

Here’s our test:

```js
it('handles shift values greater than 26', () => {
  expect(encode('abc', 26)).toBe('abc')
  expect(encode('abc', 28)).toBe('cde')
})
```

And… the test passes! Would you look at that. It turns out that our modulo operator we used during requirement 5 helped us out here with requirement 6.

So, no new source code to write at the moment. And that’s ok. Sometimes you’ve already covered the test case, whether intentionally or unintentionally.

Let’s move on to the next requirement.

---

### Requirement 7: handles shift values less than 0

What if we wanted to shift our characters in the other direction by using a negative shift amount? We’ll need to make sure our letters will wrap properly from the beginning of the alphabet back around to the end of the alphabet.

Here’s our test:

```js
it('handles shift values less than 0', () => {
  expect(encode('abc', 0)).toBe('abc')
  expect(encode('abc', -2)).toBe('yza')
})
```

The test fails! Ok let’s go fix this in our source code by also applying the modulo operator to the shift amount:

```js
export const encode = (str, shiftAmount) => {
  const encryptedMessage = str.split('').map((character, index) => {
    const code = str.charCodeAt(index)
    const moduloShiftAmount = (shiftAmount % 26) + 26

    // 97-122 => a-z; 65-90 => A-Z
    if (code >= 65 && code <= 90) {
      const shiftedCode = ((code + moduloShiftAmount - 65) % 26) + 65
      return String.fromCharCode(shiftedCode)
    } else if (code >= 97 && code <= 122) {
      const shiftedCode = ((code + moduloShiftAmount - 97) % 26) + 97
      return String.fromCharCode(shiftedCode)
    }

    return character
  })

  return encryptedMessage.join('')
}
```

This ensures that our shift amount is always transformed into a positive number when we start to encode our characters.

---

### Requirement 8: handles bad input

Last but not least, let’s make sure our method can gracefully respond to bad input or when it is used incorrectly.

Our test will be:

```js
it('handles bad input', () => {
  expect(encode()).toBe('')
  expect(encode(1, 1)).toBe('')
  expect(encode(1, 'abc')).toBe('')
  expect(encode('abc')).toBe('abc')
})
```

We could probably write some more expectations as well, but this at least looks for missing arguments and incorrect argument types.

Let’s update our source code to handle this now:

```js
export const encode = (str = '', shiftAmount = 0) => {
  if (typeof str !== 'string' || typeof shiftAmount !== 'number') {
    return ''
  }

  const encryptedMessage = str.split('').map((character, index) => {
    const code = str.charCodeAt(index)
    const moduloShiftAmount = (shiftAmount % 26) + 26

    // 97-122 => a-z; 65-90 => A-Z
    if (code >= 65 && code <= 90) {
      const shiftedCode = ((code + moduloShiftAmount - 65) % 26) + 65
      return String.fromCharCode(shiftedCode)
    } else if (code >= 97 && code <= 122) {
      const shiftedCode = ((code + moduloShiftAmount - 97) % 26) + 97
      return String.fromCharCode(shiftedCode)
    }

    return character
  })

  return encryptedMessage.join('')
}
```

Adding some default values and doing some type checking makes the test pass. We did it! All 8 requirements are now complete.

---

If you were to check the code coverage on our caesar cipher right now, you’d see that we have 100% code coverage. Awesome! While this code isn’t incredibly complex, we did have some default values for the function parameters, and we had some branching logic looking at the value types and the character codes.

By using TDD, we’ve made sure every condition of our method is adequately tested, so we can use the caesar cipher in our made up app with confidence.

---

## Conclusion

Well, that’s it for now! We’ve gone over what TDD is, why it’s useful, what a typical TDD workflow looks like, when to use TDD, when not to use TDD, and even did some hands-on TDD to write our `encode` method for our caesar cipher.

If you want some more practice, go write a `decode` method!

