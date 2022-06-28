# Tests Are for the Future

Imagine this: You’ve just written some code for a new feature. You’re wrapping up writing some unit tests. As you’re writing your tests, you begin to have an existential crisis. “Why am I writing these tests?” you ask yourself. “I’ve already manually verified that my code works, and I even had another developer test it out themselves as well. What’s the point?”

The answer lies in one crucial realization: **Your tests are for the future.**

Sure, they’re for right now too and can help you catch some edge cases you may have forgotten about while developing your new feature. But tests are mainly for those who will be working on your code in the coming months and years.

Let’s explore how that’s true.

---

## Documentation

Tests serve as documentation for how a certain feature should behave.

Tests are essentially product requirements written as code. Developers who are working with this feature later on may have questions about the intent of the code or how certain scenarios should be handled.

Rather than digging into old JIRA tickets or potentially outdated documentation hosted elsewhere, developers can jump on over to the test suite right in their IDE. By looking at the test cases, they can get a pretty good idea of how the feature works.

![Boy building with Legos and looking at instructions](https://miro.medium.com/max/6000/0*ua6kvWrv8rJtg9ux)
<figcaption>Photo by Kelly Sikkema on Unsplash.</figcaption>

---

## Avoiding Regressions

Tests help you avoid regressions in your codebase as you are developing new features.

While these new features may seem unrelated to some piece of existing code, there’s always the possibility that the two are connected in some way that you’ve missed. A solid test suite will catch areas where you’ve inadvertently affected existing code in a negative way.

Without tests in place, you can never be quite certain that the new code you’re writing plays nicely with the old code without doing some extensive (and tedious) manual testing.

![Cracked glass](https://miro.medium.com/max/5120/0*KL2ZjdvF7PCEUZU4)
<figcaption>Photo by Ivan Vranić on Unsplash.</figcaption>

---

## Refactoring

The most compelling reason for writing tests and why they are for the future is that they allow you to refactor with confidence.

I’m sure you’ve worked somewhere that has a large legacy application the team is supporting. There is something absolutely crucial buried in that legacy application. Maybe it’s your payment processing business logic. Maybe it’s your authentication code.

Whatever it is, it’s essential to the core functionality of your application, and everyone is scared to touch it. It’s old and seems to be working properly, but it’s turned into a huge mess of spaghetti code that no one really understands anymore.

And why is everyone scared to work on it? Because it doesn’t have any tests! And that means that any line of code you change opens up the possibility of breaking something without your knowledge. It means that every small change you make to this piece of functionality needs to be heavily manually tested. It means that you get extremely nervous and cross your fingers as you click the “Submit” button to merge your code.

![Anxious engineer biting pencil](https://miro.medium.com/max/10944/0*abqnXHg6mlec8t8J)
<figcaption>Photo by JESHOOTS.COM on Unsplash.</figcaption>

---

Now, in an alternate reality, imagine that same piece of core functionality, but with a nice test suite that adequately covers the code. When it comes time to refactor the code, you can do so with confidence. Why? Because you’ll know if you’ve broken something. If the tests are all passing now, you make some changes, and now you have a few failures, it’s clear that something isn’t quite right yet.

But that’s not troubling because you’ve caught the errors before you’ve released these new changes to production and you’re able to find the root cause and make sure your refactor is working properly this time.

---

## Conclusion

Tests are for the future. They provide documentation, help you avoid regressions, and allow you to refactor with confidence.

--- 

P.S. If you want to learn more, check out [my piece on test-driven development](https://medium.com/swlh/a-case-for-test-driven-development-8e36df33dc21?source=friends_link&sk=b1756de0b41e2956ac1012faba43aa1e).
