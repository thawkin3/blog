# In Defense of Clean Code: 100+ pieces of timeless advice from Uncle Bob

*Clean Code* by Robert C. Martin is the most recommended programming book of all time. Search any list of “top books for software engineers,” and you are almost guaranteed to find this book on the list.

And yet, some people love to hate on *Clean Code*, even going so far as to say that [it’s probably time to stop recommending *Clean Code*](https://qntm.org/clean). I’d argue that sentiments like this are deeply misguided.

Yes, some of the advice in the book is questionable. Yes, some of the content feels dated or hasn’t aged well with time. Yes, some of the examples are confusing. All of this is true. But let’s not be so quick to discount all the *good* advice that the book has to offer!

Completely ignoring a book simply because of a few bad ideas is a perfect example of several cognitive distortions: mental filtering, magnification, and discounting the positive, to name a few.

In fact, Uncle Bob and the other contributing authors have taken care to preemptively handle this concern in the book’s first chapter:

> Many of the recommendations in this book are controversial. You will probably not agree with all of them. You might violently disagree with some of them. That’s fine. We can’t claim final authority. On the other hand, the recommendations in this book are things that we have thought long and hard about. We have learned them through decades of experience and repeated trial and error. So whether you agree or disagree, it would be a shame if you did not see, and respect, our point of view.

So without further ado, let’s consider all the timeless advice that *Clean Code* has to offer! We’ll go through the book, chapter by chapter, summarizing many of the ideas Uncle Bob presents.

---

## Chapter 1: Clean Code

1. The total cost of owning a mess compounds over time.

2. It’s very difficult to rebuild a legacy system from the ground up. Refactoring and incremental improvements are often the better path to take.

3. In messy codebases it can take days or weeks to accomplish tasks that should only take hours.

4. Take the time to go fast.

5. Clean code does one thing well. Bad code tries to do too much.

6. Clean code is well-tested.

7. When reading well-written code, every function does pretty much what you expected.

8. If you disagree with a principle that someone with decades of experience is teaching, you’d do well to at least consider their viewpoint before disregarding it.

9. Code is read far more often than it is written.

10. Code that is easier to read is easier to change.

11. Leave the codebase better than you found it (The Boy Scout Rule).

---

## Chapter 2: Meaningful Names

1. Choose your variable names carefully.

2. Choosing good names is hard.

3. The name of a variable or function should tell you what it is and how it is used.

4. Avoid single character variable names, with the exception of commonly used names like `i` for the counter variable in a loop.

5. Avoid using abbreviation in variable names.

6. Variable names should be pronounceable so that you can talk about them and say them out loud.

7. Use variable names that are easily searchable.

8. Classes and objects should have names that are nouns.

9. Methods and functions should have names that are verbs or verb-noun pairs.

---

## Chapter 3: Functions

1. Functions should be small.

2. Functions should do one thing.

3. Functions should have descriptive names. (Repeated from Chapter 2)

4. Extract code in the body of if/else or switch statements into clearly named functions.

5. Limit the number of arguments a function accepts.

6. If a function needs a lot of configuration arguments, consider combining them into a single configuration options variable.

7. Functions should be pure, meaning that they don’t have side effects and don’t modify their input arguments.

8. A function should be a command or a query, but not both (Command Query Separation).

9. Throw errors and exceptions rather than returning error codes.

10. Extract duplicated code into clearly named functions (Don’t Repeat Yourself).

11. Unit tests make refactoring easier.

---

## Chapter 4: Comments

1. Comments can lie. They can be wrong to begin with, or they can be originally accurate and then become outdated over time as the related code changes.

2. Use comments to describe *why* something is written the way it is, not to explain *what* is happening.

3. Comments can often be avoided by using clearly named variables and extracting sections of code into clearly named functions.

4. Prefix your TODO comments in a consistent manner to make searching for them easier. Revisit and clean up your TODO comments periodically.

5. Don’t use Javadocs just for the sake of using them. Comments that describe what a method does, what arguments it takes, and what it returns are often redundant at best and misleading at worst.

6. Comments should include all the relevant info and context someone reading the comment will need. Don’t be lazy or vague when you write a comment.

7. Journal comments and file author comments are unnecessary due to version control and git blame.

8. Don’t comment out dead code. Just delete it. If you think you’ll need the code in the future, that’s what version control is for.

---

## Chapter 5: Formatting

1. As a team, choose a set of rules for formatting your code and then consistently apply those rules. It doesn’t matter so much what rules you agree on, but you do need to come to an agreement.

2. Use an automated code formatter and code linter. Don’t rely on humans to manually catch and correct each formatting error. This is inefficient, unproductive, and a waste of time during code reviews.

3. Add vertical whitespace in your code to visually separate related blocks of code. A single new line between groups is all you need.

4. Small files are easier to read, understand, and navigate than large files.

5. Variables should be declared close to where they’re used. For small functions, this is usually at the top of the function.

6. Even for short functions or if statements, still format them properly rather than writing them on a single line.

---

## Chapter 6: Objects and Data Structures

1. Implementation details in an object should be hidden behind the object’s interface. By providing an interface for consumers of the object to use, you make it easier to refactor the implementation details later on without causing breaking changes. Abstractions make refactoring easier.

2. Any given piece of code should not know about the internals of an object that it’s working with.

3. When working with an object, you should be asking it to perform commands or queries, not asking it about its internals.

---

## Chapter 7: Error Handling

1. Error handling shouldn’t obscure the rest of the code in the module.

2. Throw errors and exceptions rather than returning error codes. (Repeated from Chapter 3)

3. Write tests that force errors to make sure your code handles more than just the happy path.

4. Error messages should be informative, providing all the context someone getting the error message would need in order to effectively troubleshoot.

5. Wrapping third-party APIs in a thin layer of abstraction makes it easier to swap out one library for another in the future.

6. Wrapping third-party APIs in a thin layer of abstraction makes it easier to mock the library during testing.

7. Use the Special Case pattern or Null Object pattern to handle exceptional behavior like when certain data doesn’t exist.

---

## Chapter 8: Boundaries

1. Third-party libraries help you ship your product faster by allowing you to outsource various concerns.

2. Write tests to ensure that your usage of any given third-party library is working properly.

3. Use the Adapter pattern to bridge the gap between a third-party library’s API and the API you wish it had.

4. Wrapping third-party APIs in a thin layer of abstraction makes it easier to swap out one library for another in the future. (Repeated from Chapter 7)

5. Wrapping third-party APIs in a thin layer of abstraction makes it easier to mock the library during testing. (Repeated from Chapter 7)

6. Avoid letting too much of your application know about the particulars of any given third-party library.

7. It is better to depend on something you control than to depend on something you don’t control.

---

## Chapter 9: Unit Tests

1. Test code should be kept as clean as production code (with a few exceptions, usually involving memory or efficiency).

2. As production code changes, test code also changes.

3. Tests help keep your production code flexible and maintainable.

4. Tests enable change by allowing you to refactor with confidence without the fear of unknowingly breaking things.

5. Structure your tests using the Arrange-Act-Assert pattern (also known as Build-Operate-Check, Setup-Exercise-Verify, or Given-When-Then).

6. Use domain-specific functions to make tests easier to write and easier to read.

7. Evaluate a single concept per test.

8. Tests should be fast.

9. Tests should be independent.

10. Tests should be repeatable.

11. Tests should be self-validating.

12. Tests should be written in a timely manner, either shortly before or after the production code is written, not months later.

13. If you let your tests rot, your code will rot too.

---

## Chapter 10: Classes

1. Classes should be small.

2. Classes should be responsible for only one thing and should have only one reason to change (Single Responsibility Principle).

3. If you can’t think of a clear name for a class, it’s probably too big.

4. Your job is not done once you get a piece of code to work. Your next step is to refactor and clean up the code.

5. Using many small classes instead of a few large classes in your app reduces the amount of information a developer needs to understand while working on any given task.

6. Having a good test suite in place allows you to refactor with confidence as you break large classes into smaller classes.

7. Classes should be open for extension but closed for modification (Open-Closed Principle).

8. Interfaces and abstract classes provide seams that make testing easier.

---

## Chapter 11: Systems

1. Use dependency injection to give developers the flexibility to pass any object with a matching interface to another class.

2. Use dependency injection to create object seams in your app to make testing easier.

3. Software systems are not like a building that must be designed up front. They are more like cities that grow and expand over time, adapting to current needs.

4. Delay decision making until the last responsible moment.

5. Use domain-specific language so that domain experts and developers are using the same terminology.

6. Don’t over-complicate your system. Use the simplest thing that works.

---

## Chapter 12: Emergence

1. Systems that aren’t testable aren’t verifiable, and systems that aren’t verifiable should never be deployed.

2. Writing tests leads to better designs because code that is easy to test often uses dependency injection, interfaces, and abstraction.

3. A good test suite eliminates your fear of breaking the app during refactoring.

4. Duplication in your code creates more risk, as there are more places in the code to change and more places in the code for bugs to hide.

5. It’s easy to understand the code you’re currently writing because you’ve been deeply involved in understanding it. It’s not so easy for others to quickly gain that same level of understanding.

6. The majority of the cost of a software project is in long-term maintenance.

7. Tests act as living documentation of how your app should (and does) behave.

8. Don’t move on as soon as you get your code working. Take time to make it cleaner and easier to understand.

9. The next person to read your code in the near future will most likely be you. Be kind to your future self by writing code that is easy to understand.

10. Resist dogma. Embrace pragmatism.

11. It takes decades to get really good at software engineering. You can speed up the learning process by learning from experts around you and by learning commonly used design patterns.

---

## Chapter 13: Concurrency

1. Writing concurrent code is hard.

2. Random bugs and hard-to-reproduce issues are often concurrency issues.

3. Testing does not guarantee that there are no bugs in your application, but it does minimize risk.

4. Learn about common concurrency issues and their possible solutions.

---

## Chapter 14: Successive Refinement

1. Clean code usually doesn’t start out clean. You write a dirty solution first and then refactor it to make it cleaner.

2. It’s a mistake to stop working on the code once you have it “working.” Take some time to make it even better after you have it working.

3. Messes build gradually.

4. If you find yourself in a mess where adding features is too difficult or takes too long, stop writing features and start refactoring.

5. Making incremental changes is often a better choice than rebuilding from scratch.

6. Use test-driven development (TDD) to make a large number of very small changes.

7. Good software design involves a separation of concerns in your code and splitting code into smaller modules, classes, and files.

8. It’s easier to clean up a mess right after you make it than it is to clean it up later.

---

## Chapter 15: JUnit Internals

1. Negative variable names or conditionals are slightly harder to understand than positive ones.

2. Refactoring is an iterative process full of trial and error.

3. Leave the code a little better than you found it (The Boy Scout Rule). (Repeated from Chapter 1)

---

## Chapter 16: Refactoring SerialDate

1. Code reviews and critiques of our code are how we get better, and we should welcome them.

2. First make it work, then make it right.

3. Not every line of code is worth testing.

---

## Chapter 17: Smells and Heuristics

1. Clean code is not a set of rules but rather a system of values that drive the quality of your work.

*[In this chapter, Uncle Bob lists 66 more of his code smells and heuristics, many of which have been covered throughout the rest of the book. Reproducing them here would essentially be copying and pasting the title of each item, so I’ve refrained from doing so. Instead, I’d encourage you to read the book!]*

---

## Conclusion

Let’s finish where we began: *Clean Code* by Robert C. Martin is the most recommended programming book of all time.

There’s a good reason why.
