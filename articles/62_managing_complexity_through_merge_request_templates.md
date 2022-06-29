# Managing Complexity Through Merge Request Templates

**All code repos should use merge request templates.**

My goal in this article is to convince you that the above statement is true. Let’s dig in!

---

## The professional world is complex

Let’s start with a little background for context. The professional world is complex. Take a look at just about any industry, and in it you’ll find complexity. Let’s examine, for instance, the fields of medicine, aviation, and construction. These fields may seem vastly different, but they also share many similarities.

First, each field contains too much information for any one person to know. Doctors specialize and super-specialize to occupy a specific niche. A doctor might be a heart surgeon or a pediatrician or an ear, nose, and throat doctor. Airplane pilots may be familiar with the controls in just a couple models of airplanes even though there are thousands of different models of airplanes in the world. In construction, there are architects, structural engineers, plumbers, woodworkers, heating and air conditioning specialists, electricians, and more. No one person knows everything there is to know about their broad field.

Second, each field involves time-sensitive work. A surgeon may need to respond to a flatlining patient during an operation gone wrong. A pilot may need to quickly respond to an emergency warning or flashing signal on his dashboard. And contractors working on a massive project must coordinate the work of many different teams to ensure the right things are done at the right time.

Third, each field requires some degree of training and skill. It takes time to become good at any of these professions.

---

## A fourth commonality

There is also a fourth thing that each of these fields has in common: they all use checklists.

Why? Because they work.

Let’s take a look at an example in each field.

---

## Aviation: Boeing’s Model 299

In 1935 a military flight competition was held. The outcome of the competition would determine which airplane manufacturers would win large government contracts, and Boeing’s Model 299 bomber was favored to win. It was bigger and faster than the rest of its competition and was the obvious choice.

However, disaster struck that day. The pilot flying the Model 299 crashed, killing two of the five crew members. Reporters deemed the model “to much airplane for one man to fly.” It was simply too complicated for a human being to operate.

In response, Boeing created a pilot’s checklist. This checklist contained incredibly simple items, like making sure that the brakes are released and that the doors are locked. But with this simple checklist, pilots managed to fly the Boeing Model 299 for a combined total of 1.8 million miles without a single accident (*The Checklist Manifesto*, pages 32–34).

Checklists work.

---

## Medicine: Intensive care units

Let’s look at another example. In 2001, researchers at John Hopkins Hospital found that simply having nurses and doctors in intensive care units (ICUs) create their own checklists for what they think needs to be done each day “improved the consistency of care to the point that the average length of patient stay in intensive care dropped by half” (*The Checklist Manifesto*, page 39).

One of the most common causes of infection in ICU patients occurs when their central line gets infected, which can happen if the central line isn’t placed or taken care of properly. In 2006, a study was published which showed that ICUs in Michigan following a central line checklist saw a 66% decrease in central line infection rates within the first three months of instituting the checklist (*The Checklist Manifesto*, page 44).

Checklists work.

---

## Construction: Building plan conflicts

Let’s now take a look at one last example in the field of construction. Project managers during a construction project have a different sort of complexity to wrangle: How do you make sure that the right things get done by the right people at the right time? And how do you manage conflicts between building plans presented by each group of contractors?

Construction project managers have software for detecting conflicts in various construction plans. If a light fixture is supposed to go where a support beam is also supposed to go, they have to work it out. They use software with checklists to make sure the right people talk, everyone is notified, and plans are corrected so that everyone is satisfied.

With this software and these checklists in hand, skyscrapers are built.

Checklists work.

---

## Applying these lessons to the field of software engineering

So, how we can apply these lessons to our own field of work? Well, it turns out, software engineers face a lot of the same challenges that doctors, pilots, and construction crews face.

We also work in a field in which there is too much information for any one person to know. We specialize as frontend engineers, backend engineers, and site reliability engineers. Even within these areas, we super-specialize. One engineer might be well-versed in React but be a novice in Angular. Another engineer might be excellent at improving system-wide performance but know very little about web accessibility.

