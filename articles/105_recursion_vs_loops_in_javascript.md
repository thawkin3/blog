# Recursion vs. Loops in JavaScript

Recursion doesn’t have to be scary

![Photo by [Compare Fibre](https://unsplash.com/@comparefibre?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/12480/0*UsnUMDsvzFQj2DdQ)*Photo by [Compare Fibre](https://unsplash.com/@comparefibre?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

Recursion is the nemesis of every developer, only matched in power by its friend, regular expressions.

Recursion can be hard to wrap your head around, for a couple reasons. First, you have to grasp the concept of a function calling itself. Second, you have to understand the difference between the base case and the recursive case, or else you may find yourself stuck in an infinite loop until you cause a stack overflow.

If you can master those two concepts, recursion isn’t actually as scary or complex as you may think. In fact, recursive code is often shorter to write and (in some cases) easier to read.

Let’s walk through five code examples together. We’ll solve each problem first by using a loop, and then we’ll solve it using recursion. Which approach is better? That’s for you to decide.

---

## Example #1: Factorial

Let’s write a function that allows us to calculate the factorial for positive integers. Factorials are written like this: `5!`. And the formula for a factorial looks like this:

```
n! = n * (n - 1) * (n - 2) * ... * 1
```

So `5!` would be:

```
5! = 5 * 4 * 3 * 2 * 1 = 120
```

How could we write a function for this? One approach would be to use a `while loop`. We can create a variable for our `result` and then multiply it with `x` in a loop, decrementing `x` by 1 each time. The code looks like this:

```js
const factorial = (x) => {
  if (x < 0) {
    throw new Error('x must be greater than or equal to 0');
  }

  if (x <= 1) {
    return 1;
  }

  let result = 1;

  while (x > 0) {
    result *= x;
    x--;
  }

  return result;
};
```

Note that we’ve also handled bad inputs less than 0, and we’ve simplified the cases of 0 and 1 since `0!` and `1!` both equal 1.

So that’s a solution that utilizes a loop. What if we wanted to write this function recursively? A recursive solution could look like this:

```js
const factorial = (x) => {
  if (x < 0) {
    throw new Error('x must be greater than or equal to 0');
  }

  if (x <= 1) {
    return 1;
  }

  return x * factorial(x - 1);
};
```

Whoa! That’s much shorter. Note that on the last line we return `x` multiplied by the result of calling our same `factorial` function again with `x — 1` as the argument. This is the recursive case.

If we just called our function recursively, we’d end up in an infinite loop, so we need some way to break out. That’s why we have the condition that handles when `x <= 1`. When we reach that point, we stop recursively calling our `factorial` function. This is the base case.

Ready for another example?

---

## Example #2: Power

Let’s try a similar example. This time let’s implement the power function, which calculates the result of a base number raised to an exponent. So for example, `2^3` is two raised to the power of three, or `2 * 2 * 2`, which is 8.

In JavaScript, there are a couple of ways to do power functions natively, like by calling `Math.pow(2, 3)` or using the exponentiation syntax like `2 ** 3`. For now, let’s pretend like those utilities don’t exist. How could we write this functionality ourselves?

Using a `while loop`, we could write a function that looks like this:

```js
const power = (x, y) => {
  if (y < 0) {
    throw new Error(
      'exponent must be greater than or equal to 0 (for the purposes of this example)'
    );
  }

  if (y === 0) {
    return 1;
  }

  let result = 1;

  while (y > 0) {
    result *= x;
    y--;
  }

  return result;
};
```

Note that we’ll ignore negative exponents for the purpose of this example, even though it’s perfectly valid to have a negative exponent in real life. We’ll also handle the case where the exponent is 0, since any number raised to 0 is always itself (`2^0 = 2`).

Just like we did for our factorial example, we’ve started with an initial `result` of 1. We then multiply `result` by `x` in a loop as we decrement `y` until we reach 0.

Now, what would a recursive solution look like? How about this:

```js
const power = (x, y) => {
  if (y < 0) {
    throw new Error(
      'exponent must be greater than or equal to 0 (for the purposes of this example)'
    );
  }

  if (y === 0) {
    return 1;
  }

  return x * power(x, y - 1);
};
```

Wow, it’s much shorter again! At the very bottom we have our recursive case where we multiply `x` by the result of calling our `power` function with `x` and a decremented `y — 1`.

Right above that we have our base case in which we return 1 if `y` equals 0. That way we can break away from infinitely calling our `power` function.

An interesting callout is that in both of our recursive solutions we don’t have to keep track of a `result` variable. The call stack does that for us!

Ready to move on?

---

## Example #3: Sum the Numbers in an Array

Let’s write a function that sums all the numbers in an array. In JavaScript, the `reduce` method provides a simple tool for reducing an array into a sum like this:

```js
numbers.reduce((number, sum) => number + sum, 0);
```

For the time being, let’s pretend that `reduce` doesn’t exist. How could we write our own function? One option would be to use a `for loop` to loop over all the items in the array and add them together. The code looks like this:

```js
const sumNumbersArray = (numbers) => {
  let sum = 0;

  for (let i = 0; i < numbers.length; i++) {
    sum += numbers[i];
  }

  return sum;
};
```

Short and simple. And what if we wanted to do this recursively? We could write a recursive solution like this:

```js
const sumNumbersArray = (numbers) => {
  if (numbers.length === 0) {
    return 0;
  }

  return numbers[0] + sumNumbersArray(numbers.slice(1));
};
```

Each time we add the first number to the result of calling the `sumNumbersArray` function on the rest of our array, excluding the first number.

Now is this a better way than using the `for loop`? It’s a subjective question, but in my opinion, using recursion here feels more unnatural than just using a loop. What do you think?

In the meantime, let’s consider another example.

---

## Example #4: Find the Largest Number in an Array

Let’s write a function that takes an array of numbers and returns the largest one. In JavaScript, we could use the `Math.max` utility function like this:

```js
Math.max(...numbers);
```

But for argument’s sake, let’s once again pretend like we don’t have this functionality available. How could we write it ourselves? Using a `for loop`, our code could look like this:

```js
const maxNumberArray = (numbers) => {
  let max = numbers[0];

  for (let i = 1; i < numbers.length; i++) {
    if (numbers[i] > max) {
      max = numbers[i];
    }
  }

  return max;
};
```

We start by assuming that the first number in our array is the largest number. Then we loop over the array, starting at the second element, and compare the current element with the max number. If the current element is larger than the max number, we update the max number with that value. Once we’ve finished looping through our list, we’ve found the largest number.

So, fairly straightforward. Now, what if we wanted to write this recursively? We could write our function like this:

```js
const maxNumberArray = (numbers) => {
  if (numbers.length <= 1) {
    return numbers[0];
  }

  const numberA = numbers[0];
  const numberB = maxNumberArray(numbers.slice(1));

  return numberA > numberB ? numberA : numberB;
};
```

Does this one feel a little harder to intuitively grasp? Let’s walk through it.

If the array contains zero elements or one element, we’ll just return the first element (which is `undefined` in the case of an empty array). That’s our base case.

For arrays that contain two or more elements, we’ll do a comparison. We’ll take the first element in the array and then compare it with the result of recursively calling `maxNumberArray` on the remainder of the array. We’ll then return whichever number is bigger.

Still not clear? Me neither.

Let’s look at an example of this being called, and let’s walk through the steps each time. Imagine we call the function with this argument:

```js
maxNumberArray([1, 9, 5, 7, 3, 8, 2, 4])
```

As the function recursively calls itself, the unresolved functions get pushed onto the call stack. Once we reach the base case of an array with only one element, the functions begin to resolve. So, this is what our comparisons end up looking like:

```
Is 2 greater than 4? 4 is our max number.
Is 8 greater than 4? 8 is our max number.
Is 3 greater than 8? 8 is our max number.
Is 7 greater than 8? 8 is our max number.
Is 5 greater than 8? 8 is our max number.
Is 9 greater than 8? 9 is our max number.
Is 1 greater than 9? 9 is our max number.
```

So, conceptually, we’re walking backwards through the array, starting with the last two numbers. We compare those two numbers and keep the largest. Then we move left by one element and compare that with our current max number. We continue moving left until we reach the beginning of the array, at which point we’ve compared all our numbers and found the largest one.

Is this recursive example better? Again, it’s a subjective question, but I feel like the answer is no. The cognitive complexity of grasping how this recursive function works is higher than the effort required to understand the function implemented with a loop.

The recursive function feels like a “clever” solution but not a “clear” solution.

Let’s do one more example before we wrap up.

---

## Example #5: Find a Key Hidden Inside Boxes within Boxes

This will be our most complex example, but it’s a perfect use case for recursion. Let’s imagine that you have a box. This box can contain multiple other boxes, and those boxes can contain other boxes, and so on. A box may also be empty. And, finally, the box we’re searching for contains a key.

So imagine that:

* The first box (Box A) contains three other boxes (Box B, Box C, and Box D).
* Box B has nothing inside it.
* Box C has two boxes inside it (Box E and Box F).
* Box E has one box inside it (Box G).
* Box G contains the key (hooray!).
* Box F has nothing inside it.
* Box D has one box inside it (Box H).
* Box H has nothing inside it.

Does your head hurt yet?

How could we write a function that searches through the boxes until we find the key?

We could write this function using a `while loop` like this:

```js
export const findKeyInBox = (box) => {
  const pile = [];
  pile.push(box);

  while (pile.length > 0) {
    const currentBox = pile.pop();
    console.log(`Looking in Box ${currentBox.id}`);

    for (let i = 0; i < currentBox.contents.length; i++) {
      if (currentBox.contents[i].type === 'key') {
        console.log(`Found the key in Box ${currentBox.id}!`);
        return currentBox.id;
      }

      if (currentBox.contents[i].type === 'box') {
        console.log(
          `Found Box ${currentBox.contents[i].id} in Box ${currentBox.id}`
        );
        pile.push(currentBox.contents[i]);
      }
    }
  }
};
```

We start with an empty pile (which is an array that we can use as a stack data structure), and then we add our first box to the pile.

Then, while that pile is not empty, we grab a box from the pile and look inside it.

For each item we find in the box, it could either be the key (yay, we found it!) or another box (great…). If we find a key, then we’re done. If we find a box, then we add that box to our pile.

We repeat our loop until we’re all out of boxes or until we’ve found the key.

Now, what if we wanted to find a recursive solution to this problem? We could write our function like this:

```js
export const findKeyInBox = (box) => {
  console.log(`Looking in Box ${box.id}`);

  for (let i = 0; i < box.contents.length; i++) {
    if (box.contents[i].type === 'key') {
      console.log(`Found the key in Box ${box.id}!`);
      return box.id;
    }

    if (box.contents[i].type === 'box') {
      console.log(`Found Box ${box.contents[i].id} in Box ${box.id}`);
      const boxContainingKey = findKeyInBox(box.contents[i]);

    if (boxContainingKey) {
        return boxContainingKey;
      }
    }
  }
};
```

Note the differences between our recursive solution and our loop solution. In the recursive solution we don’t keep track of the pile. We simply look through the first box, and then, if we find more boxes, we recursively call our `findKeyInBox` function on that box. The call stack is once again keeping track of our state for us!

---

## Conclusion

So, which is better: loops or recursion? That’s for you to decide. There usually aren’t significant performance differences between loops and recursion, and the Big O notation is the same, as the same number of operations are performed in each case.

The real thing that you’re optimizing for is readability. Is it easier for you to comprehend the recursive solution or the loop solution? And what about for the next developer that will read this code?

Code is read 10x more than it is written, so make sure you optimize for readability.

---

Thanks for reading! If you’d like to revisit these examples and see test cases, you can find all of the functions we’ve discussed in this [GitHub repo](https://github.com/thawkin3/recursion-vs-loops-js).
