# Automated Accessibility Testing

As more and more companies focus on making their apps accessible, a question that often comes up is “How do we make sure we don’t let things slip?” We’ve done all this work to remediate our app, but how do we make sure we don’t dig ourselves back into a hole six months later and end up in a similar situation that we are in now? How do we keep things accessible?

There are a few solutions. The first is education and training. Everyone in the company needs to be an accessibility advocate and understand accessibility best practices. The second is better organizational processes. Companies should include accessibility audits in natural checkpoints throughout the software development lifecycle, like when UX provides design mockups to the engineering team or when the engineering team is code complete on a new feature. The third is automated testing, and that’s what I’d like to focus on today.

---

## Disclaimer

As a brief disclaimer before we begin, I want to emphasize that when it comes to accessibility there is no adequate alternative to good manual testing with a mouse, keyboard, and screen reader. Ask any accessibility consultant, and they will tell you the same thing.

The hangup is that engineers are often dissatisfied with that answer. Engineers like to automate everything. Manual testing sounds tedious, and it doesn’t scale.

And, you would be right. Those are fair concerns. So, let’s take a look at some of the automated tools we have available and examine their benefits as well as their drawbacks.

---

## Automated Accessibility Tools

There are several good tools that can assist us in our accessibility efforts. Some of the common tools that I’ve used are ESLint plugins like [eslint-plugin-jsx-a11y](https://www.npmjs.com/package/eslint-plugin-jsx-a11y), tools from Deque like the [axe DevTools Chrome extension](https://chrome.google.com/webstore/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd) or the [axe Monitor](https://www.deque.com/axe/monitor/) web crawler, and CI tools like [Google Lighthouse](https://github.com/GoogleChrome/lighthouse-ci/blob/main/docs/getting-started.md) or [GitLab CI/CD with Pa11y](https://docs.gitlab.com/ee/user/project/merge_requests/accessibility_testing.html).

The thing to know about all of these tools is that they are all static analysis checkers.

Static analysis checkers examine the code, whether that be the JavaScript source code or the built HTML on the web page, and then report possible violations based on a set of rules.

And, here’s the kicker: these static analysis checkers can only catch about 10–30% of accessibility issues in your app.

Yes, you read that number correctly. 10–30%. Why is that number so low? To get a better understanding of why, we should look at the kinds of things that these tools are good at identifying as well as the things that they are bad at identifying.

---

## What Static Analysis Checkers Are Good at Identifying

Static analysis checkers are good at identifying **invalid usages of HTML**. For instance, they’ll catch when you use an anchor tag (`<a>`) without an `href` attribute. Maybe you’ve put a click handler on the anchor tag to make it function more like a button, which would be invalid. The static analysis checker would report a violation and let you know that you should either use a `<button>` element with a click handler or else provide a valid `href` attribute for your `<a>` element if you really intended for it to be a link.

As another example, static analysis checkers can identify when you’ve used heading elements (`<h1>` through `<h6>`) in the wrong order. The rule is that heading levels can only increase by one, so you can’t have an `<h1>` element followed by an `<h4>` element. If the static analysis checker sees this in your app, it will report a violation.

As a third example, a static analysis checker could also identify if you incorrectly nest elements in a list. The direct descendants of `<ul>` or `<ol>` elements need to be `<li>` elements, so if you have something like a `<div>` as a child of your `<ul>` or `<ol>` list container, the static analysis checker will complain.

Static analysis checkers are also good at identifying **bad uses of roles and interaction handlers**. A common mistake that I frequently see is someone using a `<div>` element with a click handler rather than a `<button>` element. The problem with this approach alone is that you lose a lot of the functionality that the semantic `<button>` element provides for you out of the box. For instance, the `<button>` element responds to clicks as well as Enter and Space key presses, and it correctly communicates its role (“button”) to screen readers. A static analysis checker looking at your source code (like `eslint-plugin-jsx-a11y`) will report these violations and let you know that if you have a click handler you will also need an accompanying keyboard interaction handler as well as an appropriate role on your element.

Finally, static analysis checkers that run against the rendered app in your browser are also great at catching **color contrast issues** when the color contrast ratio for any given foreground-background combination falls below the required threshold.

As a quick review, these are some of the main things that static analysis checkers are good at identifying:

* Invalid usages of HTML
* Bad use of roles and interaction handlers
* Color contrast issues

---

## What Static Analysis Checkers Are Bad at Identifying

Now, let’s talk about what static analysis checkers are bad at identifying. The short answer is that they will be bad at identifying things that have technically correct source code but that provide a poor user experience for humans.

For example, let’s consider the **tab order** of a page. For most Western languages that read left to right, the tab order on the page will generally go left to right, top to bottom. You might have a column layout on your page, in which case the tab order would go down one column before moving along to the next column. When tabbing through a page, you may sometimes encounter the tab focus moving to an element that you didn’t expect, maybe skipping a few other buttons or just going somewhere completely unrelated. This is disorienting to a human but isn’t something that a static analysis checker would be able to catch. Only a human can tell what tab order makes sense or not.

Another example would be **unhelpful aria-labels**. A static analysis checker will be good at telling you when a label is missing, like for an icon-only button that doesn’t have an aria-label. But a static analysis checker won’t be able to tell you if the aria-label is helpful or makes sense. You could type some nonsense characters as the aria-label value to get past the static analysis checker, but it won’t be helpful to your users.

Third, static analysis checkers can’t identify **keyboard traps**. These are unintentional focus traps where a keyboard-only user cannot escape without using their mouse. You might encounter a keyboard trap when interacting with popup content like a modal or a tooltip or a dropdown menu. A keyboard-only user needs to be able to get into and out of keyboard traps, so if they can’t escape, that’s an issue.

Fourth, static analysis checkers can’t identify when there is **missing alternative functionality** on the page to accommodate all users. Take drag-and-drop behavior for example. Drag-and-drop functionality is inherently inaccessible because it requires the use of the mouse and fine motor control to move the mouse pointer from one specific position to another. This isn’t a problem on its own, but you do need to provide alternative methods to accomplish the same task. So for something like using drag-and-drop functionality to reorder items in a list, you might also provide keyboard controls to allow keyboard-only users to press the Enter key to activate “reorder mode” and then use the arrow keys to move items up or down in the list. Static analysis checkers can’t possibly know when you have sufficient alternative methods for accomplishing any given task.

Fifth, static analysis checkers can’t identify **areas where semantic HTML usages could be improved**. For example, maybe you’ve built a table out of `<div>` elements. Visually it looks like a table, but it won’t have the same navigation behavior for screen reader users, and it won’t be communicated as a table for screen reader users. Static analysis checkers won’t complain because the actual HTML code you’ve written is technically correct without any invalid syntax. The static analysis checker doesn’t know that you’ve intended for this to represent a table.

Similarly, you might have a list of items on the page that are built using paragraph (`<p>`) elements rather than `<ul>`/`<ol>` and `<li>` elements. Or maybe you have a dialog modal but that is missing all the required accompanying modal markup, like `aria-modal="true"`, `role="dialog"`, and an aria-label providing a title for the modal. Screen readers will see technically correct HTML but will not know your intent behind the type of widgets or information you are trying to convey.

Again, as a quick review, these are some of the main things that static analysis checkers are bad at identifying:

* Confusing tab order
* Unhelpful aria-labels
* Keyboard traps
* Missing alternative functionality
* Areas where semantic HTML could be improved

---

## Humans vs. Computers

So, we have a dilemma here. As engineers, we want to be able to automate our accessibility testing. But, the tools that we have at our disposal are not sufficient on their own to give us confidence that our app is in fact accessible. What do we do about this?

The key here is to understand that computers are good at some things, and humans are good at some things.

Computers are fast, don’t need rest, and can execute instructions flawlessly (even when we as humans give them incorrect instructions!).

Humans, on the other hand, are better at higher-level thinking and reasoning. When it comes to auditing the accessibility of our app, we can take a step back and ask, “Does this make sense? Can I use this? Does what we’ve built provide a good user experience?”

So, rather than competing, why not let humans and computers work together to provide the best of both worlds?

As humans, we can decide what criteria are important, what is worth testing, and what the expected behavior should be. We can codify those requirements as automated tests. Computers can then run our tests, and we can include these tests in a continuous integration (CI) pipeline to prevent accessibility regressions in our app.

Let’s look at a couple examples.

---

## Example 1: Modal

For our first example, let’s imagine that we’re building a modal. We can find guidance for how we can build accessible modals through the [Web Content Accessibility Guidelines (WCAG)](https://www.w3.org/TR/WCAG21/) as well as the [WAI-ARIA Authoring Practices](https://www.w3.org/TR/wai-aria-practices-1.1/#dialog_modal) docs.

Our modal criteria will look like this:

* Modal is opened when the trigger button is clicked​
* Modal has appropriate aria markup (`aria-modal="true"`, `role="dialog"`, aria-label)​
* Focus is sent to the first focusable item inside the modal when it opens​
* Focus is trapped inside the modal​
* Modal is closed when the Close button is clicked, and focus is returned to the trigger button​
* Modal is closed when the Escape key is pressed, and focus is returned to the trigger button​
* Modal is closed when anywhere outside the modal is clicked, and focus is returned to the trigger button​

Our next questions would naturally be, at what level should we test this criteria, and how can we write these tests?

When writing accessibility tests, the correct level to test them will almost always be as unit tests. You don’t need to write an end-to-end test to verify that your modal has the correct aria markup. Unit tests will suffice.

So how can we write unit tests for these criteria? Using the same tools you already do for your other unit tests. I primarily work in React, so my tools of choice are [Jest](https://jestjs.io/) as my test framework with [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) and the [User Event](https://testing-library.com/docs/user-event/intro) library as my test libraries.

React Testing Library is great for rendering and interacting with components. User Event is a companion library that helps make testing user interactions even simpler. It’s great for testing things like tab behavior or firing events that the document is listening for.

---

## Example 2: Clickable Div Button

Let’s consider another example. We discussed clickable `<div>` elements earlier in this article and some of the functionality you have to re-implement on your own if you choose to use an element other than the semantic `<button>` element.

Our acceptance criteria for this button will look like this:

* Click handler is called on click​
* Click handler is called on Enter keypress​
* Click handler is called on Space keypress​
* Click handler is *not* called on any other keypress​
* Element has `role="button"` attribute

So, where and how can we test for these criteria? Your answer should be the same as last time. We can write unit tests for this using our test framework and libraries of choice.

---

## Key Takeaways

We’ve covered a lot of info here today. If there is anything you remember from this article, I hope it will be these points:

1. Static analysis checkers on their own are not sufficient tools to ensure that your app is accessible.
2. It’s important to do manual exploratory testing to validate that humans can actually use your app with a mouse, keyboard, and/or screen reader.
3. We can take those findings from our manual testing, fix the bugs, and write automated tests to prevent regressions.

Thanks for reading, and thank you for being an accessibility advocate.
