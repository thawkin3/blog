# Understanding the Difference Between Reference and Value in JavaScript

“Objects are passed by reference, not by value.”

Have you heard that phrase before but struggled to understand what it means? This is a concept that often causes new developers to stumble when first learning JavaScript.

In this article we’ll look at a few examples to better understand how variables are treated and what the difference is between “reference” and “value”.

---

## Passing Primitives

Primitive data types in JavaScript are things like `number`, `string`, `boolean`, or `undefined`. There are other primitives, but those are the most common ones.

**Primitives are passed by value.** To understand what that means, let’s look at a simple example:

```js
const myNumber = 10;

const addOne = x => x + 1;

const anotherNumber = addOne(myNumber);

console.log(myNumber);
console.log(anotherNumber);
```

In this example we have a variable `myNumber` which has the value `10`. We have a function `addOne` that takes an argument and returns the argument plus `1`. Then we call the `addOne` function using the `myNumber` variable as the argument and save the result to another variable called `anotherNumber`. Finally, we log to the console the values of both of our variables.

So, the question is: what gets logged?

If you answered `10` and `11`, you are correct. Because numbers are passed by value, the value of `myNumber` is passed to the function, but when the number is incremented, the `myNumber` variable is unaffected.

---

## Comparing Primitives

So now we know that primitives are passed by value. But what about when they’re compared? To answer that, let’s look at another example:

```js
const x = 5;
const y = 5;

console.log(x === y);
```

We have two variables, `x` and `y`, both of which have the value `5`. When we log to the console a check for strict equality, what do we get?

If you answered `true`, you are correct. This is because **primitives are compared by value** too, and `5` is equal to `5`.

---

## Passing Objects

Now, what about data types that are not primitives in JavaScript? For example, `objects` are not primitives, and neither are `arrays` (which are really just objects, secretly).

**Objects are passed by reference.** To understand what that means, let’s look at a simple example:

```js
const someNumbers = [1, 2, 3];

const addNumberToArray = arr => {
  arr.push(100);
  return arr;
}

const otherNumbers = addNumberToArray(someNumbers);

console.log(someNumbers);
console.log(otherNumbers);
```

In this example we have a variable `someNumbers` which is an array that contains three elements. We have a function `addNumberToArray` that takes an argument (an array), pushes the value `100` into the array, and then returns the array. Then we call the `addNumberToArray` function using the `someNumbers` variable as the argument and save the result to another variable called `otherNumbers`. Finally, we log to the console the values of both of our variables.

So, the question is: what gets logged?

If you answered `[1, 2, 3, 100]` and `[1, 2, 3, 100]`, you are correct.

Oh no! We’ve inadvertently modified our input array that we passed to the function!

Because objects are passed by reference, the reference to `someNumbers` is passed to the function. So, when the value `100` is pushed to the array, that value is being pushed into the same array that `someNumbers` represents.

If you wanted to be sure not to modify the original array in a function like this, it would be necessary to push the value `100` into a copy of the input array using the `concat` method or the ES6 `spread` operator. For example:

```js
const someNumbers = [1, 2, 3];

const addNumberToArray = arr => [...arr, 100];

const otherNumbers = addNumberToArray(someNumbers);

console.log(someNumbers);
console.log(otherNumbers);
```

Now when we log those two variables to the console, we’ll see `[1, 2, 3]` and `[1, 2, 3, 100]` get logged. Much better.

---

## Comparing Objects

So now we know that objects are passed by reference. But what about when they’re compared? To answer that, let’s look at another example:

```js
const object1 = { someKey: 'someValue' }
const object2 = { someKey: 'someValue' }

console.log(object1 === object2);
```

We have two variables, `object1` and `object2`, both of which are an object with only one property. The key is `someKey`, and the value is `someValue`. When we log to the console a check for strict equality, what do we get?

If you answered `false`, you are correct. This is because **objects are compared by reference** too. Even though these two objects are the same in value, they are not the same object. These are two separate objects held in two separate variables, so their references are different.

If you wanted a quick sanity check, you could also check if each object is equal to itself, like this:

```js
console.log(object1 === object1);
console.log(object2 === object2);
```

Both of these logs to the console will be `true` since in each case you are comparing an object to itself, which is the same reference.

If you really wanted to check if `object1` and `object2` had the same keys and values, you’d need to write a utility method that would loop over the objects’ keys and values and make sure that they are all identical. Or, you could use a helper method from a library like `lodash` which implements this functionality for you.

---

## Conclusion

Primitives are passed and compared by value. Objects are passed and compared by reference. Understanding the difference will save you a lot of headaches debugging your code!

*This article was originally published here: https://medium.com/javascript-in-plain-english/understanding-the-difference-between-reference-and-value-in-javascript-21c0a6bac7a9*

---

## Update

The mental model I've had that "primitives are passed by value; objects are passed by reference" has served me well over the years, and it's been helpful in understanding what behavior to expect, but it appears that I've been using the incorrect terms to explain what's really going on underneath the hood.

A more correct way to explain this concept would be:

> Primitives are passed by value. Objects are passed by "copy of a reference".

> Or, arguments in JavaScript are always passed by value. But the "value" of an object is the reference.
