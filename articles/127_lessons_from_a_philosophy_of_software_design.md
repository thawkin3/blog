# Lessons from A Philosophy of Software Design

![A Philosophy of Software Design by John Ousterhout](https://cdn-images-1.medium.com/max/8064/1*IZaoXjwHGPZ2dn67BERA0A.jpeg)_A Philosophy of Software Design by John Ousterhout_

I recently finished reading _A Philosophy of Software Design_ by John Ousterhout. If you enjoyed _Clean Code_ or _The Pragmatic Programmer_, you’ll likely enjoy this one too. It’s very similar to those books, has some overlapping content, and focuses primarily on what separates good software design from bad software design.

When I read tech books, I always take notes and highlight passages that resonate with me. It helps me retain the information I found meaningful. I’ve reproduced most of my notes below, and I hope that you’ll find something that resonates with you as well.

Enjoy!

## On coding advice

When you disagree with someone’s coding advice (something in this book or in _Clean Code_), try to understand why you disagree with it.

“Every rule has its exceptions, and every principle has its limits. If you take any design idea to its extreme, you’ll probably end up in a bad place.”

## Complexity

“Complexity is more apparent to readers than writers. If you write a piece of code and it seems simple to you, but other people think it is complex, then it is complex. When you find yourself in situations like this, it’s worth probing other developers to find out why the code seems complex to them; there are probably some interesting lessons to learn from the disconnect between your opinion and theirs. Your job as a developer is not just to create code that you can work with easily, but to create code that others can also work with easily.”

Symptoms of complexity

- **Change amplification**: Seemingly simple changes require code modifications in many different places
- **Cognitive load**: How much a developer needs to know in order to complete a task
- **Unknown unknowns**: It’s not obvious which pieces of code must be modified to complete a task

Good design should be obvious.

## Modules, interfaces, and implementations

Modules should be deep. Complex implementations should be hidden behind simple interfaces with sensible defaults that work for the most common use cases.

“The best features are the ones you get without even knowing they exist.”

“As a module developer, you should strive to make life as easy as possible for the users of your module, even if it means extra work for you.”

“Reduce the number of places where exceptions must be handled.”

## Design approaches

“Designing software is hard, so it’s unlikely that your first thoughts about how to structure a module or system will produce the best design. You’ll end up with a much better result if you consider multiple options for each major design decision.”

“Try to pick approaches that are radically different from each other; you’ll learn more that way. Even if you are certain that there is only one reasonable approach, consider a second design anyway, no matter how bad you think it will be. It will be instructive to think about the weaknesses of that design and contrast them with the features of other designs.”

## Comments and documentation

Comments should be used to explain things that can’t be expressed in code, like the rationale for why a certain decision was made.

“The biggest challenge with cross-module documentation is finding a place to put it where it will naturally be discovered by developers.”

## Consistency, conventions, and coding styles

“Consistency creates cognitive leverage: once you have learned how something is done in one place, you can use that knowledge to immediately understand other places that use that same approach.”

Document coding conventions and coding style preferences. Automate and enforce what you can, like with ESLint and Prettier. When joining a new company or team, stick to existing conventions that are already in place. Refactoring or re-writing all the code to use a new convention is rarely worth it.

## Tactical vs. strategic programming; also Agile

Tactical vs. strategic programming. Don’t just focus on getting a small change working if it makes future changes more difficult.

“One of the risks of agile development is that it can lead to tactical programming. Agile development tends to focus developers on features, not abstractions, and it encourages developers to put off design decisions in order to produce working software as soon as possible. For example, some agile practitioners argue that you shouldn’t implement general-purpose mechanisms right away; implement a minimal special-purpose mechanism to start with, and refactor into something more generic later, once you know that it’s needed. Although these arguments make sense to a degree, they are against an investment approach, and they encourage a more tactical style of programming. This can result in rapid accumulation of complexity.”

## Importance of tests

“Tests, particularly unit tests, play an important role in software design because they facilitate refactoring. Without a test suite, it’s dangerous to make major structural changes to a system. There’s no easy way to find bugs, so it’s likely that bugs will go undetected until the new code is deployed, where they are much more expensive to find and fix. As a result, developers avoid refactoring in systems without good test suites; they try and minimize the number of code changes for each new feature or bug fix, which means that complexity accumulates and design mistakes don’t get corrected.

“With a good set of tests, developers can be more confident when refactoring because the test suite will find most bugs that are introduced. This encourages developers to make structural improvements to a system, which results in a better design. Unit tests are particularly valuable: they provide a higher degree of code coverage than system tests, so they are more likely to uncover any bugs.”

## Performance improvements

Before making performance changes, measure what you need in order to create benchmarks for the existing performance. Our intuitions of what changes will improve performance are sometimes wrong, having either no meaningful impact or actually making performance worse.

Design code around the critical path. Performance improvements in other places that are not the biggest bottleneck will still result in a bottleneck when you hit that part of the path.

## Conclusion and the benefits of good design

“The investments you make in good design will pay off quickly. The modules you defined carefully at the beginning of a project will save you time later as you reuse them over and over. The clear documentation that you wrote six months ago will save you time when you return to the code to add a new feature. The time you spent honing your design skills will also ay for itself: as your skills and experience grow, you will find that you can produce good designs more and more quickly. Good design doesn’t really take that much longer than quick-and-dirty design, once you know how.

“The reward for being a good designer is that you get to spend a larger fraction of your time in the design phase, which is fun. Poor designers spend most of their time chasing bugs in complicated and brittle code. If you improve your design skills, not only will you produce higher quality software more quickly, but the software development process will be more enjoyable.”
