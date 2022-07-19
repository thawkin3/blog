# You Can’t Keep Building on a Broken System: Why Managing Technical Debt is So Important

Every software company has some amount of **technical debt**, which is additional development work created in the long-term by taking a shortcut in the short-term to get code out the door. Technical debt can take the form of poor design decisions, much-needed refactorings, technology upgrades, and outstanding bugs.

Much like taking on actual financial debt, technical debt buys you a little more time now but must be paid back with interest in the future. Also like financial debt, there are [wise and unwise ways](https://martinfowler.com/bliki/TechnicalDebtQuadrant.html) to take on and manage technical debt.

In this article, I’d like to discuss why it’s important to pay off your technical debt quickly and the consequences of failing to do so. I’ll also address some of the common obstacles that software engineers face when trying to prioritize time to work on technical debt.

It should be noted that I am a software engineer by profession, so this article will reflect that viewpoint, but this article is for everyone.

---

## Why Reduce Technical Debt?

Let’s tackle the most essential question first: **Why should you reduce your organization’s technical debt?** After all, your app is working well enough, and you’ve gotten by just fine so far, and surely you have more important feature work to get done, don’t you?

While that line of reasoning can be tempting to accept, would you take a similar approach to manage your financial debt? Debt comes with interest, and if you are slow to pay off the debt, the interest quickly adds up.

![Piggy bank](https://miro.medium.com/max/1400/0*UjiIpLIkCHlW-sPt)
<figcaption>Photo by Fabian Blank on Unsplash</figcaption>

If you were to make only the minimum payment on your credit card each month, you’d likely end up paying back several times the original amount you borrowed.

Conversely, with a typical 30-year home mortgage, making one extra payment each year takes several years off the total life of the loan and saves you tens of thousands of dollars.

Clearly, it pays to get rid of your financial debt as soon as possible.

---

Yes, paying down your technical debt takes time and effort. And it does take you away from your other feature work. However, paying off your technical debt early is worth it.

Let’s explore some of the benefits of paying down your technical debt:

1. It allows you to move more quickly in the future.
2. It allows you to retain your top engineering talent.
3. It helps you avoid re-writing your whole application every few years.

---

## 1. Move More Quickly

A common pushback to tackling technical debt is that “we don’t have time.” What if I told you that managing your technical debt would *save* you time?

Every time I hear someone tell me that they don’t have time to pay down technical debt, I’m reminded of this comic:

![Pulling a cart with square wheels](https://miro.medium.com/max/1400/1*kYGhmDdLgGYUszH1LluvqQ.png)
<figcaption>Cartoon by Scott Simmerman</figcaption>

Obviously it’s much easier and faster to pull a cart that has round wheels than to pull a cart that has square wheels. Yet, figuratively, organizations can find themselves pulling carts with square wheels without realizing it.

Martin Fowler illustrates the benefits of addressing technical debt and keeping the quality of your code high in his article [“Is High-Quality Software Worth the Cost?”](https://martinfowler.com/articles/is-quality-worth-cost.html).

In it, he includes the following graphic:

![Speed of delivery over time](https://miro.medium.com/max/1400/1*e06yIuLHBJ5I8QqIM5tZ6w.png)
<figcaption>Courtesy of Martin Fowler</figcaption>

The brown line represents the trajectory of a software company that ignores technical debt and allows the quality of their codebase to fall. While they may ship features more quickly in the beginning, their speed of delivery slows down more and more over time.

The poor design decisions, open bugs, outdated technology, or whatever it may be, eventually brings feature development to a halt as it becomes increasingly difficult to work with the complex and buggy codebase.

On the other hand, the blue line represents the trajectory of a software company that understands the importance of maintaining high code quality, addressing technical debt, and fixing bugs early.

While they do have a slower start in shipping new features in the beginning, after a short time they start to outpace the first company in delivering value to the customer. That’s because the care they take in maintaining the health of their codebase prevents them from getting bogged down in issues over time.

---

## 2. Retain Top Engineering Talent

This is more anecdotal evidence than a solid fact, but in my experience, companies that fail to address technical debt lose their best engineers. No one likes to work with a buggy system or in a project where every day it’s a struggle to make progress on their assigned work. Very few engineers enjoy working with “legacy code” or working with technologies that are decades old.

![Frustrated engineer](https://miro.medium.com/max/1400/0*cNXvaqrVaM0pS8YA)
<figcaption>Photo by Tim Gouw on Unsplash</figcaption>

So, when the developer experience goes downhill, what do software engineers do? Those who are able to find better jobs go somewhere else. In other words, your organization’s top talent leaves while those who are unable to find other work, whether that be from lack of in-demand skills or some other reason, stay.

So, if you want to retain your top engineering talent, your organization needs to show that it values high-quality code as much as its engineers do.

---

## 3. Avoid Re-Writing Your App From Scratch

Why do companies re-write their apps from scratch? Sometimes it’s a very valid reason, like needing to get off of an outdated tech stack from 20 years ago while they modernize their platform.

But oftentimes, apps get re-written from the ground up due to the issues in the speed of development we discussed earlier in the Martin Fowler graphic above. The developer experience becomes so painful that eventually your engineers throw up their hands in frustration and say, “I can’t work with this. We need to start over.” After many long conversations, the organization eventually agrees and decides to re-write the app from scratch and spends years and millions of dollars doing so.

![Etch A Sketch](https://miro.medium.com/max/1400/1*fgvQ7_DRpSshru5mYkpH9A.jpeg)
<figcaption>Photo courtesy of Amazon.com</figcaption>

I’ll admit it’s fun to start over on a new project. You get to choose your favorite programming languages and tools, and you get to design everything “the right way” this time. (Although in the future, other developers may feel the same way about your code that you feel about the current legacy app...)

But could this situation have been avoided? My answer is that, yes, you likely could have avoided a complete re-write had you prioritized addressing your technical debt and kept things from spiraling out of control to begin with.

---

## Why Technical Debt Doesn’t Get Addressed

Hopefully, by now you agree that reducing technical debt as quickly as possible is a good thing. **So why is it so common that technical debt doesn’t get addressed?**

I can think of a few reasons:

1. Engineers think feature work is more important.
2. Engineers think tackling technical debt is not their problem.
3. Engineers are afraid to push back on product and engineering management.
4. Engineers are unable to effectively persuade product and engineering management.

Let’s look at each of these reasons and explore ways to respond.

---

## 1. Feature Work is More Important

Yes, releasing new features is important. Without features, there is no product. But you can’t just develop features 100% of the time.

When I hear someone tell me feature work is more important and we can’t prioritize tackling technical debt, I have to ask myself, is it really true right now that feature work is more important? Would our customers really like another half-baked feature full of bugs, delivered on top of an already slow and buggy application?

I’m being a bit facetious here, but there’s some truth to that question. If we’re talking about delivering value to our customers, let’s be sure to deliver a good user experience, which means making sure the app is performant and doesn’t have bugs that frustrate the customer or block the customer from using the app to its intended purpose.

A good compromise that allows you to work on both features and technical debt is to spend somewhere around 70–80% of your time each sprint developing new features and 20–30% of your time each sprint fixing bugs and tackling technical debt.

![Shaking hands](https://miro.medium.com/max/1400/0*mFJddYfKNAdjiV3p)
<figcaption>Photo by Sebastian Herrmann on Unsplash</figcaption>

---

## 2. Technical Debt is Not My Problem

If an engineer feels this way, this is likely a symptom of an underlying engineering culture issue. I want to work in an environment in which everyone feels a sense of pride and ownership in the codebase and wants to make it the best it can be. If someone doesn’t share that same value and vision, then I probably don’t want to work with that person.

The solution here might be either 1) to try to influence the engineering culture in a more positive direction, or 2) for you to leave the company so that you can find an organization with a better cultural fit.

---

## 3. Fear of Pushing Back

I see this fear most in developers who are newer in their careers. They may feel the internal conflict of knowing that there are some significant pieces of technical debt that need to be addressed, but they’re afraid to voice that concern. Perhaps it’s because they think that speaking out will reflect poorly on them or that it may jeopardize their job.

![Worried employee looking over her shoulder](https://miro.medium.com/max/1400/0*QAjygQHepzYyJlRi)
<figcaption>Photo by Icons8 Team on Unsplash</figcaption>

It’s important to remember though that as software engineers, we have the greatest insight into the amount of technical debt the company is facing at any given time. I’d argue that it’s actually our *obligation* to voice these concerns and to push back on product management or engineering management when technical debt continues to be de-prioritized or ignored.

It’s also been my experience that pushing back earns you nothing but respect, as long as you voice your concerns respectfully.

---

## 4. Inability to Persuade

This leads me to the fourth, and perhaps most crucial, problem. Let’s say that as an engineer, you do agree that tackling technical debt is important, and you have brought this up with product management and engineering management, but they haven’t listened.

![Unproductive conversation](https://miro.medium.com/max/1400/0*tlfdEKsLz6ZWXPr7)
<figcaption>Photo by Mimi Thian on Unsplash</figcaption>

Rather than feeling helpless in this scenario, it may just be that you need to communicate your concerns more clearly. If all you’re saying is that “we need to address this issue” but you’re not offering any clear explanation or reason as to why then you’re not communicating effectively.

You should provide context as to why the technical debt you’d like to prioritize is important and what benefits this would bring to the business. It’s especially important to explain things in terms that someone who is not an engineer can relate to and understand.

---

For instance:

“We really need to take some time writing tests to increase our code coverage in this repo. Due to our lack of tests, bugs are frequently introduced, and we have to spend time going back and fixing these problems after they’re already in production. Getting our test suite in a better state will give us more confidence in our code and will make sure that our customers have a better experience using our app too.”

---

Or maybe:

“We have 30 open customer-facing bugs right now. These are creating a bad experience for our customers and have even caused one client to cancel their contract with us. I really think we need to spend this sprint fixing these bugs rather than working on this new feature.”

---

Or even:

“Our continuous integration pipeline is taking too long. We have to wait over an hour to get feedback to see if our build passes, and this delayed feedback loop is making it take a really long time to finish tasks. I really think we need to spend some time optimizing our build to make it finish faster so that all of our engineers can move faster. Just think of how much more we could get done if our build finished ten minutes faster each time it was run, spread across 100 engineers each creating new builds five times per day. That’s over 83 hours (10 * 100 * 5 / 60) of saved collective development time each day!”

---

## Conclusion

Taking on technical debt is inevitable. But, how an organization chooses to handle its technical debt can make or break a company. As an engineer, I hope you feel inspired and empowered to address the technical debt you’re currently facing in your own role. Doing so will make your company a better place to work and will create a better experience for your product’s users as well.
