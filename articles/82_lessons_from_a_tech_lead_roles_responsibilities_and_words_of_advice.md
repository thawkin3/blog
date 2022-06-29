# Lessons from a Tech Lead: Roles, responsibilities, and words of advice

I’ve been a tech lead for several years at a few different companies. Each role has been somewhat unique, and yet there have been commonalities between each experience. In this article I’d like to share with you what a tech lead is, what they do, and some lessons that I’ve learned along the way.

---

## What is a Tech Lead

The first thing to know is that a tech lead is not a manager. A tech lead is still an individual contributor and is expected to be a high producer of work while also helping the team. The tech lead is usually a more senior member of the team, but they don’t always have to be. Since the tech lead doesn’t have any direct reports, they don’t have 1on1s with their team members or do performance reviews. They do, however, help mentor their teammates.

---

## Responsibilities of a Tech Lead

Tech leads wear many hats. They play the role of architect, project manager, software engineer, mentor, and teammate all at once.

Tech leads are responsible for helping drive the high-level architectural discussions regarding the work that the team is doing. They lead design meetings and tech breakdowns. They ask questions and try to poke holes in ideas to ensure that edge cases are covered.

Tech leads help organize the work by breaking down feature epics into stories and tasks. They may do this individually or with the rest of their team, depending on the company. They help prioritize the work and ensure that the right things get done at the right time.

Tech leads help remove blockers. This may involve chasing down answers to questions for the product owner, UX designer, or engineers from other teams. It may also involve working with a teammate to clarify some acceptance criteria on a ticket. It always involves doing code reviews on a daily basis and making sure pull requests don’t sit for too long without receiving attention.

Tech leads also help mentor their teammates and are responsible for helping level up the team. They ensure that best practices are implemented and followed. They teach through pair programming and through code reviews. They frequently share articles, advice, and ideas.

In short, being a tech lead is an exercise in influencing without direct authority.

---

## Lessons Learned

Now that we have a solid foundation for what a tech lead is and what they do, let’s look at some of the lessons I’ve learned throughout my experience.

---

### Prioritize the Needs of the Team

As a tech lead, you are no longer interested in only your own success. You’re invested in the success of the team as a whole. This means that the needs of the team outweigh the needs of the individual.

I usually spend the first 1–3 hours of my day reviewing all open pull requests, answering questions over Slack, and triaging new bugs. In short, I try to do everything I can to unblock my team members from anything they are waiting on. Only after all that is done do I start on my own work for the day. I’ll check Slack or do code reviews throughout the rest of the day as well, but only at natural stopping points so as not to interrupt my flow.

---

### Time Management

It’s very difficult to balance your time between your own work and the work of the team. Some days you’ll be on a “maker’s schedule” where you are heads down coding for large blocks of time. Other days you’ll be on a “manager’s schedule” where you have many meetings and interruptions without time for deep work. Most days will be a mix of both.

If you are always doing code reviews or answering questions from your teammates, you’ll have very little time to write code yourself. Learn to set boundaries and block off time on your calendar for interruption-free hours to focus on your own work. Find ways to make your team more self-reliant, and help them find answers to their own questions. Identify tasks that you can delegate.

---

### Delegation

It’s a mistake to try to do everything by yourself. The whole point of having a team is to share the load and to accomplish more together than any one individual could do on their own. If you try to bear that burden alone, you’ll surely burn out.

The tricky part is knowing [when to do something yourself and when to delegate it](https://betterprogramming.pub/how-to-effectively-delegate-tasks-as-a-technical-leader-b6d634497512).

---

### Process Improvement

If you find yourself constantly putting out fires for your team, take a minute to reflect on the root cause of those problems.

For example, are your team members asking the same questions repeatedly? Use this as an opportunity to document your processes so that you can point people to your wiki page the next time the same question comes up.

Do you find that you have to review every pull request on your team? Why is that? Take the time to set expectations as a team that every team member is responsible for reviewing code. Teach them what to look for, and walk them through your thought process. Document these code review best practices and guidelines as well. Offloading even a few code reviews will save you hours each week.

Are you the one that always gets pulled in to solve the hard problems when something goes wrong? Use these as teaching moments. Have a team member shadow you or pair program with you as you diagnose the problem and troubleshoot. Now, the next time this happens, you’ll have a second person on your team who is equipped to help.

---

### Optimize the Developer Experience

Look for things to optimize to make the development process easier and help your team become more productive.

Is there documentation missing? Create it.

Do people know how to run the app locally for any given repo? Document the steps in the README.

Are tests missing in an area that frequently breaks? Write those tests.

Is a CI pipeline slow? See if there are steps in the pipeline you can run in parallel, test suites with duplicate tests you can trim down, or other things you can do to speed it up.

Are there opportunities for training to level up your team? Work with each team member individually to identify gaps in their skill set and come up with a plan to help them fill in those gaps. 

---

### Learn to Give Hard Feedback

One of the hardest parts of being a leader is giving negative feedback. Learn to have hard conversations. As Kim Scott says in her book *Radical Candor*, you should care personally and challenge directly.

Ask good questions. Challenge your team members in a way that encourages them to grow.

If someone isn’t pulling their weight, work with them to identify the root cause and brainstorm solutions together. Is the problem a motivation issue or a skills issue? If it’s a skills issue, are there other tasks or projects they could work on that would be better suited for their skill set?

The line between tech lead and manager becomes a little blurred here, so it’s important to work closely with your manager in these situations. As a tech lead, you are not responsible for having performance conversations or for making firing decisions, but your input does have a significant impact. Do everything you can to help your team members be successful. Sometimes though, the kindest thing to do is to help someone move on.

---

### Make Good Judgment Calls

Your teammates will often look to you to offer advice or provide direction when the path forward is uncertain. Software engineering is full of tradeoffs as you weigh short-term and long-term consequences of technical decisions.

For example, should we write tests for this piece of functionality? In general, the answer is nearly always yes. But what if the code repo is legacy code that hasn’t been touched for five years, is using an outdated tech stack, and doesn’t have an existing test suite? Do you spend the time to set up test infrastructure for this code in addition to writing your specific tests, or do you skip writing tests in this situation?

Or, consider the following scenario: You’re about to release a new feature, but a bug is discovered right before the release cut-off. Do you delay the release of the code so that you can fix the bug? Or do you allow the bug to go to production? The answer will depend on things like the severity of the bug, how frequently you release code, and how quickly you think you can fix the bug.

The takeaway here is that you’ll frequently need to make hard decisions where there is not a 100% correct solution. Learn to use good judgment, make decisions with limited information, and stand by your decisions. When things go wrong, own your mistakes and find ways to learn from them.

---

### Always Keep Learning

Finally, don’t become complacent. You need to keep learning in order to continue to help your team level up. By setting an example of continuous improvement, you can create a culture of learning. Be sure to share the interesting articles and books you’re reading or the side projects you’re working on. These will provide a wealth of ideas for you to share with your team.

---

## Conclusion

The role of tech lead is a demanding one but one filled with opportunities for growth. To summarize, your job will go a lot more smoothly if you:

1. Prioritize the needs of the team over the needs of the individual

2. Manage your time wisely

3. Learn to delegate

4. Look for ways to improve your processes and documentation

5. Look for ways you can optimize the developer experience

6. Learn how to give hard feedback and how to have hard conversations

7. Trust yourself to make good judgment calls

8. Always keep learning

Thanks for reading!
