# 3 Common Mistakes With React Keys

When first learning React, it's easy to get tripped up by simple mistakes. Even seasoned developers make mistakes.

One area that is oftentimes misunderstood is how to use a `key` when iterating over items to be displayed in the UI.

In this article we'll look at three mistakes with React keys and how to avoid them.

---

## Why Are Keys Necessary

First off, let's make sure we understand why we use keys.

The [React docs](https://reactjs.org/docs/lists-and-keys.html#keys) explain that "keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity."

So, keys help identify elements, especially when React is performing its diffing algorithm to note what has changed in the UI.

With that basic understanding in mind, let's look at our first mistake.

---

## Mistake #1: Not using a key

If you are iterating over an array of items, perhaps with the `Array.map` helper method, and then displaying those items in the UI, you must add a key to each item.

For example, this shopping list component iterates over an array of grocery items:

{% gist https://gist.github.com/thawkin3/0570f1c0a6365734c3d217bdea98b83d %}

But, we forgot to add a key on our `<li>` elements! React will help us out here and actually adds a warning to the JavaScript console when a key is forgotten:

![Error message when forgetting to add a key](https://dev-to-uploads.s3.amazonaws.com/i/vcv20dz0y184wh0kg5w4.png)
<figcaption>Error message when forgetting to add a key</figcaption>

The simple fix is to add that key on the `<li>` element on line 9, like this:

{% gist https://gist.github.com/thawkin3/3a010fa18f1e4613763d7d4644350c3f %}

---

## Mistake #2: Adding the key in the wrong place

Even when using keys, sometimes developers will misunderstand where the key should go. For example, what if we broke out our shopping list into two separate components: the list itself and the list items.

You might think to do something like this:

{% gist https://gist.github.com/thawkin3/1d1ff062658c83942ea288b0306bd3e7 %}

As you can see, the key is being added on the `<li>` element in the `ShoppingListItem` component on line 3.

However, the correct place to add the key is the place in which the mapping or iteration is happening. So adding the key down on line 11 would be more appropriate:

{% gist https://gist.github.com/thawkin3/c7da44a6c87d7b6134e23a09165c8ccc %}

Much better!

---

## Mistake #3: Not using a stable identifier as a key, especially when working with dynamic lists

Now that we know we need to add a key and where to add it, it's time to tackle the most critical part: what the key should be.

Ideally, the key should be a unique identifier that does not change. If you are iterating over an array of objects retrieved from the backend, each object likely has an `id` property you could use. In the case of our shopping list above, each shopping list item name was unique, so the name itself worked well.

If you don't have a unique identifier in the data itself, it is *sometimes* acceptable to use the index as the key. For example, this list of students having non-unique names:

{% gist https://gist.github.com/thawkin3/2a2e8295f7c41f6f902e2e1e1172e5e3 %}

Tyler is such a great name that we have two Tylers in the list of classmates. Without having a unique ID, using an index as the key is an acceptable solution.

But! Here's the catch: If our data is dynamic in any way, we need to be careful. For example, if our list could be sorted or filtered, we'd run into some problems here when using an index as the key.

---

Let's now imagine that our list of classmates is used for taking attendance in class. Each student's name will have a checkbox next to it, and, for the teacher's convenience, the list can be sorted alphabetically (A-Z) or reverse alphabetically (Z-A).

The code looks like this:

{% gist  https://gist.github.com/thawkin3/6a0a853e6697d449fa6ed47f19b6c673 %}

Now, let's see what happens when we try checking a few of the checkboxes and then sorting our list.

![Unstable keys lead to an unexpected user experience](https://dev-to-uploads.s3.amazonaws.com/i/i92v6bon13c90fyzzqyd.gif)
<figcaption>Unstable keys lead to an unexpected user experience</figcaption>

Oh no! The checked checkboxes don't move with the student names correctly! First Adam and John are present, but then after the list is sorted Z-A, only the two Tylers are present!

Because we used an index as our key, the first two items in the list stayed checked, even though the actual item data and text content had changed.

---

To fix this, we need to use a stable identifier as our key. I'm going to modify our data so that each student has a unique ID that we can use.

Our code now looks like this:

{% gist https://gist.github.com/thawkin3/404a54917b4a15dfb158193988b8a6bf %}

Note that our array of strings is now an array of objects, with each student having a name and an ID. The ID is used as the key on line 37.

Here's the resulting user experience:

![Checkboxes stay with the right names now](https://dev-to-uploads.s3.amazonaws.com/i/175r49mpzzlns7j7jd4q.gif)
<figcaption>Checkboxes stay with the right names now</figcaption>

Neat! Now the checkboxes move with their accompanying student names when the list is sorted. Adam and John are marked present no matter how the list is sorted.

---

## Conclusion

There you have it. Three mistakes with React keys and three ways to use them correctly. Happy coding!
