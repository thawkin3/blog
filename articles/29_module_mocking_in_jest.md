# Module Mocking in Jest

When testing JavaScript code using Jest, sometimes you may find yourself needing to mock a module. Whether it's because the module or functions it exports are irrelevant to the specific test, or because you need to stop something like an API request from trying to access an external resource, mocking is incredibly useful.

There are, however, several different approaches to module mocking in Jest, which can lead to confusion. Which approach is the right one for any given scenario?

In this article we'll walk through various scenarios using ES6 modules with named exports, a default export, or a mix of both.


---

## ES6 Module Exports

ES6 modules provide two different ways to export methods and variables from a file: **named exports** and **default exports**. Any given file could have one or more named exports, one default export, or both named exports and a default export.

The way you mock your module in Jest will depend on the way in which data is exported from the module.


---

## Module Mocking Scenarios

When testing a module in Jest, there are several possible module mocking scenarios you might run into:

1. Not needing to mock anything at all
2. Automatically mocking the module
3. Mocking the module using the module factory method
4. Mocking the module using the module factory method and mock implementations
5. Partially mocking some methods in the module but not all the methods

Let's explore each of these possibilities below.


---

## Mocking Named Exports

First let's consider how we would test a module that only exports named exports. We'll start with a fictional `utils.js` file that contains three methods that are all exported as named exports:

```js
export const method1 = () => 'You have called Method 1'

export const method2 = () => 'You have called Method 2'

export const method3 = () => 'You have called Method 3'
```

If we were to test these methods exactly as they are, without needing to mock anything, our test file would look like this:

```js
import { method1, method2, method3 } from './utils.js'

describe('named exports - unmocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called Method 1')
  })

  it('returns the correct value for Method 2', () => {
    expect(method2()).toBe('You have called Method 2')
  })

  it('returns the correct value for Method 3', () => {
    expect(method3()).toBe('You have called Method 3')
  })
})
```

---

If we wanted to mock these methods using automatic mocking, we could simply pass the file path to the `jest.mock` method.

*Note: In these examples, we are going to be writing tests to verify that the mocking behavior is working properly. These are somewhat "meta" tests, in that you probably wouldn't need to be testing that Jest is behaving properly. In a real testing scenario, you'd likely be mocking one module that is consumed by a second module, where the methods from the first module aren't relevant to what you're trying to test in the second module.*

```js
import { method1, method2, method3 } from './utils.js'

jest.mock('./utils.js')

describe('named exports - automatically mocked file with no return values', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).not.toBe('You have called Method 1')
    expect(method1()).toBe(undefined)
  })

  it('returns the correct value for Method 2', () => {
    expect(method2()).not.toBe('You have called Method 2')
    expect(method1()).toBe(undefined)
  })

  it('returns the correct value for Method 3', () => {
    expect(method3()).not.toBe('You have called Method 3')
    expect(method1()).toBe(undefined)
  })
})
```

You can see that for each method, the real return value is replaced by an undefined return value. That's because we automatically mocked the module using this statement: `jest.mock('./utils.js')`.

---

Now, what if we wanted more control over how each method is mocked? In that case, we can use the `jest.mock` method along with a module factory method like so:

```js
import { method1, method2, method3 } from './utils.js'

jest.mock('./utils.js', () => ({
  method1: () => 'You have called a mocked method 1!',
  method2: () => 'You have called a mocked method 2!',
  method3: () => 'You have called a mocked method 3!',
}))

describe('named exports - module factory mocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called a mocked method 1!')
    expect(() => expect(method1).toHaveBeenCalledTimes(1)).toThrow()
  })

  it('returns the correct value for Method 2', () => {
    expect(method2()).toBe('You have called a mocked method 2!')
    expect(() => expect(method2).toHaveBeenCalledTimes(1)).toThrow()
  })

  it('returns the correct value for Method 3', () => {
    expect(method3()).toBe('You have called a mocked method 3!')
    expect(() => expect(method3).toHaveBeenCalledTimes(1)).toThrow()
  })
})
```

As you can see, we now have explicitly set what each of our mocked methods should do. They return the value we've set them to. However, these are not true mock functions or "spies" yet, because we can't spy on things like whether or not any given function has been called.

---

If we wanted to be able to spy on each of our mocked functions, then we'd need to use the module factory along with a mock implementation for each function like this:

