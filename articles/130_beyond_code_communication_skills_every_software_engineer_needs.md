# Beyond Code: Communication Skills Every Software Engineer Needs

Software engineers are not exactly known for their great communication skills. However, despite what you might think, communicating with others is a large part of any software engineering job.

As a software engineer, you likely spend more time communicating than writing code: asking for help, providing help, coordinating projects, submitting code, reviewing code, writing proposals, reviewing proposals, presenting in meetings, having discussions in meetings, submitting bug reports, responding to bug reports, and more.

How well you can communicate will influence how well you’re able to work with others — and how you’re perceived.

Communication is one of those soft skills that will help you progress in your career more than just about anything else, even technical skills. The best software engineers I know are great coders but also excellent at communicating with others.

In my last decade of software engineering, I’ve seen a fair share of both good and bad communication (sometimes it feels like more bad than good). What follows are some tips for how you can communicate more effectively in your day-to-day work.

---

## When creating a pull request

Always write a thorough pull request (PR) description. Include some brief context for what the PR does and why. Add screenshots or videos to show the changes in action. Provide steps for how to test the changes so the reviewer can verify that the code is working as expected.

As the code author, you show competence by being thorough. You also show that you respect your code reviewer’s time by doing those things that will make it easy for them to review your code.

Nothing frustrates me more as a reviewer than being asked to review a PR that has absolutely no context — just a blank PR description or maybe a link to a Jira ticket. What if I don’t know what these changes do? What if I don’t know why these changes are being made? What if I don’t know how to test these changes or where I can even find them in the app? This kind of missing information leads to delays as we go back and forth requesting and gathering more details.

As the code author, you should do as much as possible to reduce the cognitive load on the reviewer. Providing context is just one of many ways you can [make your code reviewer fall in love with you](https://mtlynch.io/code-review-love/).

---

## When reviewing code

Be clear in each comment whether your feedback is an optional “nit” (nitpick) or if it’s blocking feedback. When no explicit distinction is made, it’s sometimes hard for the code author to know if you’re requesting changes, just thinking out loud, or providing alternative approaches just as something to consider for the future.

Be as helpful as you possibly can in your comments. Don’t just say something is “wrong” or needs to be changed. Explain the pitfalls of the current approach and then give a suggested solution if you have one. If the code change is small enough, provide that code snippet in your comment so that the code author can easily apply it. Make it easy for the code author to take your advice.

Time is wasted when both the code author and code reviewer have to spend multiple cycles trying to get clarity on the importance of the requested changes and what an acceptable solution might look like.

Finally, when you’ve requested changes and are finished reviewing, clearly tell the code author that you’re done reviewing for now. Ask them to let you know when the changes have been made and the PR is ready for another review. Time is wasted when one or both of you are sitting around incorrectly thinking that you are waiting on the other person.

---

## When submitting a bug report

Always include steps to reproduce, the expected behavior, the actual behavior you see, and screenshots or videos.

You should strive to make it as easy as possible for someone to understand what issue you’re reporting.

As someone on the receiving end of the bug report, nothing is more frustrating than receiving incomplete information. If all you tell me is “this feature doesn’t work”, I’ll have many followup questions for you: What exactly doesn’t work? Does the app crash? Does it do something but just not what you thought it would do? How are you getting into this state? What are you trying to do? Are there any error messages in the UI or in the console logs?

Gathering missing information that wasn’t included in the bug report leads to more wasted time and more back and forth.

Remember that when you’re submitting a bug report, you’re often looking for help in getting this bug fixed. Be considerate of others and try to be as helpful as you can so that they can help you. Think of what information someone solving the problem might need. When in doubt, providing more information is generally better than providing less information.

---

## When asking for help

Don’t fall prey to [the XY problem](https://xyproblem.info/): asking about your attempted solution rather than your actual problem.

For those not familiar with the XY problem, the scenario usually goes something like this:

> - User wants to do X.
>
> - User doesn’t know how to do X, but thinks they can fumble their way to a solution if they can just manage to do Y.
>
> - User doesn’t know how to do Y either.
>
> - User asks for help with Y.
>
> - Others try to help user with Y, but are confused because Y seems like a strange problem to want to solve.
>
> - After much interaction and wasted time, it finally becomes clear that the user really wants help with X, and that Y wasn’t even a suitable solution for X.
>
> _Source: [The XY Problem](https://xyproblem.info/)_

Instead, take a step back and provide context when asking for help. What are you actually trying to solve and why? Then, what have you tried, and what was the result of those explorations?

Without providing the full context for your question, you risk getting help and advice for an attempted solution that won’t actually solve the root of your problem.

---

## When requesting that people do something

Be clear what action is required, by whom, by when.

Meetings can fail for a variety of reasons, particularly if the meeting participants aren’t aligned on what the purpose of the meeting is and what the expected outcome of the meeting should be. If a meeting is coming to a close but it’s unclear what the next steps are, don’t be afraid to be the one who asks “Okay, what are we doing next as a result of this discussion?”

Meetings also become unproductive when notes are taken but the action items are vague. (Or worse, notes aren’t taken at all!) If it’s not clear who owns each action item, the work most likely won’t be done. Instead, assign an owner by writing someone’s name next to each bullet point, and follow up during the next meeting or agreed upon deadline.

Similarly, when sending out announcements over Slack or email, be clear in your message if the information is just an FYI or if action is required.

---

## Conclusion

Everything outlined above is just a small handful of specific scenarios in which you might find yourself. There are sure to be others!

In all your communication, aim for respect and clarity, and try to find ways to reduce the cognitive load for the other person. Strive not just to be understood, but to be so clear that it is impossible to be misunderstood.
