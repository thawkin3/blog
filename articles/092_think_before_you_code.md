# Think Before You Code

Think before you code. I give this advice to every junior engineer I meet. Yet most younger developers have a hard time following it.

Taking the time to slow down and think will make you a more efficient engineer and a better troubleshooter and problem solver.

Let’s look at three scenarios where this is relevant: in job interviews, in bug fixing, and in feature development.

---

## Job Interviews

It’s a common mistake during whiteboard interviews to start coding before you fully understand the problem. The interviewer likely gave you a problem statement that intentionally or unintentionally had many details left out.

For instance, what assumptions can you make? What are your constraints? Can you assume that you will always be given valid input, or do you need to guard against invalid arguments? Will the dataset be small enough that you can take a brute force approach, or do you need to optimize your algorithm for specific space or time constraints? Ask questions until you feel that you fully understand the problem.

Next, what test cases should you consider? It’s often not necessary to write out fully functional tests during a coding exercise, but it’s a good idea to jot down the types of input you’d like to test and the expected output for each one. For example, if you are writing a function that determines if a string is a palindrome, you might want to have at least the following as your test inputs: `racecar`, `noon`, `dog`, `moon`, `42`. With those five test cases, you’re able to consider how your code should handle palindromes of even or odd length (`racecar` and `noon`), strings of even or odd length that are not palindromes (`dog` and `moon`), and invalid input (`42`). I’m sure you can think of other test cases to consider as well.

Finally, are you able to write some pseudocode to validate your approach at a high level? Before getting into the nitty gritty details of the actual code, it’s a good idea to think through how you might tackle the problem so you can catch issues in your approach early on.

---

## Bug Fixing

When troubleshooting an issue, inexperienced developers tend to panic. They begin frantically trying various things in an attempt to resolve the problem. This is a mistake. Pause and take time to think through the issue.

What do you know that changed between when the code was working and when it wasn’t working? If you upgraded the version of a dependency, what does the change log between the two versions look like? If the code works on your machine but not in a test environment, what’s different between the two environments? If yesterday’s release worked fine but today’s release has a bug, what commits were included in today’s release that might be the culprit? If you don’t know where to start looking for the root cause of the problem, who might you be able to ask that may have some insights into what’s happening?

Taking even just five minutes to think about the problem before you start trying to fix it may end up saving you hours in the long run.

---

## Feature Development

Developing new functionality should begin with a plan. What acceptance criteria do you need to meet in order to appropriately meet the user’s needs? Do you have test cases written out, either in actual executable code or bullet points to consider? If designing an API endpoint, what should the inputs and outputs be? If working with complex data, what type of data structure would best represent the data?

Spend time designing your code before jumping in. You may be in a hurry to start coding, but taking the time to plan will save you painful rewrites down the road.

---

## Conclusion

Junior engineers jump right into coding without fully understanding the problem or thinking through potential solutions first. So before you start frantically typing — stop. Think before you code.