```js
import { method1, method2, method3 } from './utils.js'

jest.mock('./utils.js', () => ({
  method1: jest.fn().mockImplementation(() => 'You have called a mocked method 1!'),
  method2: jest.fn().mockImplementation(() => 'You have called a mocked method 2!'),
  method3: jest.fn().mockImplementation(() => 'You have called a mocked method 3!'),
}))

describe('named exports - module factory with mock implementation mocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called a mocked method 1!')
    expect(method1).toHaveBeenCalledTimes(1)
  })

  it('returns the correct value for Method 2', () => {
    expect(method2()).toBe('You have called a mocked method 2!')
    expect(method2).toHaveBeenCalledTimes(1)
  })

  it('returns the correct value for Method 3', () => {
    expect(method3()).toBe('You have called a mocked method 3!')
    expect(method3).toHaveBeenCalledTimes(1)
  })
})
```

As you can see, by utilizing the `jest.fn()` method to create a mock function and then defining its implementation using the `mockImplementation` method, we can control what the function does and spy on it to see how many times it was called.

---

Finally, if we only want to mock some of the methods but not all of them, we can use the `jest.requireActual` method to include the actual module exports in our test file. For example, here we mock the `method3` function but *not* the `method1` or `method2` functions:

```js
import { method1, method2, method3 } from './utils.js'

jest.mock('./utils.js', () => ({
  ...jest.requireActual('./utils.js'),
  method3: jest.fn().mockImplementation(() => 'You have called a mocked method!'),
}))

describe('named exports - partially mocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called Method 1')
  })

  it('returns the correct value for Method 2', () => {
    expect(method2()).toBe('You have called Method 2')
  })

  it('returns the correct value for Method 3', () => {
    expect(method3()).toBe('You have called a mocked method!')
  })
})
```

---

## Mocking Default Exports

We've covered quite a few use cases for module mocking! But, each of the scenarios which we've considered so far used named exports. How would we mock our module if it made use of a default export instead?

Now let's imagine that our `utils.js` file has only a single method that is exported as its default export like so:

```js
const method1 = () => 'You have called Method 1'

export default method1
```

To test this method without mocking it, we would write a test like this:

```js
import method1 from './utils.js'

describe('default export - unmocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called Method 1')
  })
})
```

---

If we wanted to automatically mock the module, we could use the `jest.mock` method again, just like we did with our module that used named exports:

```js
import method1 from './utils.js'

jest.mock('./utils.js')

describe('default export - automatically mocked file with no return values', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).not.toBe('You have called Method 1')
    expect(method1()).toBe(undefined)
  })
})
```

---

If we need more control over what the mock function looks like, we can again use the module factory method. However, this is where things differ from our previous approach with named exports.

In order to successfully mock a module with a default export, we need to return an object that contains a property for `__esModule: true` and then a property for the `default` export. This helps Jest correctly mock an ES6 module that uses a default export.

```js
import method1 from './utils.js'

jest.mock('./utils.js', () => ({
  __esModule: true,
  default: () => 'You have called a mocked method 1!',
}))

describe('default export - module factory mocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called a mocked method 1!')
    expect(() => expect(method1).toHaveBeenCalledTimes(1)).toThrow()
  })
})
```

---

If we need to be able to spy on our method, we can use the `mockImplementation` method that we've used before. Note that this time we don't have to use the `__esModule: true` flag:

```js
import method1 from './utils.js'

jest.mock('./utils.js', () => jest.fn().mockImplementation(() => 'You have called a mocked method 1!'))

describe('default export - module factory with mock implementation mocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called a mocked method 1!')
    expect(method1).toHaveBeenCalledTimes(1)
  })
})
```

---

For a module that only has a single export that is the default export, we won't have any way to only partially mock the module, so that case is not applicable here.


---

## Mocking Named Exports and a Default Export

Alright, we've now covered a module that has only named exports and a module that has only a default export. Expert mode time: How about a module that has both named exports and a default export? Let's see if we can apply what we've learned so far to mock this kind of module.

We'll start again with our `utils.js` file, which will look like this:

```js
export const method1 = () => 'You have called Method 1'

export const method2 = () => 'You have called Method 2'

export const method3 = () => 'You have called Method 3'

const defaultMethod = () => 'You have called the Default Method'

export default defaultMethod
```

Note that we have three named exports and one default export, so a total of four methods to work with.

To test all four of these methods without mocking anything, we would write our tests like this:

```js
import defaultMethod, { method1, method2, method3 } from './utils.js'

describe('default and named exports - unmocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called Method 1')
  })

  it('returns the correct value for Method 2', () => {
    expect(method2()).toBe('You have called Method 2')
  })

  it('returns the correct value for Method 3', () => {
    expect(method3()).toBe('You have called Method 3')
  })

  it('returns the correct value for Default Method', () => {
    expect(defaultMethod()).toBe('You have called the Default Method')
  })
})
```

---

If we wanted to automatically mock all of our methods, we'd still just pass the file path to the `jest.mock` method. Nice and easy:

