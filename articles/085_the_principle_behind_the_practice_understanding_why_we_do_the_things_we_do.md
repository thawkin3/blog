# The Principle Behind the Practice: Understanding why we do the things we do

There are principles and there are practices. A principle is a universal truth that is applicable everywhere. A practice is a specific implementation of a principle and can vary based on the situation.

Understanding the difference between the two is an important step in becoming wise. When you understand the principle behind the practice, you understand why we do the things we do. You understand the rules and also when to break the rules. Understanding the principle behind the practice helps you to be more flexible and apply good judgment in your day-to-day decisions.

Let’s explore some principles and practices in the world of software engineering.

---

## Testing and Verification

It’s generally accepted as a good practice to write tests for your code. If we agree that we should write tests, some followup questions might be:

1. How much code coverage is enough?

2. At what level should I write my tests? Unit, integration, or end-to-end?

3. When is it ok to *not* write tests?

These are all good questions. Industry experts have voiced their opinions, I have my opinions, and I’m sure you have opinions of your own as well. As a general rule of thumb, my answers to these three questions would be:

1. [Shoot for 100% code coverage](https://dev.to/thawkin3/clean-code-with-unit-tests-tips-and-tricks-for-keeping-your-test-suites-clean-483l), but don’t be fanatical about it. Sometimes getting that last 5% of coverage is not worth the return on investment. But if you’re *not* going to test something, you should have a pretty good reason justifying why not.

2. [Push your tests as low down the testing pyramid as you can](https://dev.to/thawkin3/when-to-write-end-to-end-tests-48h0). Most things can be tested at a unit and integration test level, and these tests have the benefit of being easy to write and fast to run. Crucial workflows like checking out on an e-commerce site or logging into a web app should be verified with end-to-end tests.

3. [It may be justifiable to not write tests for your code in some instances](https://dev.to/thawkin3/comment/1ljli). For example, if you have a one-line CSS fix, there probably isn’t a good way to test this besides a quick manual verification by looking at the UI. If you’re making a small change in a legacy code repo without any existing test infrastructure and the code in this repo changes very infrequently, you may be ok without writing automated tests.

Now, those are some general practices on testing and my brief thoughts on the matter, but what are the principles behind these practices? Some that come to mind, in no particular order, are:

1. **We should minimize the risk of deployment.** (Writing automated tests, having high test coverage, doing code reviews, and implementing continuous integration pipelines is how we implement this principle.)

2. **We should prefer automating repetitive tasks rather than doing them manually.** (Writing automated tests is how we implement this principle.)

3. **We should prefer fast feedback loops so that we’ll know very quickly when we’ve done something wrong.** (Writing automated tests, pushing tests down the testing pyramid, and using continuous integration pipelines is how we implement this principle.)

1. **We should maximize value for the business by spending time only on the most important things.** (Choosing not to be fanatical about getting to 100% test coverage or making a judgment call to not write tests for a hard-to-verify small change may be how we implement this principle.)

---

## Feature Branches

A typical strategy for working on a codebase with a large number of developers is to use feature branches. The master branch serves as the source of truth for the codebase, and developers create feature branches off of the master branch when writing new code. When the developer is ready to merge his code back into the master branch, he creates a merge request, gets a code review, makes updates based on feedback as needed, and then merges his code.

A good practice when creating merge requests is to make sure that your feature branch is up to date with the latest code from master. If, for example, you have a feature branch that is 10 commits behind master, there is a risk that the changes in those 10 commits may impact the functionality you’re working on.

Generally, if someone else has also touched the same lines of code that you’ve changed, you’ll see a merge conflict and have to pull the latest from master into your branch to resolve those conflicts anyway.

But sometimes the issues can be sneakier than that, where something has changed that impacts the functionality of your feature without actually directly modifying the files you’ve worked on or causing a merge conflict. If you merge your code while still 10 commits behind master, you may be in for a headache with broken functionality or broken tests once both sets of changes are together in the master branch.

Now, is it always a problem if your feature branch is a few commits behind? Not necessarily. If the missing commits don’t touch the same area of code that you’re working on, you should be fine to merge even when you’re a little behind the master branch. Or, if you’re working on a large monolith shared by the entire engineering organization that developers commit to hundreds of times each day, chances are that staying up to date with the very latest code will be a fool’s errand. As long as you’re reasonably up to date when you merge, you should be ok.

So, let’s again look at the principles behind the practice. Why do we want to keep our feature branches up to date with the latest from master? Some principles include:

1. **We should minimize the risk of deployment.** (This is the same as the first principle from the last section. Ensuring that our feature branch also contains the other changes that are currently in the master branch helps us avoid surprises and is one way we can implement this principle.)

2. **We should avoid easily preventable mistakes.** (We don’t want to have to troubleshoot what went wrong with two separate branches that worked well on their own but broke things when used together. We also don’t want to have to fight a fire with broken functionality that was merged into master when we could have easily caught the issue beforehand. Keeping our feature branches up to date is one way we can implement this principle.)

---

## Communication and Slack Etiquette

Let’s look at one more example. This time we’ll discuss a less technical topic but one that’s important for how individuals in organizations work together: communication.

Many companies use Slack for instant messaging. Slack allows you to send messages in public channels, private channels, or direct messages. Messages in public channels are viewable by everyone, messages in private channels are viewable only by those invited to the channel, and direct messages are viewable only by the two people involved in the exchange.

Let’s say you have a question to ask another team about an issue you’re running into. Where do you send your message?

In general, you should [prefer to use public channels over private channels and direct messages](https://dev.to/thawkin3/slack-etiquette-how-to-use-slack-effectively-and-respectfully-at-work-388e).

Posting messages in public channels has several benefits. It allows anyone who’s interested to participate in the conversation. It ensures that the right people are included and reduces the need to have the same conversation multiple times with multiple different groups of people in various private channels and direct messages. It also reduces the burden of answering the question by sharing it with the whole team rather than having only a single individual be aware that there’s a question. As an added bonus, Slack’s search functionality will allow others to search for this same question in the future and find the answers they need.

Now, not every message should be in a public channel. Some conversations may be more private, like discussing hiring decisions or compensation changes. Or they may be more personal in nature. Private channels and direct messages are better suited for these kinds of messages.

So, we should post in public channels whenever possible and appropriate. That’s the practice. Now let’s look at the principles behind the practice:

1. **We should involve the right people in any given conversation.** (If you send a direct message to one person, you may spend some time going from person to person until you find the right person or group to talk to. Or, even if you’ve found someone who can help, the rest of the team may need some visibility into what’s going on. Posting questions in public channels is one way to keep everyone in the loop and implement this principle.)

2. **We should respect our co-workers’ time.** (If you message someone directly, you’ve now placed the responsibility of answering on them alone. This person may be busy, they may be out of the office, or they may not even know the answer. Posting questions in public channels allows team members who are able and available to answer and is one way to implement this principle.)

3. **We should communicate effectively and efficiently.** (I can’t tell you how many times I’ve had the same conversation four times in a row with four different groups of people until we finally get in the same channel to coordinate together. Posting questions in public channels eliminates this redundancy and is one way to implement this principle.)

4. **We should make it easy for people to find relevant information.** (Slack’s search functionality turns Slack into a wonderful informal documentation tool. [Using Slack as well as wiki pages](https://dev.to/thawkin3/death-to-tribal-knowledge-d39) are a couple ways to implement this principle.)

---

## Avoiding the Cargo Cult

We’ve now explored a few practices and the principles behind the practices. If these practices don’t work for you, that’s ok!

The nice thing about practices is that they are by definition flexible in nature. They may vary across companies, countries, or cultures. But if you value and understand the principles behind these practices, you’ll be able to develop practices that are tailored for your unique situation.

On the flip-side, beware those who follow practices without understanding the reasoning behind them. Being dogmatic about software development strategies or design patterns leads to inflexibility, ineffectiveness, and [cargo cult programming](https://en.wikipedia.org/wiki/Cargo_cult_programming). You should always be able to explain why you do what you do.

Practices change, but principles endure. Understanding the difference is an important skill to master.
