# Getting Rid of QA Engineers is a Mistake

I've noticed a growing trend in software companies over the last few years in which upper management declares that quality assurance roles will no longer exist at the company. Then, the entire QA department is either let go or transitioned into traditional software development roles. Everyone is now just a software engineer, and everyone owns quality.

This is a mistake.

I'd argue that the idea that everyone owns quality, while positive in intent, is deeply misguided for one big reason: **when everyone owns quality, no one does.**

---

Yes, every software engineer should take pride in their work. Yes, every software engineer should carefully test their code, both manually and with automated tests. Yes, every software engineer should consider more than just the happy path and look for edge cases and instances where things could go wrong. These things are all well and good.

But, software engineers and QA engineers simply have different mindsets. Software engineers want to build things. QA engineers want to see how they can break things. Software engineers have pressure to deliver new features. QA engineers have pressure to ensure the app works smoothly for customers.

So why not just train software engineers to have a quality mindset, you say?Sure, that's fine. Just know that transitioning between these two mindsets throughout the day is no small feat. Managing your time between software engineering and QA tasks is no small feat either.

---

There is also a difference in skillset. Software engineers are typically expected to have expertise in one or two programming languages and frameworks within the company's tech stack and to be good at building products with those tools. QA engineers are expected to have expertise in testing frameworks like Selenium, Pytest, Cypress, or Playwright and to be good at writing airtight test suites with those tools.

So why not just train software engineers on another testing framework, you say? Sure, that's fine. Just know that you're adding that to the list of everything else software engineers are expected to do. We must all be full-stack engineers, experts in both frontend and backend development, security, accessibility, localization, DevOps, and now testing.

How does the saying go? "A jack of all trades is a master of none."

Or do you prefer the alternate ending? "A jack of all trades is a master of none, but oftentimes better than a master of one."

Scholars have debated the origins of the phrase but generally accept that the second line was added later. Regardless, there is no denying that in the ever-expanding and hyper-specialized field of software engineering, one simply cannot become a master of all things. Yes, we can teach software engineers to use additional testing frameworks, but their expertise will never be as deep as someone's who focuses exclusively on QA.

---

The biggest challenge comes when testing end-to-end pieces of functionality that are built by multiple teams. Without a QA organization, questions of ownership arise.

Who's in charge of making sure this functionality gets tested? Who owns updating and maintaining the shared test suite? Who determines what level of coverage is acceptable, and who enforces that requirement? What about the actual test suite infrastructure and pipeline maintenance? Who owns that?

In general, the popular guidance is to create self-organizing cross-functional teams that can work in independent silos to build, test, deploy, and maintain their own areas of responsibility. This is good advice.

But the advice starts to break down once you reach any boundaries that need to be tested between teams. Without anyone formally in charge of QA, shared test suites and end-to-end tests will decay. Standards will stop being enforced. Coverage will decrease. Quality will decrease. Entropy is inevitable.

I've seen it happen. Twice.

Getting rid of all your QA engineers is a mistake.