We also deal with time-sensitive work. In most cases, it may not be a matter of life and death, but we have deadlines to meet and customer contracts to uphold.

Finally, our work requires training and skill. Not every engineer attends a university to obtain a computer science degree, but every engineer spends years learning and perfecting their craft.

So, it would appear that we could benefit from using checklists too.

---

## Merge request templates

Merge request (MR) templates (or pull request templates if you use GitHub), are a form of checklist. MR templates prompt engineers to provide relevant details for the MR.

MR templates ensure that the small things don’t get missed. Just like pilot checklists ensure that the brakes are released and the doors are locked, MR templates ensure that unit tests are written and that other simple yet important items are not forgotten.

MR templates help facilitate conversation and make the code review process more efficient by standardizing it.

Below is what an example MR template might look like:

![Example merge request template](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qsrannvr4u8qequdg11r.png)
<figcaption>Example merge request template</figcaption>

You’ll note that the MR template begins with its own checklist. We want to ensure that the engineer submitting the code has written unit tests and has reviewed the code themselves before asking for a code review. This particular repo contains frontend code, so many of the next items deal with frontend concerns. We want to ensure that the changes are cross-browser compatible, that the code is accessible, and that any user-facing text is translated using our localization service. Finally, we include a reminder to add or update documentation as needed.

Next we ask the developer to include a brief summary of what changes are being made in this MR. This helps provide the context for the engineer reviewing the code.

Next we have a section to include links to the ticket in your ticket-tracking tool like Jira or Workfront. That way anyone viewing this MR can take a look at the original work request to see even more context or backstory.

After that is a section for a test plan, which includes steps for how someone could manually verify the changes that you’re making. In other words, this provides a very simple way for someone to verify that the code is doing what the engineer says it should be doing.

Last, we include a section for screenshots or videos to demonstrate the code in action, if that would be relevant or helpful for the MR.

---

## Example scenarios in which merge request templates are useful

Now, if that seems like a lot of information to provide for every MR, rest assured that it only takes about two minutes to fill out. Let’s look at some of the benefits of using an MR template.

When reviewing the code, nothing is more frustrating as a reviewer than when you are given an MR with absolutely no context. What is this code trying to solve? Is it fixing a bug? Adding a new feature? Why was this written in the first place? MR templates help provide that much-needed context.

Even more important, MR templates help establish a standard of baseline performance. When you include an item for “I’ve written unit tests” in your MR template, that sets the expectation that all MRs should have unit tests. It also serves as a reminder for important items to check. You’d be surprised at how often routine things are missed even by seasoned professionals.

As a second scenario, think about when you as a developer are working on a bug fix somewhere in your application. This part of the code may not have been touched for months or years, and you may not have much context. If you're like me, one of the first things you might do is open up a "git blame" tool in your IDE to see when changes were last made and by whom. You can then find the past MRs and take a look at what changes were made and why. Imagine your dismay when you pull up an old MR and there's no context provided! On the flip side, imagine your appreciation for a nicely filled out MR template that gives you exactly the history and context you were looking for.

---

## My invitation to you

At this point, I think I’ll rest my case. But before we wrap up, I’d like to leave you with an invitation: Add a merge request template to your repos at work. You’ll be amazed at how helpful they really are.

The exact contents of your MR template may vary from what was presented here. Feel free to adapt this example to your own needs. Just remember to keep your MR template short, precise, and practical.

---

## Addressing possible concerns

Finally, if you feel like MR templates may not be worth your time, remember that even some surgeons are offended by the suggestion that they need a checklist in order to do their jobs well. But it turns out, they do. And all of us do too.

---

## Conclusion

Checklists work. Merge request templates work. They help create a baseline of higher performance and will help lift the quality of your codebase. In the end though, just checking off boxes is not the ultimate goal — embracing a culture of teamwork and discipline is.
