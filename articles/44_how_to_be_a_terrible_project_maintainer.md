# How to Be a Terrible Project Maintainer

Hey, you! Yeah, you. Are you a software engineer? Do you have ownership over a particular repository at your company? Do you want to ensure that working with your repo is a constant source of frustration for your fellow developers? Great! Then read on for these tips on how to be a terrible project maintainer.

---

## Don't write good documentation

Especially on how to do local development or how to contribute to the repo. You want to keep people guessing. Running your project locally should be a puzzle that only the greatest minds can solve. If you want to be even more cryptic, consider including outdated or incorrect instructions that reference non-existent scripts meant to run the app.

---

## Don't write tests

You know your code works – at least you think it does. It works on your machine anyway. Besides, tests take too much time to write. And if other people contribute code to your repo and introduce bugs, well, that's their problem. They should be more careful.

If you do insist on writing tests, don't require them to pass. You wouldn't want a few small failing tests to hold up your code from being merged, would you? After all, we need to get these new features out to our customers.

I haven't sold you on this idea yet? I see. Well, if you absolutely must have tests included as part of a continuous integration pipeline, at least make a few of your tests flaky. It should be exciting waiting to see if re-triggering the pipeline for the fourth time will make the tests pass. When the pipeline finally passes, it'll feel like you've won the lottery.

---

## Don't keep your project dependencies up to date

Sure, you may fall several major versions behind on critical packages your app relies on, but what's the big deal? It's not like new functionality or bug fixes or security patches are included in these new versions. Package maintainers just publish new versions as part of a power trip to make everyone download a new version and boost their package's download stats on npm.

---

## Use inconsistent formatting

Code formatters limit your freedom of expression. If you felt like putting a semicolon on line 8 but not down on line 11, so be it. Use four spaces for tabs in some files but two spaces for tabs in others. Even throw in a few real tab characters here and there to keep people on their toes. Code doesn't need to be pretty. This is computer science, not computer art.

---

## Be slow to respond to questions

Why are people even asking you questions? Can't they figure this out on their own? The app makes perfect sense to you, and it should make sense to everyone else too. And if someone asks you to document something, refer them back to the first item on this list as you exasperatedly explain the architecture of the app to the ninth person who's asked you this week. Why can't these people remember anything?

---

## Be slow to review merge requests

With how busy you are, your coworkers should be grateful you're even taking the time to review their code at all. Contributing to your repo is an honor, and it's one well worth waiting for. When you do finally get around to reviewing someone else's code, be sure to leave vague criticism that attacks the developer rather than the code. They should know that they will never measure up to your astounding intellect. As an added bonus, make sure the code review goes through multiple rounds of feedback, each several days apart.

In fact, you probably shouldn't even accept merge requests at all. This is your project, and you don't want other engineers polluting your codebase with their poorly thought out code. Be exceptionally clear that contributions are *not* welcome.

---

## Good luck

Well, good luck out there. It's tough being the very worst, but with some practice, you too can become a terrible project maintainer.
