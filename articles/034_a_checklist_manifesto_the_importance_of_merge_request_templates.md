# A Checklist Manifesto: The importance of merge request templates

Filling out a checklist is often thought of as a mind-numbing rote task. But what if I told you that checklists are actually essential to getting the job done right?

Checklists have been proven to help reduce human errors in complex tasks such as flying airplanes or performing surgery (see [The Checklist Manifesto by Atul Gawande](http://atulgawande.com/book/the-checklist-manifesto/)).

So why not apply checklists to software engineering? In this article we'll examine the benefits of using merge request (or pull request) templates prior to merging new code.

---

## Merge Request Templates

Most software companies use git in conjunction with platforms like GitHub or GitLab to provide version control for their code.

It's common for the production code to live on the master (or main) branch. When developers are writing new features or fixing bugs, they create a branch off of the master branch, write their code, and then create a merge request asking to merge their new code into the master branch. Another developer would then review the code, approve (assuming the code looks good), and merge the new code.

Both GitHub and GitLab allow you to configure your repository to include a merge request template, which is basically just a form that a developer fills out when creating a merge request.

Merge request templates are optional, but I'd like to argue here that they are essential! A good merge request template helps create an engineering mindset of excellence and can help keep the quality of your code high.

A merge request template might look like this:

---

> **Description**: what are you changing and why

> **Ticket Info**: links to tickets in your task management system like JIRA or Workfront

> **Test Plan**: step-by-step instructions for how someone could verify that your code works

> **QA Risk**: low, medium, or high

> **Screenshots or Videos**: proof of your code working properly

> **Additional Checklist Items**:
[ ] I have written automated tests for this change
[ ] I have updated documentation as needed
[ ] The code works in all supported browsers

---

Your merge request template might include more or less than what I have above, but these six fields provide us with a solid foundation. Let's examine why.

---

## Description

First off, the description. This one is simple enough. Have you ever been asked to review someone else's code without being given any context as to why this code was written? Wouldn't it be helpful to know what problem this new code is trying to solve? I would think so.

And wouldn't you like to know why the engineer solved the problem the way they did or what alternatives they considered? I would.

The merge request description helps provide the context for the code review.

---

## Ticket Info

Just about every company uses some sort of work management system to track their work. It's especially helpful for software engineers to have a system where bugs can be logged and new features can be requested.

Having a "ticket info" field in your merge request simply provides a spot to include a link to the ticket that prompted this code to be written. That way someone reviewing the code could also take a look at the original bug report and any additional info the ticket contains. Keeping the ticket and the merge request linked makes sure nothing gets lost, especially if you need to revisit this several months or years later.

---

## Test Plan

The test plan field is my personal favorite. Having this field in the merge request template forces the developer making the merge request to think about how they would manually test the code to ensure that it works. Hopefully it would lead to them actually verifying that their code works as well, leading to higher quality and helping catch simple mistakes.

The test plan also gives the reviewer clear steps in how to verify that the code works properly in the event that they wanted to pull down the code and test it out locally on their machine.

*(Before anybody gets up in arms about the ineffectiveness of relying on manual testing for QA, don't worry, we'll cover automated tests later.)*

It also provides a little insight into what the developer who wrote the code thought was important to test. Maybe as the reviewer you can think of something that was missing from the test plan, which might be a good indicator that the developer who wrote the code may not have considered that case or written code to handle it properly.

---

## QA Risk

QA risk is essentially a subjective measure of how big this change is. If something goes wrong, what's the worst that could happen? Would a text input have the wrong border color? Or would your login or checkout process break? Understanding the potential impact of the code change can help prompt reviewers to be more thorough in their reviews.

---

## Screenshots or Videos

In the event that the code reviewer doesn't pull down the code to test locally themselves, asking the developer to include a screenshot or video of the code in action provides one more sanity check that things look ok. This field is particularly helpful for frontend changes that impact the UI, but it may not be as useful for backend changes.

---

## Additional Checklist Items

Finally, it can be helpful to provide one last checklist of anything else that your organization cares about. One common item might be to verify that the developer has written tests for their code. Having the checklist here is a reminder to the developer to write unit tests if they haven't already.

Another checklist item might be to update any documentation as needed. If you've changed the API of some component or modified an essential step in the build process that was previously documented in the README, you'll want to verify that the documentation still makes sense.

For UI development, if your company supports an older browser like IE11, having a checklist item about code working in all browsers is a good reminder to go double-check.

---

## Criticism

Now, some shortcomings. Can an engineer simply ignore the merge request template and choose not to fill it out? Yes.

Or could they blindly check off all the boxes in the final checklist without having actually completed any of the steps? Yes.

Ultimately, ignoring the checklist is no better or worse than not having the checklist at all. However, just the mere presence of the merge request template helps prompt engineers to be thorough in the information they provide.

---

## Conclusion

If you're still skeptical of the importance of checklists or how merge request templates can help, give it a try! I think you'll be surprised by the improvements you see.
