# "We didn't write tests because we wanted to get our code out faster"

"We didn't write tests because we wanted to get our code out faster."

Those words were uttered by a coworker during a handoff call in which my team would be assuming ownership of a code repo from their team. I cringed.

Not writing tests leaves you open to bugs, makes refactoring more difficult, and actually makes you move slower in the long run.

That's it. That's the post. We can end this article here if you want.

*(For confidentiality purposes, we won't go into where this happened or how long ago this happened.)*

---

Digging in a little deeper, let's look at why those three assertions are true.

---

## Not writing tests leaves you open to bugs

This point should be self-evident. If you have no automated tests ensuring your code is working properly, then you're relying on manual testing to verify your app's behavior. Manual testing is tedious, and as your app grows, it becomes unwieldy for one person to be able to retain a mental model of the entire app's architecture and understand how any given change in one part of the app will affect other parts of the app 100% of the time.

Untested code is buggy code. Even if the code is bug-free for now, it won't remain that way for long.

---

## Not writing tests makes refactoring more difficult

Tests provide the support you need during refactoring to ensure that the app still works correctly after your changes. Without tests in place, you really can't be confident that your refactors are safe. You're back to the responsibility of thoroughly manually testing the entire app to ensure no regressions have occurred. Wouldn't it be nicer to have automated tests do this work for you?

---

## Not writing tests makes you move slower in the long run

The larger your app becomes, the more surface area there is for bugs to hide, and the more time you must spend manually validating your changes. During manual testing it's inevitable that things will be missed. Bugs will be released into production, and your team will spend their days fighting fires and fixing bugs rather than working on exciting new features.

---

## There is a better way

If the scenario I've described doesn't sound appealing to you, good! Luckily, there is a better way. Write tests for your code –– always. And if your repo doesn't currently have tests, take steps to fix that now. Ensure that every new merge request includes tests, and slowly but surely increase your repo's test coverage as you go. You'll be glad you did.