```js
import defaultMethod, { method1, method2, method3 } from './utils.js'

jest.mock('./utils.js')

describe('default and named exports - automatically mocked file with no return values', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).not.toBe('You have called Method 1')
    expect(method1()).toBe(undefined)
  })

  it('returns the correct value for Method 2', () => {
    expect(method2()).not.toBe('You have called Method 2')
    expect(method1()).toBe(undefined)
  })

  it('returns the correct value for Method 3', () => {
    expect(method3()).not.toBe('You have called Method 3')
    expect(method1()).toBe(undefined)
  })

  it('returns the correct value for Default Method', () => {
    expect(defaultMethod()).not.toBe('You have called the Default Method')
    expect(defaultMethod()).toBe(undefined)
  })
})
```

---

To be able to actually define the mock methods, we'd use the module factory method, which looks like a combination of what we've used for the named exports and the default export. The object we return will have keys for `__esModule` and `default` in addition to a key for each named export method name:

```js
import defaultMethod, { method1, method2, method3 } from './utils.js'

jest.mock('./utils.js', () => ({
  __esModule: true,
  default: () => 'You have called a mocked default method!',
  method1: () => 'You have called a mocked method 1!',
  method2: () => 'You have called a mocked method 2!',
  method3: () => 'You have called a mocked method 3!',
}))

describe('default and named exports - module factory mocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called a mocked method 1!')
    expect(() => expect(method1).toHaveBeenCalledTimes(1)).toThrow()
  })

  it('returns the correct value for Method 2', () => {
    expect(method2()).toBe('You have called a mocked method 2!')
    expect(() => expect(method2).toHaveBeenCalledTimes(1)).toThrow()
  })

  it('returns the correct value for Method 3', () => {
    expect(method3()).toBe('You have called a mocked method 3!')
    expect(() => expect(method3).toHaveBeenCalledTimes(1)).toThrow()
  })

  it('returns the correct value for the Default Method', () => {
    expect(defaultMethod()).toBe('You have called a mocked default method!')
    expect(() => expect(defaultMethod).toHaveBeenCalledTimes(1)).toThrow()
  })
})
```

---

And if we need to be able to spy on those methods, we can use a very similar approach, but this time with the addition of the `jest.fn().mockImplementation` method again:

```js
import defaultMethod, { method1, method2, method3 } from './utils.js'

jest.mock('./utils.js', () => ({
  __esModule: true,
  default: jest.fn().mockImplementation(() => 'You have called a mocked default method!'),
  method1: jest.fn().mockImplementation(() => 'You have called a mocked method 1!'),
  method2: jest.fn().mockImplementation(() => 'You have called a mocked method 2!'),
  method3: jest.fn().mockImplementation(() => 'You have called a mocked method 3!'),
}))

describe('default and named exports - module factory with mock implementation mocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called a mocked method 1!')
    expect(method1).toHaveBeenCalledTimes(1)
  })

  it('returns the correct value for Method 2', () => {
    expect(method2()).toBe('You have called a mocked method 2!')
    expect(method2).toHaveBeenCalledTimes(1)
  })

  it('returns the correct value for Method 3', () => {
    expect(method3()).toBe('You have called a mocked method 3!')
    expect(method3).toHaveBeenCalledTimes(1)
  })

  it('returns the correct value for the Default Method', () => {
    expect(defaultMethod()).toBe('You have called a mocked default method!')
    expect(defaultMethod).toHaveBeenCalledTimes(1)
  })
})
```

---

And finally, to only partially mock the module, we can make use of `jest.requireActual` again and then override the methods that we want. Note the use of `__esModule: true` here again:

```js
import defaultMethod, { method1, method2, method3 } from './utils.js'

jest.mock('./utils.js', () => ({
  __esModule: true,
  ...jest.requireActual('./utils.js'),
  method3: jest.fn().mockImplementation(() => 'You have called a mocked method!'),
}))

describe('default and named exports - partially mocked file', () => {
  it('returns the correct value for Method 1', () => {
    expect(method1()).toBe('You have called Method 1')
  })

  it('returns the correct value for Method 2', () => {
    expect(method2()).toBe('You have called Method 2')
  })

  it('returns the correct value for Method 3', () => {
    expect(method3()).toBe('You have called a mocked method!')
  })

  it('returns the correct value for the Default Method', () => {
    expect(defaultMethod()).toBe('You have called the Default Method')
  })
})
```

---

## Conclusion

We've covered a lot of module mocking scenarios today! You should now have a large set of tools at your disposal so that you can successfully mock whatever you need to during your testing.

One option that we didn't discuss is how to mock a module using the `__mocks__` directory, but that's a topic for another day.

If you'd like to check out these examples in a working git repo, feel free to check out the code here: [https://github.com/thawkin3/jest-module-mocking-demo](https://github.com/thawkin3/jest-module-mocking-demo).

Thanks for reading, and happy testing!
