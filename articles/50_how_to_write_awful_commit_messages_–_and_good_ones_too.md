# How to Write Awful Commit Messages – And Good Ones Too

When is the last time you've looked through your repo's git history? Go ahead and do it right now using `git log`. What do you see? Meaningful commit messages? Or a tangled mess of unhelpful statements?

Code is read more frequently than it is written, and the same is true of commit messages. Engineers working on legacy code will rely on the commit messages from previous developers to gain much needed context on why the code was written the way it was.

Therefore, make your commit messages good.

In this article we'll learn how to write good commit messages by examining the exact opposite - truly terrible commit messages.

---

## The Vague Commit Message

```
fixing some stuff
```

Cool. So… what stuff were you fixing? Why were you fixing it? What problem were you trying to solve? What approach did you end up taking, and why?

I have no idea.

---

## The Code Review Feedback Commit Message

```
changes based on code review feedback
```

Ok. So I know these were some requested changes, and they must be related to earlier changes on this same merge request. But, like our last unhelpful commit message, what changes exactly did you make?

---

## The Exploratory Commit Message

```
testing something out
```

Neat. But what were you trying, exactly? What prompted this experimental change? What problem are you trying to solve? How did you solve it? What other approaches did you consider and then decided against?

---

## Principles for Good Commit Messages

As you can see, each of these three examples suffer from the same core problem: they're too vague. The person reading the commit message later is lacking information that may be useful in providing greater context for why the commit was made.

![Git Commit comic](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2t567uimaex6n1ip0k6t.png)
<figcaption>Git Commit comic (Source: https://xkcd.com/1296/)</figcaption>

---

There are plenty of guidelines out there for writing standardized commit messages. I personally enjoy writing commits according to the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification, but you should find something that works for you and your team. Regardless of the standard you choose, the following principles are the same:

- Standardize your commit messages so that everyone is formatting their commit messages in the same way.
- Specifically mention the file, package, or feature you are working on in your commit message.
- Use the body of the commit message to add additional context or reasons for why you did what you did.
- Reference the issue or task from Jira or whatever task management system your team is using.

Applying these principles while using the Conventional Commits guidelines, you might write a commit message that looks like this:

```
fix: corrects the url on user avatar link in nav bar

Before, clicking on the user avatar would take you 
to the home page rather than the user's profile 
page. Now it properly takes you to the user's 
profile page.

Closes !123456
```

Now someone viewing your commit later down the road has all the context they need to understand your change. They know what change you made to the code, why you made it, and they can find the issue in Jira to see any other conversations or notes made on the original ticket.

Much better!
